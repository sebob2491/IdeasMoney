# Track B design: an original-IP idle RPG, web first, then Steam

Status: design, 2026-11-01. Evidence: `WHAT-TO-BUILD.md` Sections 4.3, 8 and 9; `research/04-validate-ideas-1-5.md` (Idea 5); `research/06-ai-reverse-engineering-games.md`; `research/07-browser-remakes-and-mashups.md`.

## 1. What it is

A deep-systems idle RPG in the Melvor Idle tradition: gathering and crafting skills, combat with gear progression, areas and dungeons, a prestige layer, and hundreds of hours of content, under an original setting with no third-party IP. It ships first as a free web game to build a community, then as a paid Steam release once wishlists justify it. It is data-driven and officially moddable from day one.

Honest expectations from the research: comparable deep-systems games (Magic Research 1 and 2, Increlution) grossed $38K to $90K lifetime on Steam; breakouts needed a visual hook or a publisher; the genre's supply rose 150% in 2026 and players are increasingly AI-sceptical. This track is the asymmetric bet and the showcase for simulation-driven development. It costs direction and playtesting, not money.

## 2. Setting: pick one (open question Q-004)

The WoW theme must go entirely: names, zones, factions, lore, item names, spell names. A CI lint (Section 6) enforces a forbidden-terms list. Three directions, each sized for icon-based art:

1. **Sonoran myth** (recommended). A frontier of canyons, mesas, dry rivers and ruined missions under a sky with two moons. Gathering: prospecting, agave harvesting, mesquite woodcutting, riverbed fishing, cactus-fruit foraging. Crafting: silversmithing, leatherwork, adobe building, distilling. Combat: cryptids, dust spirits, outlaw constructs. Prestige: monsoon seasons that reshape the land and reset with permanent "watermarks." Distinctive ochre palette, unusual in the genre, authentic to where you live, and a story hook for devlogs. Risk: regional flavor reads niche to some players; mitigated by universal mechanics and clear iconography.
2. **Abyss expedition.** A drowned world explored from dive stations. Gathering: salvage, kelp, pressure crystals. Crafting: suits, lamps, submersibles. Combat: bioluminescent fauna and leviathans. Prestige: descending to a new depth layer. Strong hook; few idle games are deep-sea.
3. **The archive.** Scholars reclaim a ruined academy. Gathering: excavation, transcription. Crafting: alchemy, enchanting, bookbinding. Combat: constructs and curses. Prestige: new "editions." Appeals to the Magic Research audience; more abstract.

A plain original high-fantasy world is the safe fourth option and the most crowded.

## 3. The existing game

Azeroth Idle (`azeroth-idle.html`) is the design reference and content seed, not the codebase. A single-file HTML game cannot carry a hands-off pipeline: no module boundaries for tests, no headless simulation, no save migrations, no content schema. The plan is a rebuild in TypeScript with the architecture below, porting the systems and content tables with renamed, re-themed content. The review heuristics already encoded in your skills (broken references, unobtainable items, sink-less materials, gear coverage, gold flow) become CI checks. Please add the file to the Track B repo, or to this one, so the port can start (Q-005).

## 4. Architecture

**Stack.** TypeScript strict, Vite, pnpm workspaces. React for the UI (the deepest agent training data), with fine-grained state via Zustand selectors so ticking numbers do not re-render the world. Electron plus steamworks.js for the Steam build (the proven path for web games on Steam); Tauri later if bundle size matters. Playwright for end-to-end tests.

**Packages.**

| Package | Role | Rules |
|---|---|---|
| `core` | Deterministic simulation: skills, inventory, combat, economy, prestige, offline progress, save and migrations | Pure TypeScript, no DOM, no timers; every formula in `formulas.ts`; seeded PRNG; fixed-timestep tick |
| `content` | All game data as JSON validated by Zod schemas: skills, items, recipes, monsters, areas, dungeons, shop, achievements, prestige upgrades, strings | Agents add content here; the content linter gates it |
| `ui` | React components, panels, theming, accessibility | Reads core state via selectors; never computes game logic |
| `sim` | Headless runner and player policies; Monte-Carlo progression; balance reports | Runs thousands of hours in seconds; CI uses it |
| `tools` | Content linter, forbidden-terms lint, balance dashboard, save inspector | CLI, used by hooks and CI |
| `apps/web` | PWA, offline, itch.io and own-domain builds | Free |
| `apps/desktop` | Electron, Steamworks (achievements, cloud saves), Steam Deck layout | Paid |

**Core design points.** Fixed-timestep ticks (ten per second) with action timers in ticks, so results are reproducible from a seed. Offline progress is computed by fast-forward simulation with a 24-hour cap, the genre norm. Saves are versioned with forward migrations and a fuzzed corruption test. An event log makes any bug reproducible from seed plus inputs.

**Content.** Target scale at 1.0: about 14 skills, 600 items, 120 monsters, 25 areas, 10 dungeons, 250 recipes, 150 achievements, a prestige layer with about 40 upgrades. Every entity has an id, a schema, and cross-references the linter checks. Content packs are the modding unit.

**Modding.** Day one: data packs (JSON overrides and additions) loaded from a folder or URL with a permission prompt. Later: a sandboxed script API and mod.io on Steam, the route that gave Melvor over a million mod downloads. Modding is what turns players into a community, and it is cheap when the game is data-driven from the start.

