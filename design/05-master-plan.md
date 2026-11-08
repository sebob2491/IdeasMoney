# Master plan: gates, calendar, metrics, risks, and the setup-week checklist

Status: design, 2026-11-01. This is the document that ties the package together. Nothing is built until Gate 0 passes.

## 1. The design package

| Doc | Title | Status |
|---|---|---|
| 00 | Studio pipeline (overview) | Done |
| 01 | Track A product design (overview) | Done; vertical pending Phase 0 |
| 02 | Track B game design (overview) | Done |
| 03 | Track B setting options and default | Done; default Sonoran myth, "Dry River" |
| 04 | Track B port inventory | Done; no source exists, extraction plus parity |
| 05 | Master plan (this document) | Done |
| 06 | Pipeline detailed specification | Done |
| 07 | Track A technical design | Done |
| 08 | Track A vertical decision brief | Done; your answer to Q-002 pending |
| 09 | Track B game design document | Done |
| 10 | Track B technical design | Done |
| 11 | Legal and compliance | Done |
| 12 | Naming shortlists and process | Done |
| 13 | All open questions in one place | Done; the page you answer |
| research/phase0-scorecard.md | Phase 0 vertical scorecard | Done |

## 2. Operating model

Three parties. **You** set direction: answer the questions in `DECISIONS.md`, merge PRs, play the game build, approve money and legal steps. **Routines** do the daily work: nightly builders, reviewers, planner, maintenance, watchdogs, releases, all as scheduled cloud sessions. **Interactive sessions like this one** do design, setup, incident response, and anything that needs judgment the routines flag as `needs-human` and you then delegate.

## 3. Gates

Each gate is a yes or no with evidence. Nothing proceeds past a gate without your word, except where a default is written.

| Gate | Criterion | Evidence | Default if you say nothing |
|---|---|---|---|
| G0 Design approved | You have read the package, the open questions have answers or accepted defaults, and the design branch is merged | `DECISIONS.md` shows Q-012 answered | Hold |
| G1 Pipeline green | Both repos have skeletons, all gates pass on the skeleton, routines exist and the first three nightly PRs merged cleanly | Digest week 1 | Proceed if green |
| A1 Vertical chosen | Phase 0 scorecard accepted; kill criteria passed; legal register started | `design/08` written | Hold |
| A2 Platform proven | Three real sources within SLA for two consecutive weeks; parity between two parsers on the top source | Watchdog report | Proceed |
| A3 First revenue | Billing live; first self-serve paying customer | Stripe | Proceed |
| A4 Traction | Ten paying customers or two enterprise pilots | Stripe and contracts | Decide: scale coverage or pivot vertical |
| B1 Fun | You play the vertical slice for an hour and want to keep going | Your one-line verdict | Hold until yes |
| B2 Audience | 500 weekly active players on the free web alpha, or a clear signal to change course | Analytics | Decide |
| B3 Steam page | Page live, wishlists growing week over week for four weeks | Steamworks | Proceed |
| B4 Launch | About 7,000 wishlists, demo shipped at Next Fest | Steamworks | Launch |

## 4. Integrated calendar

Week 0 is the setup week after G0. Dates are relative because G0 depends on you.

| Weeks | Track A | Track B | You |
|---|---|---|---|
| 0 | Repo skeleton, gates green, routines on; Phase 0 scorecard delivered | Repo skeleton, gates green, routines on; extractor and parity harness running against the legacy game | Read scorecard; answer Q-002; one hour |
| 1 to 3 | Vertical decision (A1); legal register for first ten sources; three sources live on fixtures then real | Core ported for two skills with parity passing; content schemas; first Dry River content pack | Weekly hour; two or three buyer conversations for Track A |
| 4 to 6 | Platform milestone M1: adapter framework, canonical schema, raw store, three sources within SLA (A2 starts the two-week clock) | Vertical slice: four skills, combat, one area, shop, saves, offline progress; private alpha | Play the slice (B1) |
| 7 to 12 | Fifteen states; event derivation; API v0 with keys; docs; dashboard | Full skill set, areas, dungeons, prestige v1, achievements, data-pack mods; public web alpha; Discord; feedback thread | Weekly hour; post as the human face |
| 13 to 18 | Thirty-plus states; webhooks; bulk exports; Stripe billing; free tier; jurisdiction pages; MCP connector; Apify actors (A3) | Polish; commissioned art; accessibility pass; Electron and Steamworks | Approve pricing; sign Stripe; choose artist |
| 19 to 20 | Outbound to thirty named buyers begins (agent drafts, your name) | Steam page live with screenshots and trailer; demo build (B3) | Steam account; approve trailer |
| 21 to 26 | All states plus major city regimes; warehouse share; security and privacy docs; A4 decision | Next Fest demo (Feb 2027); launch preparation | Monthly review |
| 27 to 28 | Steady state: adjacent record sets by scorecard | Launch 1.0 on Steam when B4 is met; post-launch cadence | Launch day presence |

## 5. Cadence

- **Nightly:** builders, maintenance, watchdogs.
- **Weekly (Sunday):** planner writes the digest and the questions; you answer within the week.
- **Monthly:** a one-page metrics review against Section 6 targets, written by the planner, read by you; gates are evaluated here.
- **Quarterly:** re-run the scorecard logic on Track A's adjacent record sets; re-check Track B against the genre's release volume and the wishlist curve.

