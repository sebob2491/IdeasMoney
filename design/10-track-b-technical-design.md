# Track B technical design: core, content, extractor, parity, simulation, UI, desktop, build

Status: design, 2026-11-01. Builds on `design/02-track-b-idle-game.md` (architecture and roadmap), `design/04-track-b-port-inventory.md` (extraction plus parity path, D-010), `design/00-studio-pipeline.md` (hooks, gates, issue sizes) and `research/02-build-and-test-methods.md` Sections B1, B3, B6, B9 and D. Setting and names follow the Dry River default (D-005). Everything here is design: type sketches and schema sketches, no implementations.

Facts about the legacy bundle quoted below were read from `azeroth-idle/index.html` (1.85 MB, one `<script>` wrapped in an IIFE, `(function(){"use strict";...})();`). Mangled identifiers such as `Y`, `Q`, `ka`, `Te` change with every rebuild, so the tools described here find them by pattern and never hardcode them. The bundle stays where it is (D-008).

## 1. Architecture overview, package contracts and dependency rules

The game is a pnpm workspace of five packages and two apps. The rule that makes the nightly pipeline possible is that `core` is a pure function of (content, seed, inputs, ticks): no DOM, no timers, no wall clock, no `Math.random`, no I/O. Everything else is a shell around that function.

| Package | Exports (public API surface) | Pure? | May import |
|---|---|---|---|
| `@dr/core` | `createGame`, `advance`, `applyCommand`, `selectors/*`, `formulas`, `save/*`, `Rng`, `EventLog`, state and command types | Yes; lint bans `window`, `document`, `Date`, `performance`, timers, `Math.random`, `fetch`, `crypto` | `@dr/content` types only, `zod` |
| `@dr/content` | `schemas/*` (Zod), `loadContent(json[]) -> ContentBundle`, `ContentBundle` type, `strings` loader, the base content JSON, `rename-map.json` | Yes (parsing only) | `zod` |
| `@dr/sim` | `runSim`, `runMany`, `policies/*`, `metrics/*`, `bands` loader, `goldens` comparer, `tune` loop | Yes except the CLI entry (`node:worker_threads`, `node:fs`) | `@dr/core`, `@dr/content` |
| `@dr/tools` | CLIs: `content-lint`, `forbidden-terms`, `legacy-extract`, `save-inspect`, `balance-report`, `parity` | No (Node CLIs) | `@dr/core`, `@dr/content`, `@dr/sim`, Playwright (parity and extractor pass C only) |
| `@dr/ui` | React components, `GameProvider`, hooks and selectors, theme tokens, i18n hook | No (React, DOM) | `@dr/core`, `@dr/content`, `react`, `zustand` |
| `apps/web` | Vite PWA entry, service worker, persistence adapters for browser storage | No | `@dr/ui`, `@dr/core`, `@dr/content` |
| `apps/desktop` | Electron main and preload, steamworks.js bridge, Steam Cloud adapter, packaging | No | `apps/web` build output, `@dr/core` (save types), `steamworks.js` |

Dependencies point downward only: `apps/* -> ui -> core -> content(types)`, `sim -> core`, `tools -> everything`. Enforcement is mechanical: ESLint `no-restricted-imports` per package, a `dependency-cruiser` check that fails on any upward or sideways edge, a Vitest test that loads every `core` module under a frozen `globalThis` proxy which throws on forbidden globals, and package `exports` maps that hide internals.

Two further contracts:

- **Content is data, core is code.** `core` never contains an id string from content. Starting content for a new game (the first camp, the first action) is named in `content/base/game.json`, not in code.
- **The UI never computes game logic.** A number shown on screen is either in state or the output of a `core` selector. The lint rule is a test that `ui` imports nothing from `core/formulas` directly; it goes through `core/selectors`.

## 2. Core

### 2.1 State model

The state is one JSON-serializable object tree. Quantities are integers (gold in copper, XP as integers, time in ticks). Ids are strings from content. Maps are plain objects keyed by id; the determinism rules in 2.5 forbid integer-like keys so JSON round trips preserve order.

```ts
type Tick = number;  // integer, 10 per second
interface GameState {
  schemaVersion: number; contentHash: string; seed: number; rng: RngState;
  tick: Tick; playtimeTicks: Tick; savedAtMs: number | null;   // savedAtMs is written by the shell, never read by core logic
  player: { hp: number; gold: number; combatXp: Record<"attack" | "defense" | "marksmanship", number> };
  skills: Record<SkillId, { xp: number; level: number; masteryXp: Record<ActionId, number> }>;
  inventory: { slots: Record<ItemId, number>; capacity: number };
  equipment: Record<EquipSlot, ItemId | null>;
  action: { skillId: SkillId; actionId: ActionId; durationTicks: Tick; remainingTicks: Tick; repeatsLeft: number | null } | null;
  combat: { areaId: AreaId; dungeonId: DungeonId | null; waveIndex: number;
            monster: { id: MonsterId; hp: number; nextAttackTick: Tick }; playerNextAttackTick: Tick; buffs: Buff[];
            autoEat: { itemId: ItemId | null; belowPct: number } } | null;
  world: WorldState;          // unlocked areas, cleared dungeons, shop stock cursors
  prestige: PrestigeState;    // watermarks, monsoon meter, purchased upgrades
  achievements: Record<string, { unlockedAtTick: Tick }>;
  stats: Counters;            // kills, deaths, actions by id, gold by sink
  settings: GameplaySettings; // auto-eat threshold and similar: things that change simulation results
  log: EventLogState;
}
```

Derived values (max HP, hit chance, action duration after tool tiers) are never stored; they come from `selectors`, which are memoized by (state slice identity). `GameplaySettings` is inside the state because it changes simulation outcomes; UI preferences (theme, text size) are outside the state entirely.

### 2.2 Tick scheduler and action timers

`advance(state, content, ticks, commands): GameState` is the only way time passes. It is pure and returns a new state with structural sharing (plain object spread and a small `produce` helper, no immer dependency).

Per tick, systems run in a fixed order, and commands scheduled for that tick run first:

1. Commands at this tick (start action, stop, equip, buy, sell, eat, travel, enter dungeon, prestige, set settings). Commands carry the tick they apply to, so replay is exact.
2. Action timers: `remainingTicks -= 1`; at zero, complete the action (consume inputs, roll outputs and bonus drops, grant XP and mastery) and restart it if repeats and inputs remain; at most 50 completions per tick (the legacy loop has the same `r++<50` guard).
3. Combat: attack timers for player and monster, damage, death and respawn, auto-eat.
4. Buffs and regeneration.
5. Prestige meter, achievements (checks run only when a relevant counter changed in this tick, tracked by a dirty set).
6. Event log flush for this tick.

Durations are integers in ticks: `durationTicks = max(1, round(baseSeconds * 10 * speedMultiplier))`. The legacy kept fractional seconds with a `while (t >= duration)` loop at a 0.5 s sub-step, so timer parity is checked as actions completed per hour, not per-action milliseconds (Section 5). Legacy defaults carried into content: 3 s gathering, 2.5 s crafting (`time:n.time||(o.kind==="gather"?3:2.5)`), 4 s fishing, a 100 ms interval.

The shell drives the clock with an accumulator, `ticksDue = floor((now - lastTickAt) / 100)`, calling `advance` directly up to 6,000 ticks and taking the offline path (2.6) above that. The UI loop is `requestAnimationFrame`; the tick loop is `setInterval(100)`, never tied to frame rate; a Web Worker is deferred (Q-B6).

### 2.3 Seeded PRNG and stream separation

Generator: **xoshiro128\*\*** (128-bit state, period 2^128 - 1, four `uint32` words, no BigInt), each stream seeded by `SplitMix32(masterSeed ^ fnv1a(streamName))`. The legacy uses mulberry32 seeded by FNV-1a string hashes for some rolls (`me(we("ascend:"+name))`) and `Math.random` elsewhere; mulberry32's 2^32 period is too short for Monte-Carlo over hundreds of seeds.

Streams are named and fixed; adding a stream is a save migration:

| Stream | Used for | Why separate |
|---|---|---|
| `drops` | Action outputs, bonus drops, monster loot | Drop-rate tests must not be perturbed by combat rolls |
| `combat` | Hit, crit, evasion, damage variance | Combat parity over N fights with fixed gear |
| `world` | Shop restock rolls, random events, dungeon wave composition | Rare, and the UI may preview them |
| `quality` | Crafting quality and affix rolls | Affix tables change often |
| `sim` | Reserved for policies in `sim` (random policy), never used by core | Policy randomness must not change game randomness |