**Headless simulation.** `sim run --policy <name> --hours 500 --seeds 1..200` produces progression curves: time to each milestone, resource flows, bottlenecks, idle-versus-active ratios, and gold and material sinks. Policies: scripted (greedy XP, balanced, combat-first, min-max), plus optional LLM-driven personas for discovery. Target bands per milestone are hypotheses tuned by playtests, for example first dungeon clear at 4 to 6 active hours, all skills at level 50 around 40 hours, first prestige at 25 to 35 hours, 1.0 completion in the 200 to 300 hour range. A keep-if-better loop tunes constants against the bands with a held-out policy the loop cannot see. Bots answer "is it balanced," you answer "is it fun."

**Platforms and money.** Web: free, on itch.io and your own domain, as a PWA. Steam: $7.99 to $9.99 one-time with a launch discount, achievements, cloud saves, Steam Deck verified; expansions as paid DLC after launch. No ads, no in-app purchases; the genre's audience punishes both.

**Telemetry.** Opt-in, anonymous progression events to compare real players against the simulator, with a plain privacy policy.

**Accessibility.** Keyboard navigation, reduced motion, colorblind-safe palette, screen-reader labels, text scaling. Cheap to do from the start and a reliable source of good reviews.

## 5. Art and AI disclosure

Art is the genre's smallest cost and the biggest reputational risk. Phase plan: game-icons.net (CC BY, thousands of consistent monochrome icons, recolorable) for the web alpha; a commissioned icon set and key art before the Steam page goes live, budget roughly $2K to $5K (Q-006); generated art only where disclosed and reviewed. Steam's January 2026 rule exempts AI coding tools but requires disclosure of any generated content players see or hear, so the content pipeline keeps a log of what was generated, and all player-facing text is human-reviewed. The disclosure text is drafted in M3.

## 6. Track-specific oracles

- Unit and property tests on `formulas.ts` and inventories: monotonic XP curves, no negative quantities, conservation in crafting, save round-trips, migration chains from every past version.
- Content linter in CI: schema validity, no dangling references, every item obtainable, every material has a sink, gear coverage per level band, gold inflow and sinks within ratio bands, naming conventions, forbidden-terms list (every Blizzard and Warcraft name) with zero tolerance.
- Simulation regression: golden progression curves with tolerance bands; a PR that moves a milestone outside its band fails unless labeled `balance-change` with a rationale.
- Playwright end-to-end: new game, first hour scripted at high speed, save and load, offline progress, prestige; visual snapshots of each panel, human-only to update; axe accessibility checks.
- Performance budgets: tick time, memory over a 24-hour headless run, bundle size, first interaction time.
- Electron smoke test on each desktop build; Steam Deck resolution layout snapshot.
- Mutation testing (Stryker) at a 70% floor on `core`.

## 7. Roadmap

| Milestone | Weeks | Deliverables | Exit criterion |
|---|---|---|---|
| M0 Foundation | 1 to 2 | Setting decided; monorepo skeleton; `core` with two ported skills; `sim` harness; content schemas and linter; forbidden-terms lint; full gate; CI; nightly builder | Headless sim runs 100 hours of two skills under 10 seconds; gate green |
| M1 Vertical slice | 2 to 6 | Four skills, combat, one area, shop, inventory, save and offline progress; first balance bands; private web alpha | You play it for an hour and want to keep going |
| M2 Content and community | 6 to 12 | Full skill set, six-plus areas, dungeons, prestige v1, achievements, data-pack mods; public web alpha on itch.io and your domain; Discord; feedback thread on r/incremental_games; weekly content drops | 500 weekly active players or a clear signal to change course |
| M3 Polish and Steam page | 12 to 20 | Commissioned icon set and key art; accessibility pass; Electron and Steamworks; Steam page live with screenshots and trailer; demo build; AI disclosure text | Steam page live; wishlists growing week over week |
| M4 Launch | 20 to 28 | Next Fest demo (Feb 2027); launch at roughly 7,000 wishlists, the threshold cited for Steam's "Popular Upcoming" placement; post-launch content cadence | 1.0 on Steam |
| Steady state | 28 onward | Monthly content updates, mod API, first expansion, mobile evaluation | Reviews above 90% positive |

## 8. Community and marketing: what runs itself and what needs you

Runs itself: devlog drafts, patch notes, a data-generated wiki (disclosed), Discord feedback triage into issues, itch.io page updates, weekly balance reports.

Needs you: posting as the human face on Reddit and Discord, playing every build, approving the setting and names, choosing the artist, recording or approving the trailer, Steam account and page ownership.

## 9. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Genre flood and AI stigma | Original setting with a visual identity; commissioned art; disclosure; community first; depth over breadth |
| "Balanced but not fun" | Your weekly hour of play is the oracle bots cannot replace; M1 exit criterion is subjective on purpose |
| Scope creep | `features.json` as the registry; content targets fixed per milestone |
| IP leakage from the port | Forbidden-terms lint; content review before public alpha; trademark search on the final name |
| Low wishlists | Web alpha first builds the audience before the Steam page exists; launch gated on the wishlist threshold |
| Save corruption and migrations | Versioned saves, migration tests from every version, fuzzing |

## 10. Your involvement

M0: pick the setting and a working title. Then the weekly hour, with the fifteen minutes of play being the most important part. Steam account and artist commission in M3.

## 11. Decisions and accounts

Decide: setting and working title (Q-004), adding the existing game to a repo (Q-005), art budget (Q-006). Open: itch.io, a domain, Discord, Steamworks ($100 per app, recoupable) by M3.
