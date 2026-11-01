# Decision log

One line per decision. Open questions at the bottom are what you answer each week; the planner routine turns answers into decisions and issues.

## Decisions

| ID | Date | Decision | Decided by |
|---|---|---|---|
| D-001 | 2026-11-01 | Pursue Track A (public-records data product) and Track B (original-IP idle game) in parallel on a shared pipeline. | You |
| D-002 | 2026-11-01 | `main` is the integration branch; work arrives on `claude/*` branches via PRs. The GitHub default-branch setting still needs to be switched to `main` in the repository settings. | You and Claude |
| D-003 | 2026-11-01 | Design before building: `design/00-studio-pipeline.md`, `design/01-track-a-public-records-data-product.md`, `design/02-track-b-idle-game.md` are the specs the first issues derive from. | You |
| D-004 | 2026-11-01 | Separate repos. Three in total: `IdeasMoney` (control plane), a new repo for Track A (to be created by you), and the existing `sebob2491/azeroth-idle` repo for Track B. The current `index.html` stays as the design reference in `legacy/` while the new game is built alongside it. | You |
| D-005 | 2026-11-01 | Track B setting defaults to Option 1, Sonoran myth, working title "Dry River" (see `design/03-track-b-setting-options.md`). You may veto or switch any time before the public web alpha. | Claude, on your request to decide |
| D-006 | 2026-11-01 | The `azeroth-idle` repo is attached to this environment with push access. The companion `azeroth-idle-github-pages` repo was not attached (permission denied by the session's classifier); its deployment is unaffected until you say otherwise. | Claude |
| D-007 | 2026-11-01 | Track A repo `sebob2491/records-platform` created (empty, private) and attached to this environment with push access. | You and Claude |
| D-008 | 2026-11-01 | The legacy `index.html` in `azeroth-idle` stays exactly where it is. The new game is built in subfolders beside it, so no deployment can break and nothing needs confirming. | Claude |
| D-009 | 2026-11-01 | Budget is governed by plan usage, not dollars. Default throttle: one builder session per track per night, five nights a week; planner once a week; maintenance twice a week; watchdogs daily. Measured against the plan's usage window after week one and adjusted. Real-dollar costs are only infrastructure: Track A roughly $50 to $150 per month once live, Track B near zero before Steam. | Claude, on your note that you go by usage |
| D-010 | 2026-11-01 | No readable source exists: all 14 historical versions of `azeroth-idle/index.html` (Sept 12 to Oct 8, 2026) are compiled bundles with mangled identifiers. Track B proceeds on the extraction-plus-parity path: content tables lifted from the bundle, formulas recovered by an agent and verified against the legacy game running headless in Playwright. UI framework is React per the design, since there is no Solid source to carry over. | Claude, from your answer to Q-011 |
| D-011 | 2026-11-01 | Design-complete before any build. You asked for everything to be designed first and no work done yet. The two repo-setup agents launched minutes earlier were stopped while still reading the designs; neither created branches, files or issues, with one exception recorded in D-012. The Phase 0 scouting session continues because its output (the vertical choice) is a design input, not a build. | You |
| D-012 | 2026-11-01 | The Track A setup agent pushed one commit to `records-platform` `main` before it was stopped: a README of one paragraph pointing at the designs. Left in place as harmless; say the word and it is removed. | Claude, disclosed |
| D-013 | 2026-11-01 | The design package is defined in `design/05-master-plan.md` Section 1. Building starts only at Gate G0 (your approval of the package). | You |
| D-014 | 2026-11-01 | Phase 0 scorecard delivered (`research/phase0-scorecard.md`, 64-entry legal register draft) and merged into the design branch. Verdict: the contractor-licensing hypothesis survives only repositioned as the normalized record set with bond, insurance, discipline, entity resolution and bulk delivery; TradesAPI and Mesh already own per-license verification and monitoring. Decision brief: `design/08-track-a-vertical-decision.md`. | Claude, from the scouting session |

## Open questions

| ID | Question | Recommendation | Needed by |
|---|---|---|---|
| Q-001 | ~~Separate repos or monorepo?~~ Closed by D-004. | | |
| Q-002 | Track A vertical. Scorecard is in. Choose: A, contractor licensing repositioned as the normalized record set, then liquor licenses on the same framework; B, liquor licenses first; C, stop Track A. See `design/08`. | A | With Gate G0 |
| Q-003 | Track A product name, domain, legal entity, Stripe account. | A neutral descriptive name; an LLC before the first paid customer | Before M3 |
| Q-004 | ~~Track B setting.~~ Defaulted by D-005 to Sonoran myth, "Dry River." Veto window open until the public web alpha. | | |
| Q-005 | ~~Where does the existing game go?~~ Closed by D-004: Track B is built inside the `azeroth-idle` repo. | | |
| Q-006 | Track B art approach and budget: CC BY icon set now, commissioned set before the Steam page, roughly $2K to $5K. | Yes | Before M3 |
| Q-007 | ~~Monthly cap.~~ Reframed and defaulted by D-009. Only open point: tell me if the nightly routines ever block your own interactive use, and I will thin the schedule. | | |
| Q-009 | ~~Create the Track A repo.~~ Closed by D-007. | | |
| Q-010 | Rename the `azeroth-idle` repo once the game has a final name, so the Blizzard mark leaves the project URL. Keep it private until then. | Rename at M3 | Before the Steam page |
| Q-011 | ~~Where is the readable source?~~ Closed by D-010: it does not exist. One cheap check remains worth doing if convenient: look in the "Claude Work" folder for any subfolder containing `src/` or `package.json`. Original text: Where is the readable source of the game? The repo holds only the compiled bundle. Look in the "Claude Work" folder on your DESKTOP-GUGKJK5 computer for a project folder containing `src/` and `package.json` (or the readable `azeroth-idle.html`). Zip that folder without `node_modules` and upload the zip to the `azeroth-idle` repo with GitHub's "Add file, Upload files" button, the same way the early commits were made. | Find and upload; if it is gone, say so and the extraction path starts | Before Track B M0 port work |
| Q-012 | Gate G0: approve the design package (docs 00 to 12 plus the Phase 0 scorecard when it lands), or list what to change. Until then nothing is built. | Approve after reading 05 (master plan) and skimming the rest; raise changes as comments or edits | Whenever you are ready; no default |
| Q-008 | Accounts: two GitHub repos with access for this environment, Stripe, Cloudflare, Fly or Render, Neon or Supabase, Apify, Sentry, itch.io, Discord; Steamworks by M3. | Open in the order the milestones need them | Rolling |