```ts
interface RngState { [stream: string]: [number, number, number, number] }
interface Rng { next01(stream): number; int(stream, lo, hi): number; chance(stream, p): boolean; pick<T>(stream, arr: readonly T[]): T; weighted<T>(stream, items: readonly {w: number; v: T}[]): T }
```

Rule: a roll happens only in the system that owns the stream, and the number of rolls per event is fixed by content shape, never by UI state; roll order is part of the save contract.

### 2.4 Event log

A ring buffer of typed events (2,000 entries, kept in state so it survives save and load) plus a sink callback for the shell and the sim. Every change a player could notice emits an event; the activity feed, the offline summary, achievements and sim metrics all consume the same log.

```ts
type GameEvent =
  | { t: Tick; k: "action.done"; actionId: ActionId; outputs: [ItemId, number][]; xp: number }
  | { t: Tick; k: "level.up"; skillId: SkillId; level: number }
  | { t: Tick; k: "combat.kill"; monsterId: MonsterId; loot: [ItemId, number][]; gold: number }
  | { t: Tick; k: "gold"; delta: number; sink: "shop.buy" | "shop.sell" | "repair" | "upgrade" | "loot" | "quest" }
  | { t: Tick; k: "item"; itemId: ItemId; delta: number; src: string }
  | { t: Tick; k: "cmd"; cmd: Command }   // commands are logged too, so save plus log is a replay
  | { t: Tick; k: "combat.death" | "prestige" | "achievement"; id: string };
```

A bug report is `seed + content hash + command log`, and `replay(seed, content, commands, untilTick)` rebuilds the exact state. The save inspector CLI prints the last 200 events of any save.

### 2.5 Determinism rules and how they are tested

Rules (each is enforced by a lint rule or a test, not by convention):

1. No wall clock, timers, randomness or I/O in `core` (ESLint `no-restricted-globals` plus the frozen-globals test).
2. All time is integer ticks; no floating-point accumulators cross a tick boundary. Formulas that produce fractions round at a defined point with `formulas.round*` helpers; `Math.round` is banned outside `formulas.ts`.
3. Map keys are content ids matching `^[a-z][a-z0-9_]{1,63}$`, never integer-like, so JSON preserves insertion order; outcome-affecting iteration uses content order, not `Object.keys` of state.
4. No `Set` or `Map` in persisted state (JSON round trip would reorder or drop them).
5. Every random draw names its stream. `Math.random` is banned by lint.
6. Chunk invariance: `advance(s, c, n)` equals `advance(advance(s, c, k), c, n - k)` for every k, which makes offline chunking irrelevant to results.

Tests:

| Test | Oracle |
|---|---|
| Replay equality | Run a 10-hour scripted session twice from the same seed; `hash(state)` equal at every 1,000 ticks |
| Chunk invariance (property) | fast-check generates split points; final hashes equal |
| Save round trip mid-run (property) | serialize at a random tick, deserialize, continue; equals the uninterrupted run |
| Cross-runtime | The same replay in Node and in Chromium yields the same hash; guards `Math.pow` with fractional exponents in the XP curve |
| Roll-order freeze | A golden file of the first 1,000 draws per stream for seed 1; human-only to update |

### 2.6 Offline progress

Algorithm, run by the shell on load or on returning from a long tab sleep:

1. `gapMs = now - savedAtMs`; under 60 s, advance normally (the legacy uses 90 s, and a chunked catch-up box only above `Jw=1800` s; the port keeps one path).
2. `simTicks = min(gapMs / 100, capTicks)` with `capTicks = 24 h` (864,000 ticks). Time beyond the cap accrues rested XP only, as the legacy does with its `uncounted` time.
3. Set `state.offline = true` and advance in chunks of 6,000 ticks, yielding to the UI between chunks with a progress callback. The legacy yields every 30 ms of wall time (`PA(a,30,now)`); chunking by ticks keeps it deterministic.
4. Fold the events emitted during the catch-up into an `OfflineSummary` (items, XP, levels, kills, deaths, gold, time lost after the activity stopped), show it, clear `offline`.

What may happen offline (`state.offline` is read by exactly one function, `offlinePolicy`, so the rule is auditable):

| Allowed | Not allowed |
|---|---|
| The active skill action continues while inputs last; it stops on the first missing input and the summary records the stop tick | Starting, switching or re-choosing an action; auto-selling; buying |
| Combat continues in the current area with auto-eat; when food runs out and HP drops below the flee threshold the player flees, and the remaining time is "lost" (reported) | Entering a dungeon, travelling, equipping, prestige, spending gold |
| Level-ups, mastery, achievements, prestige meter growth | Random world events and shop restocks that the player should witness (they roll on return, from the `world` stream, once) |
| Death: at most one death offline; the run stops there | Repeated deaths draining food and gold |

Loading the same save with the same `now` twice yields the same state; the sim reuses `offlinePolicy` for the idle-versus-active ratio.

### 2.7 Save format, versioning, migrations, compression

```ts
// Envelope written to storage. The game state inside is validated by the state Zod schema on load.
interface SaveEnvelope {
  magic: "DRYRIVER";
  schemaVersion: number;        // integer; bumps on any shape change of GameState
  gameVersion: string;          // semver of the build that wrote it, informational
  contentHash: string;          // base content hash at save time
  packs: { id: string; version: string; hash: string }[];   // active data packs, see Section 10
  savedAtMs: number;
  playtimeTicks: number;
  checksum: string;             // crc32 of the canonical JSON of `state`
  state: GameState;
}
```

Migrations are a registry `migrations: Record<number, (prev: unknown) => unknown>` (key n upgrades n to n+1), applied in order and followed by schema validation. Every past version has a fixture save under `packages/core/test/fixtures/saves/v<n>.json` (human-only path); a test migrates each to the current version and checks invariants (no negative quantities, levels match XP, equipment references existing items). The legacy's `OR` function, which remaps skill XP from an old `40*n^2.1` curve to the new one, is such a migration embedded in game code; the port keeps that logic in `save/migrations/` only.

Compression: the stored copy is plain JSON (a late-game save is projected under 300 KB). The **export code** is `DR1.` plus base64url of deflate-raw canonical JSON (`CompressionStream` in browsers, `node:zlib` in tools), with an uncompressed `DR0.` fallback. The legacy's save-code textarea is the same idea and doubles as an injection path for the parity harness.

### 2.8 Persistence targets and error handling

| Target | Role | Notes |
|---|---|---|
| `localStorage` | Primary on web: `dryriver:save:<slot>`, `dryriver:index`, `dryriver:lease:<slot>` | 5 MB budget; same shape as the legacy (`azeroth_idle_save:index`, `:s:<id>`, `:own:<id>`, `:probe`) |
| IndexedDB | Fallback when `localStorage` is unavailable or the save exceeds 1 MB; always holds the rolling backups (last 5 saves and one per calendar day for 7 days) | Written through the same `StorageAdapter` interface |
| File export and import | `.dryriver.json` download and drag-drop import; save code copy and paste | Desktop also writes `userData/saves/` |
| Steam Cloud | `steamworks.js` `cloud.writeFile` and `readFile` of the envelope per slot, on save and on quit, with conflict resolution by `playtimeTicks` then `savedAtMs` | Section 8 |

Multi-tab: a lease `{tabId, atMs}` renewed every 5 s with a 15 s expiry, as the legacy does (`qx=15e3`, `iL=5e3`); a tab that loses it stops saving and shows a banner; `BroadcastChannel` announces takeovers.

Error handling: a write failure shows a persistent banner and retries with backoff, never stopping the game; a read failure tries backups newest first; a save that fails validation is quarantined under `dryriver:quarantine:<ts>`, never deleted; when `core` meets an id missing after a content change it clears the action and logs an `error` event (the legacy does the same: "That recipe has moved. Pick it again"); a write never regresses `playtimeTicks` without the player's confirmation.

## 3. Content

### 3.1 Schemas (Zod sketches, field level)

All schemas live in `packages/content/src/schemas/`. Shared pieces: `Id = z.string().regex(/^[a-z][a-z0-9_]{1,63}$/)`, `Ref<T> = Id` with a `.describe("ref:item")` tag the linter reads to know which collection a field points at, `Qty = z.number().int().min(1)`, `Chance = z.number().min(0).max(1)`, `Seconds = z.number().positive()`.