## 6. Metrics and targets

| Area | Metric | Target by week 12 | Target by week 26 |
|---|---|---|---|
| Pipeline | PRs merged per week, both tracks | 15 | 25 |
| Pipeline | Revert rate per 100 merged PRs | Under 3 | Under 2 |
| Pipeline | Mutation score on changed files | Above 50% | Above 60% |
| Pipeline | Issues labeled needs-human per week | Under 8 | Under 5 |
| Track A | Sources live, within SLA | 15, 90% | 51 plus cities, 95% |
| Track A | Events derived per day | Measured | Growing |
| Track A | API keys, paying customers, MRR | Free keys 50, paying 0 | Paying 10, MRR $3K to $8K |
| Track B | Simulation bands met | All M1 bands | All M2 bands |
| Track B | Content counts vs targets | M2 table | 1.0 table |
| Track B | Weekly active players, day-7 retention | 500, 20% | 2,000, 25% |
| Track B | Steam wishlists | Page not yet live | 7,000 |

## 7. Usage and cost plan

Revised 2026-11-08 (D-019 to D-022): the Actions allowance ran out in week one after about 160 CI runs per repo. CI now runs three jobs per PR, heavy suites run nightly or weekly, a $15 monthly Actions spending limit is requested (Q-014), and routine work runs on Sonnet 5.5 in two long-lived worker sessions with the reviewer in a fresh subagent. The top model is reserved for design and incident sessions in this conversation.

Agent work draws on your plan's usage, not dollars. Default throttle (D-009): one builder session per track per night, five nights a week; one planner per week; maintenance twice a week; watchdogs daily. Week 1 measures the share of your allowance this consumes; the digest reports it; the schedule thins if it crowds out your own use. Real-dollar costs: Track A infrastructure $50 to $150 per month from M1 onward, rising with traffic; domains about $15 per year each; Track B near zero until the artist commission ($2K to $5K) and the Steam fee ($100 per app).

## 8. Risk register

| # | Risk | Likelihood | Impact | Mitigation | Trigger to act |
|---|---|---|---|---|---|
| 1 | Routines consume more usage than expected and block your own sessions | Medium | Medium | Throttle defaults; weekly usage line; thin schedule | You report being blocked |
| 2 | Agents game their own tests | Medium | High | Protected test paths, mutation floors, fresh-context review, evidence check | Mutation score under floor two nights running |
| 3 | Track A vertical fails kill criteria | Medium | High | Phase 0 before any adapter work; scorecard alternatives ranked | Scorecard verdict |
| 4 | Portal changes break sources | High | Medium | Freshness SLAs, anomaly pausing, maintenance routine | Watchdog issue |
| 5 | Legal exposure on a source (terms, state copyright, FCRA) | Low to medium | High | Legal register before enabling; counsel review before launch; permissible-use terms | Counsel flags |
| 6 | Track B is balanced but not fun | Medium | High | B1 gate is your subjective verdict; weekly play | Your verdict |
| 7 | AI stigma hurts Track B reception | Medium | Medium | Original setting, commissioned art, disclosure, community first | Negative feedback pattern |
| 8 | Scope creep | High | Medium | features.json as registry; planner enforces milestone targets | Features added outside milestones |
| 9 | Platform changes (Shopify-style) in Steam, Apify, Stripe terms | Low | Medium | Own the customer relationship and data; marketplaces as channels | Policy change notice |
| 10 | Single-person dependency (you) | Medium | Medium | Everything in repos and DECISIONS.md; routines idle safely when unanswered | Two weeks without answers |
| 11 | Secrets leak through agent logs | Low | High | Gitleaks in hooks and CI; routines never hold production write secrets | Gitleaks hit |
| 12 | Legacy content extraction misses formulas | Medium | Medium | Parity harness gates "ported" status; the old game stays the oracle | Parity failures |

## 9. Setup-week checklist (runs only after G0)

Ordered and mechanical. Items marked YOU are yours.

1. YOU: merge `claude/track-designs` into `main`; set `main` as the default branch.
2. YOU: answer Q-012 (approve the package) and confirm the defaults you accept.
3. Track B repo: create branch `claude/setup-pipeline`; add the files in design 06 Section 1 with Track B values; hooks and scripts; CI; issue and PR templates; labels; monorepo skeleton per design 10; run every gate to green; push; open the setup PR for your merge.
4. Track A repo: same, with Track A values and the Python skeleton per design 07; push; setup PR.
5. Both repos: branch protection per design 06 Section 3; CODEOWNERS with your handle.
6. Both repos: first 20 to 30 issues from the milestone breakdowns in designs 07 and 10, labeled `ready` with sizes.
7. Routines: create Builder A, Builder B, Reviewer, Planner, Maintenance, Watchdog A, Watchdog B with the prompts in design 06 Section 4 and the schedules in Section 7 above; disabled until the setup PRs merge, then enabled.
8. YOU: merge the two setup PRs.
9. First night runs; morning check by an interactive session; fix anything that broke the gates; G1 evaluated after three merged nightly PRs.
10. YOU, rolling: accounts from `DECISIONS.md` Q-008 as milestones need them.

## 10. Change control

Designs change through pull requests to `design/` with a one-line entry in `DECISIONS.md`. Structural technical choices inside a track repo get an ADR in that repo's `docs/adr/`. The planner may propose design changes; only you approve them, by answering the question in the digest.
