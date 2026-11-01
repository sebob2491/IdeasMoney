# Track B port inventory: what is actually in the azeroth-idle repo

Status: findings, 2026-11-01, from the attached `sebob2491/azeroth-idle` repo (14 commits, Sept 12 to Oct 8, 2026).

## What the repo contains

- One file: `index.html`, 1.85 MB, 2,021 lines, longest line 443,394 characters. It is a **minified production bundle**, not source. Function names are mangled (`cv`, `S`, `le`); the last five commits are titled "Azeroth Idle v184" through "v189"; the earlier nine are "Add files via upload."
- Stack fingerprints in the bundle: SolidJS (124 hits), Vite (4), immer (27). So the readable project is a Vite plus Solid app compiled to a single file. Fonts load from Google Fonts (IBM Plex Mono, Space Grotesk). There are no embedded images; icons are text, CSS or inline SVG.
- Saves go to `localStorage` under a namespaced key as JSON, with a storage probe and an in-memory fallback.
- Content is embedded as object literals with readable strings: about 1,041 `id` and 1,476 `name` fields. Skills visible by name: Mining, Fishing, Herbalism, Cooking, Blacksmithing, Alchemy, Enchanting, Tailoring, Leatherworking, Jewelcrafting, Skinning. Id prefixes mark groups of zones and dungeons (`the_`, `ench_`, `incubator_`, `ss_`, `ir_`, `hl_`, `wh_`, `wb_`, `ts_`, `th_`, `ow_`, `orb_`, `kn_`, `bf_`, `an_`, `ac_`, `k_`, `town_`), tools (pickaxe, sickle, alembic, loupe, runed rod), enchant affixes (acceleration, massive, replenishing, covetous, divine), and flavor entities (a thief's ghost, a smith's ghost).
- Third-party IP terms in strings: Azeroth 31, Alliance 33, Horde 31, Elwynn 22, Westfall 8, Deadmines 7, Durotar 5, Stormwind 4, Draenei 4, Tauren 3, Blizzard 3, Orgrimmar 2, Ironforge 1, Hearthstone 1.
- The readable source is not in this repo. The skills in your Claude environment refer to `azeroth-idle.html`, so a readable copy or the source project probably lives on your computer.

## What this means for the port

**Best case: you add the source project.** The `src/` folder, `package.json` and Vite config. Then the port is a refactor from Solid source into the package layout, and the UI can stay in Solid, which is a good fit for ticking numbers and avoids a rewrite. The design's React choice was made for agent fluency in the absence of source; with Solid source in hand, Solid wins.

**Without source, the port is still feasible, with a stronger oracle than most ports get.**

1. Content tables are extractable from the bundle: ids, names and numbers are intact; only code identifiers are mangled. A one-off script pulls every object literal with an `id` into draft JSON for the new `content` package, applying a rename map to the Dry River names.
2. Formulas are recoverable two ways: an agent reads the minified logic (structure intact, names mangled, the same task the reverse-engineering research shows agents do well), and **behavioral parity tests** check the result: Playwright drives the legacy build headless through scripted sessions, reads state from the save JSON, and the new core must reproduce the same progression for the same actions within tolerance. The old game becomes the reference implementation for the new one. This is the "agent plus oracle" pattern from `research/06-ai-reverse-engineering-games.md`, applied to your own code.

Either way the new game is built alongside the old one, which stays as the reference.

## Repo layout plan for `azeroth-idle`

```
legacy/index.html          current bundle, reference only (moved from the root in M0)
packages/core              deterministic simulation
packages/content           JSON content with schemas, plus rename-map.json
packages/ui                Solid or React, per the source decision
packages/sim               headless runner, policies, parity harness
packages/tools             content linter, forbidden-terms lint, legacy extractor
apps/web                   PWA build for itch.io and your domain
apps/desktop               Electron plus Steamworks
SPEC.md CONSTITUTION.md ROADMAP.md features.json CLAUDE.md DECISIONS.md
.claude/settings.json .claude/skills/
.github/workflows/ci.yml
```

Moving `index.html` to `legacy/` only matters if GitHub Pages serves this repo's root. The separate `azeroth-idle-github-pages` repo suggests the live game is served from there, so the move should be safe; confirm before M0.

## Adjustments to the Track B design

- Add a **legacy parity harness** to M0: Playwright drives the legacy bundle through N scripted sessions and records reference traces; the new core must match for every ported system before that system counts as done.
- Add a **legacy content extractor** to M0: bundle to draft JSON with the rename map applied; the forbidden-terms lint runs on the output.
- Stack decision for the UI is contingent on source availability: Solid if the source arrives, React otherwise.
- Keep the repo private and rename it once the game has a final name (Q-010).

## Resolution (2026-11-01, later the same day)

You confirmed the game only ever existed as the one file, and every one of the 14 versions in the repo's history is a compiled bundle (first upload 1.40 MB with a 385K-character line; latest 1.85 MB with a 443K-character line; zero readable function names in any version). The source was never committed and probably never existed as a separate project on your side. Track B proceeds on the extraction-plus-parity path described above, with React for the UI since there is no Solid source to carry over. If you ever find a folder with `src/` and `package.json` in your "Claude Work" directory, upload it and the port gets faster, but nothing waits on it.