```ts
const Drop = z.object({ item: Ref("item"), chance: Chance.default(1), min: Qty, max: Qty });
const Skill = z.object({ id: Id, kind: z.enum(["gathering", "crafting", "combat", "support"]), order: z.number().int(), maxLevel: z.number().int().default(60) });

const Action = z.object({ id: Id, skill: Ref("skill"), group: z.string().optional(),
  levelReq: z.number().int().min(1), baseSeconds: Seconds, xp: z.number().int().min(0),
  inputs: z.record(Ref("item"), Qty).default({}),
  outputs: z.array(Drop).min(1), bonus: z.array(Drop).default([]),
  risk: z.object({ failChance: Chance, stunSeconds: Seconds, salvage: z.boolean() }).optional(),
  tool: Ref("item").optional() });

const Item = z.object({ id: Id, type: z.enum(["material", "food", "potion", "gear", "tool", "currency", "key"]),
  tier: z.number().int().min(0), quality: z.number().int().min(0).max(5), sell: z.number().int().min(0),
  stack: z.number().int().min(1).default(1_000_000), icon: z.string(),
  gear: z.object({ slot: z.enum(EquipSlots), ilvl: z.number().int(), armorClass: z.enum(["cloth","leather","mail","plate"]).nullable(),
    levelReq: z.number().int(), stats: z.record(StatKey, z.number()), speed: Seconds.optional() }).optional(),
  food: z.object({ healPct: z.number() }).optional(), potion: z.object({ buff: StatKey, amount: z.number(), seconds: Seconds }).optional(),
  tool: z.object({ skill: Ref("skill"), effect: z.enum(["speed","yield","risk"]), tiers: z.array(z.object({ name: z.string(), per: z.number(), cost: Cost })) }).optional(),
  legacyId: z.string().optional() });

const Recipe = Action.extend({ station: Ref("item").optional() });  // an Action of a crafting skill; alias kept for the gear-coverage rule

const Monster = z.object({ id: Id, level: z.number().int(), family: z.enum(["beast","humanoid","undead","elemental","construct","spirit","giant","insect","aberration"]),
  attack: z.enum(["melee","ranged","magic"]), rank: z.enum(["normal","elite","boss","raid"]),
  stats: z.object({ hp: z.number().int(), dmg: z.number(), speed: Seconds, armor: z.number().int(), evasion: z.number().int() }).optional(), // omitted: derived by formulas.monster*
  xp: z.number().int().optional(), gold: z.tuple([z.number().int(), z.number().int()]).optional(),
  drops: z.array(Drop), element: z.enum(["fire","cold","lightning","none"]).default("none") });

const Area = z.object({ id: Id, levelBand: z.tuple([z.number().int(), z.number().int()]), monsters: z.array(Ref("monster")).min(1),
  unlock: z.object({ skill: Ref("skill"), level: z.number().int() }).or(z.object({ dungeon: Ref("dungeon") })).or(z.object({ always: z.literal(true) })), order: z.number().int() });

const Dungeon = z.object({ id: Id, kind: z.enum(["dungeon","raid","delve"]), levelReq: z.number().int(), waves: z.array(z.array(Ref("monster"))).min(1),
  boss: Ref("monster"), needs: z.array(z.object({ item: Ref("item"), qty: Qty })).default([]), rewards: z.array(Drop), unlocks: z.array(Ref("area")).default([]) });

const ShopEntry = z.object({ id: Id, item: Ref("item"), buy: z.number().int().min(1), stock: z.number().int().nullable(), restockHours: z.number().nullable(),
  unlock: z.object({ skill: Ref("skill"), level: z.number().int() }).optional() });

const Achievement = z.object({ id: Id, kind: z.enum(["level","kill","gather","craft","gold","dungeon","prestige","misc"]),
  target: z.object({ ref: Id.optional(), count: z.number().int().min(1) }), hidden: z.boolean().default(false), points: z.number().int().default(10) });

const PrestigeUpgrade = z.object({ id: Id, track: z.enum(["gathering","crafting","combat","camp"]), tier: z.number().int(), cost: z.number().int(),
  requires: z.array(Ref("prestige")).default([]), effect: z.object({ stat: StatKey, value: z.number(), per: z.enum(["flat","pct"]) }), maxRank: z.number().int().default(1) });

const Strings = z.record(z.string().regex(/^(skill|action|item|monster|area|dungeon|shop|achievement|prestige|ui)\.[a-z0-9_]+\.(name|desc|flavor|pieces\.\d+)$/), z.string());
```

### 3.2 Ids, references, loading, strings

- Ids are `snake_case`, unique within a collection; references are typed by field, so `item.copper_ingot` and `monster.copper_ingot` cannot be confused. Content JSON never holds display text: `name`, `desc` and `flavor` live in `strings/<locale>.json` under keys such as `item.copper_ingot.name`; a missing string in another locale falls back to `en`, never to the id.
- Loading: `loadContent(files) -> ContentBundle` parses each file with its schema, builds indexes (`byId`, `actionsBySkill`, `itemsDroppedBy`), computes `contentHash` (sha256 of canonical JSON) and freezes the result; array order in JSON is semantic and preserved.
- Formulas stay in `core/formulas.ts`; content holds inputs to formulas (level, tier, rank), never computed values, unless a `stats` override is present, which the linter flags with an `info` so overrides stay rare and deliberate.

### 3.3 Content linter rules

Run by `pnpm content:lint`, in the PostToolUse hook (fast rules) and CI (all rules). Each rule has an id, a severity, and a one-line fix hint in its output.

| Rule | Severity | Check |
|---|---|---|
| L001 schema | error | Every file parses with its Zod schema; unknown keys fail |
| L002 unique-id | error | No duplicate id within a collection |
| L003 dangling-ref | error | Every `Ref` field resolves in the named collection, including inside strings keys |
| L004 naming | error | Ids match the regex; string keys match the Strings regex; no digits-only ids; no setting-specific words banned by the forbidden-terms list |
| L005 strings-coverage | error | `name` for every entity, `desc` for items and skills, in `en` |
| L006 obtainable | error | Every item is reachable in a graph walk from the level-1 starting kit through action outputs, bonus drops, monster drops, dungeon rewards and shop entries whose inputs are themselves reachable (no circular-only items) |
| L007 sink | warning | Every `material` is consumed by at least one recipe, shop sale with `sell > 0`, upgrade cost or dungeon `needs`; materials with only a vendor sink are reported as `info` |
| L008 gear-coverage | warning | For every level band of 10 and every armor class, at least one obtainable gear piece per slot has `levelReq` in the band; weapons per band for each combat skill |
| L009 gold-flow | warning | Per band, estimated gold inflow per hour (sells and drops) over band sinks (shop buys, upgrades) is within `[0.5, 2.0]`, via a sim-free estimator over `core` formulas |
| L010 level-ladder | warning | Each skill has at least one new action every 10 levels up to `maxLevel`, and action `levelReq` values are non-decreasing in file order |
| L011 drop-sanity | error | Chances in `[0, 1]`, `min <= max`, the sum of exclusive output chances is at most 1 |
| L012 orphan-string | info | Strings keys that reference no entity |
| L013 legacy-provenance | info | Entities with `legacyId` whose legacy counterpart no longer exists in the latest extractor report |

Forbidden terms are a separate CLI with a list of every Blizzard and Warcraft name from the port inventory and more (`Azeroth`, `Alliance`, `Horde`, `Elwynn`, `Westfall`, `Deadmines`, `Durotar`, `Stormwind`, race, class, zone and boss names), case-insensitive with word boundaries and no allowlist for common words (`horde` as plain English is out too). It scans content, strings, UI source and docs, excluding the root `index.html` and `packages/content/legacy-draft/`.

### 3.4 Rename map

`packages/content/rename-map.json` maps legacy ids (which the legacy derives from names by slugifying: `Ee=e=>e.toLowerCase().replace(/[^a-z0-9]+/g,"_")...`) to new ids and display strings. It is the only file where legacy names appear in the committed tree, and it is excluded from the forbidden-terms lint by path (and only by path).

```ts
interface RenameMap {
  version: number;
  collections: Record<"skill" | "action" | "item" | "monster" | "area" | "dungeon",
    Record<string /* legacyId */, { id: string; name: string; desc?: string; status: "accepted" | "draft" | "dropped"; note?: string }>>;
  // Example: "mining": { "id": "prospecting", "name": "Prospecting", "status": "accepted" }
  //          "copper_ore": { "id": "copper_ore", "name": "Copper Ore", "status": "accepted" }   (neutral names may stay)
  //          "kobold_vermin": { "id": "dust_wraith", "name": "Dust Wraith", "status": "draft" }
}
```

