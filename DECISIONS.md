# Decision log

One line per decision. Open questions at the bottom are what you answer each week; the planner routine turns answers into decisions and issues.

## Decisions

| ID | Date | Decision | Decided by |
|---|---|---|---|
| D-001 | 2026-11-01 | Pursue Track A (public-records data product) and Track B (original-IP idle game) in parallel on a shared pipeline. | You |
| D-002 | 2026-11-01 | `main` is the integration branch; work arrives on `claude/*` branches via PRs. The GitHub default-branch setting still needs to be switched to `main` in the repository settings. | You and Claude |
| D-003 | 2026-11-01 | Design before building: `design/00-studio-pipeline.md`, `design/01-track-a-public-records-data-product.md`, `design/02-track-b-idle-game.md` are the specs the first issues derive from. | You |

## Open questions

| ID | Question | Recommendation | Needed by |
|---|---|---|---|
| Q-001 | Separate repos per track, or a monorepo in this repo under `tracks/`? | Separate repos: different secrets and deploys, faster CI, one clone per scheduled run | Before setup week |
| Q-002 | Track A vertical. Confirm or override the leading hypothesis (contractor licensing and enforcement events) once the Phase 0 scorecard is in. | Decide from the scorecard, not before | End of week 3 |
| Q-003 | Track A product name, domain, legal entity, Stripe account. | A neutral descriptive name; an LLC before the first paid customer | Before M3 |
| Q-004 | Track B setting: Sonoran myth, Abyss expedition, The archive, or plain fantasy. Working title. | Sonoran myth | Before M0 |
| Q-005 | Add `azeroth-idle.html` and its skills to the Track B repo (or here) so the systems and tables can be ported? | Yes, to the Track B repo | Before M0 |
| Q-006 | Track B art approach and budget: CC BY icon set now, commissioned set before the Steam page, roughly $2K to $5K. | Yes | Before M3 |
| Q-007 | Monthly cap on agent compute and infrastructure, and the horizon (six or twelve months). | Measure week one, then cap | Setup week |
| Q-008 | Accounts: two GitHub repos with access for this environment, Stripe, Cloudflare, Fly or Render, Neon or Supabase, Apify, Sentry, itch.io, Discord; Steamworks by M3. | Open in the order the milestones need them | Rolling |
