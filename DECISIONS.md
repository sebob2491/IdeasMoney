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

## Open questions

| ID | Question | Recommendation | Needed by |
|---|---|---|---|
| Q-001 | ~~Separate repos or monorepo?~~ Closed by D-004. | | |
| Q-002 | Track A vertical. Confirm or override the leading hypothesis (contractor licensing and enforcement events) once the Phase 0 scorecard is in. | Decide from the scorecard, not before | End of week 3 |
| Q-003 | Track A product name, domain, legal entity, Stripe account. | A neutral descriptive name; an LLC before the first paid customer | Before M3 |
| Q-004 | ~~Track B setting.~~ Defaulted by D-005 to Sonoran myth, "Dry River." Veto window open until the public web alpha. | | |
| Q-005 | ~~Where does the existing game go?~~ Closed by D-004: Track B is built inside the `azeroth-idle` repo. | | |
| Q-006 | Track B art approach and budget: CC BY icon set now, commissioned set before the Steam page, roughly $2K to $5K. | Yes | Before M3 |
| Q-007 | Monthly cap on agent compute and infrastructure, and the horizon (six or twelve months). | Measure week one, then cap | Setup week |
| Q-009 | Create the Track A repo and grant this environment access. Suggested name `records-platform` (a codename; GitHub redirects on rename once the product is named). Private. | Create it empty, no README | Before Track A setup |
| Q-010 | Rename the `azeroth-idle` repo once the game has a final name, so the Blizzard mark leaves the project URL. Keep it private until then. | Rename at M3 | Before the Steam page |
| Q-008 | Accounts: two GitHub repos with access for this environment, Stripe, Cloudflare, Fly or Render, Neon or Supabase, Apify, Sentry, itch.io, Discord; Steamworks by M3. | Open in the order the milestones need them | Rolling |