The extractor applies the map when writing drafts; the linter fails on any `draft` or `dropped` entry that is referenced by accepted content.

### 3.5 Legacy-draft review workflow

1. `legacy-extract` writes `packages/content/legacy-draft/` (Section 4), gitignored except its README and the report.
2. `content promote --collection item --ids ...` copies drafts into `packages/content/base/`, applies the rename map, reduces provenance to `legacyId` and generates strings keys.
3. The content linter and forbidden-terms lint run; failures go back to the rename map, never fixed by hand in the promoted file.
4. The PR body lists promoted ids and the extractor report hash; the reviewer checks the names read as Dry River, not as a find-and-replace.
5. The weekly digest carries a names card: every `draft` entry waits for a yes, a rename or a drop; only `accepted` entries ship in the public alpha.

## 4. Legacy extractor design

The port inventory counted 1,041 `id` and 1,476 `name` fields. Probing shows the content is in three shapes, so the extractor has three passes and a reconciliation step. The probe at `scratchpad/probe-extract.js` found 1,035 `{id:"..."}` literals, 878 parse standalone, 157 reference identifiers.

**Pass A, id literals.** Scan for `{id:"<slug>"` starts, extract with a brace-balancing cursor that is string-aware (double, single and template quotes, escapes) and capped at 20 KB, then evaluate with `new Function("return (" + literal + ")")` inside a `node:vm` context with no globals. Failures are referenced identifiers. Of the 157, the probe shows `I:35, at:17, Pa:11, Ts:6, hl:5` and a tail of singletons. Resolution:

1. Collect identifier candidates from the error (`X is not defined`) iteratively until the literal evaluates or an identifier cannot be resolved.
2. For each identifier, find its definition in the bundle: `const <ident>=`, `,<ident>=` or `;<ident>=` followed by a literal (number, string, array or object, extracted by the same balancer). `hl=300` and `Pa=["head","shoulders",...]`, `Ts=["weapon"]` are such literals and are substituted textually into the vm context as `const hl=300;`.
3. If the definition is a function (`I=(e,t,a)=>n=>...`, `at`, `_t`, `wa`, `Ft`, `Kd`), the field is code, not data: the extractor records `{"$code": "<source>", "$ident": "I"}` and marks the record `needsDecoder`. Affix roll functions are a known family; a decoder table maps `I(stat, lo, hi)` to `{stat, range: [lo, hi]}`, `Ft` to one-decimal ranges, `Kd` to resistance triples.

**Pass B, builder tables.** The main tables are not id literals: the bundle registers skills with `ka(id, name, kind, desc)`, actions with `Te(skillId, name, levelReq, {inputs, outputs, by, group, time, exp, base, risky, gold, stun})`, zones with `rt(id, name, minLevel, maxLevel, expansion, monsterRows)`, dungeons with `be(id, name, levelReq, expansion, kind, monsterRows, {needs, note, pace})`, monsters as positional rows `[name, level, family, attackType, rank?, drops?, opts?]` decoded by `ml`, materials with `ll(name, sell, quality, extra)` and its wrappers `kv` (food), `bE` (potion), `vE` (elixir), and gear with `Dd(name, slot, ilvl, quality, armorClass, opts)`. Most calls are `forEach` over compact arrays: `[["Copper Vein",1,"Copper Ore",[1,2],[["Rough Stone",.4],...]],...].forEach(([e,t,a,n,o,s])=>Te("mining",e,t,{group:"Veins",outputs:{[a]:n},by:o,exp:s}))`. The pass finds each builder by the shape of its body (the function containing `acts:[]` is the skill registrar), extracts every call site and `forEach` table with the bracket balancer, evaluates the arrays in the vm and applies a **decoder** per builder that names positional fields. Decoders are written once by an agent reading the builder body and stored in `tools/legacy-extractor/decoders/<builder>.ts` with the body's hash; a rebuild that changes the body fails the hash check and forces a re-review. Counts from the current bundle: 12 skills, 19 zones, 37 dungeon-kind entries, about 493 monster rows, 13 direct `Te` calls plus eight `forEach` tables.

**Pass C, runtime dump.** The script is an IIFE, so the module registries (`Y` items, `Q` skills, `j` zones, `V` dungeons, located by patterns such as `Q[e]={id:e,name:t,kind:a,desc:n,acts:[]}` and `j.push({id:e,name:t,min:a,max:n,exp:o,mons:`) are unreachable from `page.evaluate`. The extractor writes an instrumented copy to the scratch directory, never to the repo: the same bytes with `;globalThis.__legacy={items:Y,skills:Q,zones:j,dungeons:V};` inserted before the final `})();</script>`, loads it in Playwright and dumps the registries as JSON. That is the ground truth for computed fields: gear `sell` (`yo=(ilvl,q)=>Math.round((ilvl*ilvl*.8+10)*wr[q])`), `req` (`rm(ilvl)`), monster stats (`ya=L=>30+8L+1.1L²` HP, `fl=L=>2+1.2L+.02L²` damage, times rank multipliers), and player combat (accuracy `max(.6, min(1, 1.2·acc/(acc+L)))`, evasion `min(.6, ev/(ev+400+85L))`, armor reduction `armor/(armor+400+85L)`). Formulas found next to the registries go to `report/formulas.md` as candidates for `core/formulas.ts`.

**Reconciliation and grouping.** Records from A and B are matched to C by id (the slug of the name). Pass A literals are grouped first by containing array variable (`vr=[{id:"stoneward"...`, about 50 containers) and then by key-set signature (`[id,name,prof,effect,max,per,tiers,desc,cost]` is tools; `[id,name,speed,dmg,eff,hits,on,cls,at,text,wants]` is combat skills; `[id,cat,title,keys,lead,body,tip,unlock]` is tutorial pages). An agent names each group once in `decoders/groups.json`; unknown signatures land in `unsorted/`.

Output layout:

```
packages/content/legacy-draft/
  README.md  report.json  report.md     committed: purpose and regeneration; counts, hashes, unresolved identifiers, decoder mismatches
  a-literals/<group>.json              raw literals with provenance {offset, container, keySet}
  b-tables/<collection>.json           decoded builder tables with provenance {builder, callOffset}
  c-runtime/<registry>.json            dumped registries
  merged/<collection>.json             one record per legacy id: {legacyId, legacyName, fields..., sources, conflicts}
  renamed/<collection>.json            rename map applied; input to `content promote`
  unsorted/*.json
```

`report.json` holds `{bundleSha256, extractedAt, literals: {found, parsed, resolved, unresolved: [{ident, count, sample}]}, tables: {byBuilder: {name, bodyHash, decoderHash, calls, rows}}, runtime: {registries}, merged: {byCollection: {count, withConflicts, renamed, draft, dropped}}}`. The nightly run diffs it against the committed copy and opens an issue when counts change (a new legacy upload) or a decoder hash mismatches.

Rules: the folder is gitignored except `README.md` and the two reports; the forbidden-terms lint excludes it by path; nothing imports from it; the content build fails if any file outside it contains a `legacyName` field.

## 5. Parity harness design

The harness lives in `packages/tools/parity/` and runs under Playwright test. It treats the legacy build as the reference implementation (golden-master testing, research B7).

**Serving.** A static server (`sirv`) serves the repository root so `http://127.0.0.1:4173/index.html` is the legacy game. Google Fonts requests are blocked by route (`page.route("**/fonts.g*", abort)`) for speed and determinism; the page falls back to system fonts.

**Save key discovery.** The legacy prefix is `azeroth_idle_save` with `:index`, `:s:<slotId>`, `:own:<slotId>` and `:probe`, but the fixture does not hardcode it: it diffs `Object.keys(localStorage)` around character creation, reads the `:index` record for the active slot and parses `:s:<slot>` (`{v:"0.1.0", acct, char, savedAt}`) against a Zod sketch `LegacySave`, so a shape change in a new legacy upload fails loudly.

**Scripting actions against a minified UI.** Class names are unowned; the dependable hooks are the `data-a="verb|arg"` attributes the legacy uses for event delegation (344 occurrences): `start|<race>|<class>`, `view|<page>`, `node|<actionId>`, `craft|<recipeId>`, `fight|<monsterId>`, `zone|<areaId>`, `dungeon|<id>`, `equip|<itemId>`, `sell|<itemId>`, `stop`, `save`, `export`, `import`, plus `data-skill`, `data-item`, the `#savebox` textarea and `role="status"` on the catch-up box. Selectors are `page.locator('[data-a="node|copper_vein"]')` with text fallbacks (`getByRole("button", { name: "Mining" })`), since skill and item names are stable DOM text. A `LegacyDriver` wraps them: `newGame()`, `startAction(legacyId)`, `startFight(legacyMonsterId)`, `equip(legacyItemId)`, `stop()`, `save()`, `readSave()`, `importSave(envelope)`; its legacy ids come from `legacy-draft/merged/`.

**Speeding up time.** Two modes:

- *Clock mode.* `page.clock.install()` before load, then `page.clock.runFor(ms)` in steps of 100 ms to 1 s. The legacy loop is `setInterval(100)` reading `Date.now()` through one helper, so fake timers advance both consistently, and its 0.5 s sub-stepping costs one or two sub-steps per step. Target: one simulated hour in under 20 s wall time.
- *Catch-up mode.* Save, move `savedAt` back by N seconds, reload; the legacy runs its own capped, chunked fast-forward and shows a `role="status"` box the driver waits on. This also puts the legacy offline path itself under parity.

Randomness: the legacy seeds some rolls from strings and uses `Math.random` elsewhere, so parity on stochastic outcomes is statistical, never exact. An `addInitScript` installs a seeded `Math.random` so reference traces are reproducible run to run.

**What to read from the save.** `char.skillXp[skill]`, `char.playtime`, bank contents, equipped items, gold, `char.hp`, kill and death counters, and the away-summary fields when catch-up ran. The new core replays the same session through `sim` with renamed ids (`parity/sessions/*.json`, a list of `{atSeconds, command}`); both traces are written as `ParityTrace` JSON.

**Metrics and tolerances per ported system.**

| System | Metric | Procedure | Tolerance |
|---|---|---|---|
| Skill action timing | Actions completed per simulated hour at fixed tool tier | Clock mode, 1 hour, 3 actions per skill | Within 1% (integer-tick rounding versus fractional seconds) |
| XP per action and curve | XP after N completions; level at fixed XP values | Read `skillXp`; compare `formulas.levelForXp` against legacy level labels at 20 XP points | Exact |
| Drop rates | Frequency of each output and bonus drop over N = 2,000 completions | Count bank deltas | Each rate within a 99% binomial confidence interval of the content value; also chi-square p > 0.01 against the table |
| Recipe outputs | Inputs consumed, outputs produced, fail and salvage rates | N = 1,000 crafts | Consumption exact; rates as above |
| Combat | Kill time distribution, damage taken per fight, deaths, gold and XP per kill, over N = 500 fights with fixed gear at three levels | Both engines in catch-up or sim mode | Means within 3%; kill-time p50 within 5%; death count within binomial CI |
| Offline | Items, XP and gold from a 2, 8 and 23-hour gap on identical saves | Catch-up mode | Within 2% of the online run of equal length, both engines |
| Sell prices and gear requirements | Table compare of every gear item | From pass C dump | Exact |
| Monster stats | HP, damage, armor, evasion, XP, coins per monster | Pass C dump versus `formulas.monster*` | Exact |

**Cadence and gating.** Exact-table comparisons run per PR; stochastic suites run nightly and on the `parity` label. A system's `features.json` entry becomes `ported: true` after three consecutive green nightlies with result files attached to the issue; a red nightly reopens it with the diff. Results live under `packages/tools/parity/results/` (gitignored) plus a committed `parity-status.json` the planner reads. A deliberate divergence (a rebalance) moves the metric from parity to the sim bands and is recorded in `DECISIONS.md`.

## 6. Simulation (`sim`)

**Runner API.**

```ts
interface SimOptions { content: ContentBundle; seed: number; hours: number; policy: Policy;
  decisionEveryTicks?: number /* default 100 = 10 s */; sampleEveryTicks?: number /* default 36_000 = 1 h */;
  milestones: MilestoneDef[]; offlinePattern?: { onlineHours: number; offlineHours: number } /* default 2 on, 6 off */ }
interface Policy { id: string; decide(view: ReadonlyState, api: PolicyApi, tick: Tick): Command[] }
interface SimResult { seed: number; policy: string; milestones: Record<string, { tick: Tick; activeHours: number } | null>;
  series: { tick: Tick; levels: Record<SkillId, number>; gold: number; invValue: number }[];
  flows: { items: Record<ItemId, { in: number; out: number }>; gold: Record<string /* sink */, number> };
  idleActiveRatio: number; perf: { ticks: number; wallMs: number } }
runSim(opts): SimResult;  runMany(opts, seeds: number[], workers?: number): SimResult[];  summarize(results): Stats;
```

Policies decide every 10 simulated seconds against a read-only view (same selectors the UI uses), which keeps per-tick cost near zero. The runner uses event skipping: between decisions it asks `core` for `nextInterestingTick` (the minimum of all timers) and advances in one call, which is sound because of chunk invariance.

| Policy | Behaviour | Role |
|---|---|---|
| `greedy-xp` | Always the action with the highest XP per hour it can afford | Upper bound on skilling pace |
| `balanced` | Rotates to the lowest-level skill whose next action is unlocked; fights when gear allows | The "typical player" baseline for bands |
| `combat-first` | Fights the highest-XP monster it beats with less than 20% death risk; skills only for food | Combat pacing and food economy |
| `min-max` | Looks ahead: picks actions by marginal value toward the next milestone (gold-weighted) | Lower bound on milestone times |
| `random` | Uniform choice among legal commands, from the `sim` stream | Finds dead ends and exploits |
| `persona-llm` (optional, offline only) | A model given the same view and a persona prompt | Discovery, never in CI |

**Metrics.** Time (ticks and active hours) to each milestone in `bands/milestones.json`; per-hour resource flows and net gold by sink; idle-versus-active ratio from the offline pattern; sink coverage (materials with zero `out` over the run); deaths and food consumed per hour; shop utilisation.

**Bands file.** `packages/sim/bands/milestones.json`:

```json
{ "first_dungeon_clear": { "policy": "balanced", "unit": "activeHours", "p50": [4, 6], "p90Max": 9, "rationale": "design 02 section 4" },
  "all_skills_50":       { "policy": "balanced", "unit": "activeHours", "p50": [35, 45], "p90Max": 60 },
  "first_prestige":      { "policy": "balanced", "unit": "activeHours", "p50": [25, 35], "p90Max": 45 },
  "completion_1_0":      { "policy": "min-max",  "unit": "activeHours", "p50": [200, 300], "p90Max": 380 } }
```

**Regression method.** `packages/sim/golden/<policy>.json` holds, for seeds 1 to 20, the per-hour median of levels, gold and milestone ticks. CI reruns the seeds and fails if a milestone p50 leaves its band or a golden point moves more than 5% (10% for gold) unless the PR carries `balance-change` and a `balance/<pr>.md` rationale; goldens are a protected path regenerated only by the owner-approved release step. Watchdog B runs 200 seeds for 300 hours per policy nightly and posts p10, p50, p90 per milestone, bands hit or missed, and drift versus last week.

**Monte-Carlo statistics.** Medians and percentiles with bootstrap 95% intervals (1,000 resamples); a band is missed only when the interval lies outside it, which avoids flapping on 20-seed PR runs.

**Keep-if-better loop.** `sim tune --params balance/tunables.json --objective bands --policies balanced,combat-first --heldout min-max --budget 40`: tunables are named constants in `content/base/balance.json` (XP exponent, base seconds per skill, drop multipliers, monster HP and damage coefficients, shop prices) with ranges. Each step changes one constant (coordinate descent with step halving), runs 20 seeds and keeps the change if the objective (normalized distance of p50 milestone times to band centers, plus penalties for unreachable items and sinkless materials) improves while the held-out policy worsens by no more than 5%. Git is the ledger: each kept step is a commit on a `tune/*` branch, and the loop ends by opening one `balance-change` PR with a before-and-after table. It never merges.

**Performance targets.** M0 exit: 100 hours of two skills under 10 s on one core (10 simulated hours per wall second without event skipping). M1: 50 hours per second with combat and skipping. M2: 200 hours per second so that 200 seeds × 300 hours finishes under 5 minutes on four worker threads. Memory: a 300-hour run under 200 MB; the event log sink streams to disk.

## 7. UI

**Architecture.** React 18 function components over one Zustand store holding `{ state: GameState, view: UiState }`. `GameProvider` owns the tick loop, calls `advance` and sets the new state once per tick. Components subscribe through selectors with `useShallow`, so a ticking progress bar re-renders alone while panels subscribe to slices (`state.inventory.slots`, `state.skills`) whose identity changes only when they change, guaranteed by `core`'s structural sharing. Derived views (sorted inventory, affordable recipes) come from memoized `core/selectors`. No game logic in components; a lint test keeps `ui` away from `core/formulas`.

**Component inventory.** `AppShell` (sidebar, header with gold and monsoon meter, main panel, activity feed), `SkillList`, `SkillPanel` with `ActionCard` and `ProgressBar`, `InventoryGrid` with `ItemTooltip`, `EquipmentPanel`, `CombatPanel` (area picker, monster card, HP bars, auto-eat), `DungeonPanel`, `ShopPanel`, `PrestigePanel`, `AchievementsPanel`, `SettingsPanel`, `SaveDialog` (slots, export, import, backups), `OfflineSummaryModal`, `Toast`, `ActivityFeed`, `ModsPanel`.

**Theming tokens.** CSS custom properties on `:root` and `[data-theme]`: `--bg-0..3`, `--ink-0..2`, `--accent` (ochre), `--accent-2` (dusk violet), `--ok`, `--warn`, `--bad`, `--radius`, `--space-1..6`, `--font-ui`, `--font-mono`, `--motion` (0 under reduced motion). Palette checked for the three common color-vision deficiencies; status is never conveyed by color alone (icons and text).

**Accessibility.** Every interactive element is a real `button` or `a`; roving tabindex in grids; `aria-live="polite"` on the activity feed and `assertive` on death and level-up; focus returns to the opener after modals; `prefers-reduced-motion` disables bars' animation and particle effects; text scaling by `rem` with a 100 to 175% setting; axe runs on every panel in Playwright with zero serious or critical violations as the gate.

**PWA and offline.** `vite-plugin-pwa` precaches the shell and content JSON; updates show a "new version ready" toast, never a forced reload; `display: standalone`; the offline-progress path runs on resume. Saves are local, so the app works fully offline.

**Performance budgets.** Tick to UI commit under 4 ms p95; interaction to paint under 100 ms; initial JS under 350 KB gzipped (content JSON separate, under 400 KB gzipped); first interaction under 2 s on a mid-range phone; memory steady over a 24-hour headless run (Playwright, heap growth under 10%).

## 8. Desktop

Electron with `steamworks.js`. The renderer is the `apps/web` build served from `file://` with a strict CSP and `contextIsolation`; the preload exposes a typed `window.desktop` IPC contract (`saveFile`, `cloud.read/write`, `achievements.unlock`, `overlay.isEnabled`, `richPresence.set`).

| Integration | Design |
|---|---|
| Achievements | `content/base/achievements.json` entries carry an optional `steamId`; the desktop adapter listens to `achievement` events and calls `achievement.activate`; a reconciliation on launch unlocks any missed |
| Cloud saves | The envelope per slot written with `cloud.writeFile("slot-<n>.json")` on autosave (debounced to 30 s) and quit; on launch, compare local and cloud by `playtimeTicks` then `savedAtMs`, prompt only on conflict |
| Overlay | Default on; the game detects `overlay.isEnabled()` to avoid blocking shortcuts; Steam Input is not required (keyboard and mouse plus controller through the browser Gamepad API) |
| Steam Deck | A `compact` layout breakpoint at 1280×800 with larger hit targets, a controller cursor mode, and the sidebar collapsed; a Playwright snapshot at that resolution is a gate |
| Packaging | `electron-builder` for Windows, macOS and Linux; Steam depots per OS; `steamcmd` upload from the release workflow with the owner's credentials in secrets |
| Updates | Steam handles updates; the desktop build never self-updates; the web build and the desktop build share `gameVersion`, and saves are portable both ways |
| AI disclosure | `build-meta.json` in every bundle: `{ gameVersion, builtAt, aiDisclosure: { codeTools: "Claude Code", generatedPlayerFacingContent: [...] from the content generation log }, contentHash }`, shown on the About screen and pasted into the Steam disclosure field |

## 9. Telemetry

Opt-in only, default off, a Settings switch with the privacy text beside it. When on, the client batches events and posts at most every 10 minutes to a Cloudflare Worker that validates against the same Zod schema and writes to D1. The Worker drops the IP, accepts no free text, and the install id is a random UUID the player can reset or delete (`DELETE /install/<id>`).

```ts
interface TelemetryEvent { installId: string; sessionId: string; gameVersion: string; contentHash: string; platform: "web" | "steam";
  at: number; kind: "session_start" | "session_end" | "milestone" | "level_up" | "prestige" | "offline_return" | "death" | "setting";
  data: Record<string, number | string | boolean> }   // milestone: {id, activeHours, playtimeHours}; level_up sampled at every 10 levels
```

Comparison with the simulator: a weekly job converts player milestone events into the `SimResult.milestones` shape per install and runs the same `summarize`, producing a table of player p10/p50/p90 next to each policy's. The report highlights where real players sit relative to the bands and which policy they resemble, which is the input for retuning bands (the bands are hypotheses) rather than constants.

## 10. Modding

**Data packs.** A folder or zip with `pack.json` and `content/*.json` using the base schemas:

```ts
interface PackManifest { id: string; name: string; version: string; minGameVersion: string; author: string;
  depends: { id: string; version: string }[]; permissions: ("content:add" | "content:override" | "strings" | "balance")[];
  overrides: string[] /* ids this pack is allowed to replace */ }
```

Loading order: base, then packs in dependency order. Validation runs the full content linter on the merged bundle; a pack that breaks obtainability or references is rejected with the linter output. Conflict rules: two packs adding the same id fail unless one depends on the other; an override requires the `content:override` permission and the id listed in `overrides`; later packs win among declared overrides; `balance` permission is needed to touch `balance.json`. Strings merge per key with the same rules.

**Permission prompt.** Before enabling a pack the UI shows counts of added entities per collection, the overridden base ids, whether balance changes, the pack hash and author, and a warning that achievements are off while a pack with `balance` or `content:override` is active (Q-B9). Saves stamp active packs; a changed hash disables the pack until re-approved; a missing pack makes the save read-only with an explanation. Web packs load from a URL with the same prompt; Steam adds Workshop or mod.io later.

**Scripted mods later.** Options, in preference order: QuickJS compiled to WASM with a capability API (`onEvent`, `addAction`, `modifyFormula` behind named hooks), per-tick CPU budget and memory limit; a Web Worker with a message protocol (weaker isolation); no `eval` in the main realm ever. Deterministic script execution is required for sim parity, so scripts get the `world` stream through the API and no clock.

## 11. Build, CI and release

**Workspace.** `pnpm-workspace.yaml` with `packages/*` and `apps/*`; root `package.json` scripts: `gate:fast`, `gate:full`, `content:lint`, `terms:lint`, `sim`, `parity`, `extract`. Tooling configs: `tsconfig.base.json` (strict, `noUncheckedIndexedAccess`), `eslint.config.js` (typescript-eslint strict, boundaries, restricted globals in `core`), `.prettierrc`, `vitest.workspace.ts`, `stryker.config.json` per package, `playwright.config.ts` (projects ui, parity, desktop-smoke), `.dependency-cruiser.cjs`, `semgrep.yml`, `.gitleaks.toml`, `size-limit.json`, `.github/workflows/{ci,nightly,release}.yml`, `.changeset/`.

**CI stages (per PR), with budgets.**

| # | Stage | Budget | Fails when |
|---|---|---|---|
| 1 | Install (pnpm cache) | 1 min | |
| 2 | Format, lint, boundaries, typecheck | 2 min | Any error |
| 3 | Unit and property tests (Vitest, all packages) | 3 min | Any failure |
| 4 | Content lint and forbidden terms | 1 min | Any error-level rule, any forbidden term |
| 5 | Sim regression (20 seeds, 100 hours, 3 policies) | 4 min | Band or golden breach without label |
| 6 | Build web and size budgets | 2 min | Over budget |
| 7 | Playwright UI suites with axe | 6 min | Failure or serious axe violation |
| 8 | Mutation on changed files (parallel job) | 8 min | Below floor (Section 12) |
| 9 | Semgrep, OSV-Scanner, Gitleaks (parallel) | 3 min | High finding |
| 10 | Diff-size gate | seconds | Over 600 lines without label |

Wall clock under 15 minutes with 7 to 9 in parallel. Nightly adds: full mutation run, 200-seed Monte-Carlo, parity stochastic suites, extractor diff, 24-hour headless memory run, desktop smoke build.

**Release.** Web: a `v*` tag builds, writes the changelog from changesets, deploys to `gh-pages` under `/next/` (the root keeps the legacy file until the rename, Q-B10) and to itch.io via butler, then smoke-tests the live URL with Playwright. Steam: the same tag builds Electron for three OSes, uploads to a `beta` branch with steamcmd, smoke-tests, and waits for the owner to promote `beta` to `default`. Rollback re-promotes the previous depot or redeploys the previous Pages commit.

**Versioning.** `gameVersion` is semver; minor for content drops, patch for fixes, major for a save-breaking change that migrations cannot bridge (none planned). `schemaVersion` is an independent integer. Content packs carry semver and `minGameVersion`.

## 12. Testing strategy mapped to oracles

| Oracle | Tests | Floors and policy |
|---|---|---|
| Property tests (fast-check, research B1) | Formulas: `levelForXp(xpForLevel(n)) == n`; `xpForLevel` strictly increasing; action duration never below one tick and monotone in speed; damage non-negative and monotone in attack stats. Inventory: quantities never negative; crafting conserves mass (inputs consumed plus outputs produced equal recipe spec); `add(remove(x)) == identity` within capacity. Saves: `deserialize(serialize(s)) deepEquals s` for generated states; migration chains from every fixture produce schema-valid states; export codes round trip. Determinism: chunk invariance and replay equality (2.5) | Each property runs 200 cases in CI, 2,000 nightly; shrunk counterexamples become fixtures |
| Mutation testing (Stryker, research B3) | Changed files on every PR; weekly full | Floors: `core` 70%, `content` (loader and linter) 60%, `sim` 50%, `tools` 50%, `ui` 40%; survivors become issues via the maintenance routine |
| Playwright suites (research B6) | `ui`: new game, first hour at speed through `page.clock`, save and load, offline return, prestige, every panel axe-checked; `parity` (Section 5); `desktop-smoke`: launch Electron, load a save, unlock a stub achievement; `deck`: 1280×800 snapshot | Test agents may draft tests; the healer may change locators, never assertions |
| Snapshots | Visual snapshots per panel at two themes and two widths; roll-order golden; sim goldens; save fixtures | Human-only updates: protected paths in `guard-paths.sh`; a PR that touches them without the owner as author fails |
| Performance | Tick benchmark (Vitest bench) with a p95 budget; sim hours-per-second benchmark; bundle `size-limit`; 24-hour headless heap run nightly | Budgets in Section 7 and 6; regressions over 10% fail |
| Save-corruption fuzzing | fast-check mutators over serialized saves: truncation, byte flips, key deletion, type swaps, huge numbers, duplicated ids; `load` must either produce a valid state or a typed `SaveError`, never throw elsewhere or produce an invalid state | 1,000 cases per PR, 50,000 nightly; crashes become fixtures |
| Cross-version migration | Every `v<n>` fixture migrates to current; a `v<n>` save produced by the tagged build is added at each release by the release workflow | Fixtures are human-only paths |
| Parity (Section 5) | Exact tables per PR, stochastic nightly | Gate for `ported` status |

## 13. Milestone-to-issue breakdown

Sizes: S under one hour of agent work, M under three hours. Every issue carries the standard template (goal, acceptance, out of scope, test plan); the acceptance criteria below are the checkable ones. Order within a milestone is dependency order.

**M0 Foundation, extractor, parity harness (exit: headless sim runs 100 hours of two skills under 10 s; gate green).**

| # | Issue | Size | Acceptance criteria |
|---|---|---|---|
| 1 | Monorepo skeleton: pnpm workspace, packages and apps folders, strict tsconfig, Vitest, ESLint, Prettier | S | `pnpm -r typecheck` and `pnpm -r test` green on empty packages; CLAUDE.md lists the commands |
| 2 | Package boundary enforcement | S | dependency-cruiser fails on an upward import in a test fixture; `core` frozen-globals test fails when a module touches `Date` |
| 3 | Repo files: SPEC, CONSTITUTION, features.json, DECISIONS, labels, issue template | S | Files present; labels created; `features.json` validates against its schema |
| 4 | Hooks: `guard-paths.sh`, `gate-fast.sh`, `gate-stop.sh` | M | Edit to `**/golden/**` is blocked with a reason; fast gate runs under 60 s; Stop hook rejects a session without command evidence |
| 5 | CI workflow stages 1 to 4 and 10 | M | Workflow green on main; a seeded lint error fails stage 2; diff-size gate fails a 700-line PR without label |
| 6 | Forbidden-terms lint | S | List file with every term from the port inventory; `index.html` and `legacy-draft/` excluded; a fixture with "Elwynn" fails |
| 7 | `core` PRNG: xoshiro128** plus SplitMix32 seeding and named streams | S | Known-answer test vectors pass; two streams from one seed are uncorrelated on a 1e6 sample chi-square; roll-order golden written |
| 8 | `core` state model, initial state factory, state Zod schema | M | `createGame(content, seed)` validates; every field documented in a type comment; no `Set` or `Map` in state |
| 9 | `core` tick scheduler, command queue, system order | M | Commands apply at their tick; system order test; 50-completion guard tested |
| 10 | `core` event log ring and sink | S | Ring keeps last 2,000; sink receives every event; save round trip keeps the ring |
| 11 | `core/formulas.ts` v1: XP curve, level from XP, action duration, with property tests | M | Properties from Section 12 pass; values match the legacy curve table extracted by pass C at 20 points |
| 12 | `core` skill actions: gathering and crafting with timers, inputs, outputs, bonus drops | M | Prospecting and Smelting run 1 hour headless; inventory conservation property passes |
| 13 | `core` inventory module | S | Never-negative and capacity properties pass; mutation score at or above 70% on the file |
| 14 | `core` save: envelope, serialize, deserialize, schemaVersion, migration registry | M | Round-trip property passes; fixture `v1.json` committed; a no-op migration `1 -> 2` is tested |
| 15 | `core` determinism suite | S | Replay equality, chunk invariance and mid-run save tests pass |
| 16 | `content` schemas v1: Skill, Action, Item, Recipe, Strings, plus `loadContent` | M | Schemas match Section 3.1; loader rejects an unknown key; `contentHash` stable across key order |
| 17 | `content` linter v1: L001 to L005, L011 | M | Each rule has a failing fixture; output includes rule id and fix hint; runs under 5 s |
| 18 | Rename map format and seed content for Prospecting and Smelting in Dry River names | M | Linter and forbidden terms green; every entity has `legacyId`; strings complete in `en` |
| 19 | Legacy extractor pass A | M | 1,035 literals found; unresolved identifiers listed in `report.json`; constants substituted for `hl`, `Pa`, `Ts`; functions recorded as `$code` |
| 20 | Legacy extractor pass B with decoders for `ka`, `Te`, `rt`, `be`, `ml`, `ll` family, `Dd` | M | 12 skills, 19 zones, 37 dungeon entries, about 493 monsters decoded; decoder body hashes recorded |
| 21 | Legacy extractor pass C runtime dump and reconciliation | M | Instrumented copy written only to scratch; registries dumped; `merged/` has one record per id with sources and conflicts |
| 22 | `legacy-draft` folder rules | S | `.gitignore` keeps only README and reports; terms lint excludes the path; build fails on `legacyName` outside the folder |
| 23 | `sim` runner v1 and `greedy-xp` policy | M | `sim run --policy greedy-xp --hours 100 --seed 1` writes a `SimResult`; event skipping verified against non-skipping run by hash |
| 24 | `sim` performance benchmark | S | 100 hours of two skills under 10 s in CI; benchmark recorded in the PR body |
| 25 | Parity: static server, legacy boot fixture, save-key discovery | M | Fixture creates a character and reads the save; `LegacySave` sketch validates; fonts blocked |
| 26 | Parity: `LegacyDriver` actions, clock mode, catch-up mode | M | Start an action via `data-a`; one simulated hour under 20 s wall; catch-up box awaited |
| 27 | Parity: first suite, Prospecting versus legacy Mining (timing, XP, drops) | M | Metrics and tolerances from Section 5; results file attached to the PR |
| 28 | Parity report, nightly workflow, `ported` gating | S | `parity-status.json` written; failing nightly reopens the issue; `features.json` schema has `ported` |
| 29 | Routines: nightly builder, reviewer, maintenance, Watchdog B | S | Routines created with the prompts from design 00; first dry run opens a PR |

**M1 Vertical slice (exit: you play it for an hour and want to keep going).**

| # | Issue | Size | Acceptance criteria |
|---|---|---|---|
| 30 | `core` combat: stats, hit, evasion, armor, attack timers, death and respawn | M | Formulas match Section 5 monster and player tables exactly; properties for non-negative damage and monotonicity |
| 31 | Combat parity suite over 500 fights at three levels | M | Tolerances met or divergence logged in DECISIONS |
| 32 | `core` auto-eat, potions, flee threshold | S | Auto-eat triggers at threshold; flee event emitted; no eat when inventory empty |
| 33 | `core` areas, unlocks, monster selection | S | Unlock rules from Area schema; command validation rejects locked areas |
| 34 | `core` shop: buy, sell, stock, restock from `world` stream | S | Gold conservation property; restock deterministic for a seed |
| 35 | `core` offline progress: cap, chunking, policy, summary | M | 2, 8 and 23-hour gaps match an online run within 2%; one-death rule tested; summary fields complete |
| 36 | Port Harvesting and Fishing with drop tables | M | Parity suites pass for both; content promoted via the workflow |
| 37 | Port Cooking recipes and food | S | Recipes conserve inputs; food heals as content says; parity on fail rates |
| 38 | `content` schemas Monster, Area, ShopEntry; linter L006 to L010 | M | Each rule has fixtures; L006 walk finishes under 2 s on M1 content |
| 39 | M1 content set: four skills, one area, shop, starting kit | M | Linter and terms green; all names `accepted` in the rename map |
| 40 | `content promote` CLI and review workflow doc | S | Promote copies, renames, strips provenance; README describes the five steps |
| 41 | `sim` policies `balanced` and `combat-first` | M | Both finish 100 hours; combat-first death rate under 5% per hour on M1 content |
| 42 | Bands file, golden curves, CI regression stage | M | Stage 5 fails on a seeded XP change without label; passes with label and rationale |
| 43 | Monte-Carlo summary and markdown report | S | p10, p50, p90 with bootstrap intervals; nightly posts the report |
| 44 | `ui` app shell, store bridge, tick driver, selectors | M | Tick loop at 10 Hz with drift correction; a ticking bar re-renders only itself (React Profiler test) |
| 45 | `ui` skill panel, action cards, progress bars | M | Start and stop actions; keyboard operable; axe clean |
| 46 | `ui` inventory and equipment | M | Tooltips, equip and unequip, sell; virtualized list over 600 items keeps 60 fps |
| 47 | `ui` combat panel | M | Area picker, HP bars, auto-eat control; death and respawn visible |
| 48 | `ui` shop panel | S | Buy and sell with confirmations over 10% of gold; stock timers shown |
| 49 | `ui` save dialog and offline summary | M | Slots, export code, import, backups list; summary matches the event log |
| 50 | Theming tokens, dark and light, reduced motion | S | Tokens from Section 7; contrast at least 4.5:1 on text; motion off under the media query |
| 51 | Accessibility baseline and axe in Playwright | M | Zero serious violations on every panel; focus management test for modals |
| 52 | `apps/web` PWA: manifest, service worker, offline cache, Pages deploy under `/next/` | M | Lighthouse PWA installable; app loads offline; deploy workflow green |
| 53 | Playwright e2e: new game, first hour at speed, save and load, offline return | M | Suite under 6 min; runs on every PR |
| 54 | Persistence adapters: localStorage, IndexedDB fallback, backups, tab lease | M | Fallback exercised by a test that disables localStorage; lease takeover banner shown |
| 55 | Save-corruption fuzzing | S | 1,000 cases per PR; every failure returns a typed `SaveError` |
| 56 | Performance budgets in CI | S | `size-limit` configured; tick p95 benchmark; failures block |
| 57 | Private alpha release procedure, version stamp, changelog | S | Tag builds and deploys; About screen shows `gameVersion` and `contentHash` |

**M2 Content and community (exit: 500 weekly active players or a clear signal).**

| # | Issue | Size | Acceptance criteria |
|---|---|---|---|
| 58 | `core` dungeons: waves, boss, needs, rewards, unlocks | M | Wave order deterministic; rewards from `drops` stream; parity on clear time for two legacy dungeons |
| 59 | `core` prestige v1: monsoon meter, watermarks, upgrades | M | Reset keeps watermarks and achievements; upgrade effects applied through selectors; bands for first prestige measured |
| 60 | `core` achievements engine | S | Dirty-set evaluation; 20 achievements in content; event emitted once |
| 61 | `content` schemas Dungeon, Achievement, PrestigeUpgrade and linter rules | M | Fixtures per rule; `steamId` optional field present |
| 62 | Port crafting skills batch: Silversmithing, Leatherwork, Distilling | M | Parity on recipe outputs; gear coverage L008 warnings resolved for levels 1 to 40 |
| 63 | Port remaining skills: Woodcutting, Trapping, Masonry, Cartography | M | All 14 skills present; L010 ladder passes; names accepted |
| 64 | Six areas and ten dungeons content | M | Selected from the 19 zones and 37 legacy entries by mechanic variety; linter green |
| 65 | `sim` policies `min-max` and `random`; idle-versus-active metric | M | Random policy finds no unreachable state in 50 seeds; ratio reported |
| 66 | `sim tune` keep-if-better loop with held-out policy | M | One run of 40 steps opens a `balance-change` PR with a before-and-after table; never merges |
| 67 | Data-pack loader, validation, conflict rules | M | Fixtures for each conflict rule; merged bundle passes the linter |
| 68 | Mods panel, permission prompt, save stamping | M | Prompt lists counts and overrides; changed hash disables the pack; read-only mode on missing pack |
| 69 | Telemetry client, schema, Worker endpoint | M | Default off; events validated; IP not stored (test on the Worker); delete endpoint works |
| 70 | Player-versus-simulator report | S | Weekly job produces the comparison table in the digest |
| 71 | `ui` prestige and achievements panels | M | Axe clean; snapshots taken |
| 72 | Panel visual snapshots, human-only updates | S | Snapshots for every panel at two themes; `guard-paths.sh` blocks agent updates |
| 73 | itch.io build and public alpha deploy | S | Butler push from the release workflow; page updated with the version |
| 74 | Content generation log for AI disclosure | S | Every generated string carries a log entry; `build-meta.json` lists them |
| 75 | Weekly balance report routine | S | Monte-Carlo report and band status posted to the digest |

## 14. Open questions for the owner

| ID | Question | Recommended default |
|---|---|---|
| Q-B1 | Keep the legacy XP curve, timers and drop rates exactly (parity first, tune later) or rebalance during the port? | Parity first; every deliberate change goes through `balance-change` with sim evidence |
| Q-B2 | New content ids or legacy slug ids with renamed strings? | New ids; `legacyId` keeps provenance; the rename map is the bridge |
| Q-B3 | Offline cap: flat 24 hours, or a base of 8 hours with upgrades to 24 as the legacy's upgradeable hours suggest? | Flat 24 hours in M1; an upgrade track in prestige in M2 if the sim shows idle ratio too low |
| Q-B4 | What happens to the legacy Thieving skill (risk and stun mechanics) in the 14-skill Dry River list? | Fold the risk mechanic into Trapping and Prospecting claims; no separate skill |
| Q-B5 | Which 10 of the 37 legacy dungeon-kind entries are ported first? | Pick by mechanic variety in M2 issue 64; the rest become content drops |
| Q-B6 | Tick loop on the main thread or in a Web Worker? | Main thread with drift correction; move to a worker only if UI jank is measured |
| Q-B7 | Export-code format and compression | `DR1.` plus base64url deflate-raw; `DR0.` uncompressed fallback |
| Q-B8 | Steam Cloud through the API (explicit) or Auto-Cloud (folder sync)? | API through steamworks.js for conflict control |
| Q-B9 | Disable achievements while balance-altering mods are active? | Yes, with the rule shown in the permission prompt |
| Q-B10 | Where does the new web build deploy while `index.html` stays at the root? | `gh-pages` branch under `/next/` until the repo rename (Q-010), then root |
| Q-B11 | Telemetry host | Cloudflare Worker plus D1, free tier; switch off entirely if the privacy text is not approved |
| Q-B12 | State library for React | Zustand with selectors; revisit only if the profiler shows a problem |
| Q-B13 | Monster families: keep the legacy list of ten or the nine in the Monster schema (cryptids, spirits, constructs)? | Nine Dry River families; the rename map maps each legacy family |
| Q-B14 | Policy decision interval of 10 simulated seconds | Keep; measure the effect on milestone times once with 1 s and record it |
