# All open questions in one place

Status: 2026-11-01. Every question the design package asks you, with the recommended default. Answer by editing the Answer column (or reply in the digest email once routines exist). Anything left blank proceeds on the default when Gate G0 passes, except the three marked HOLD, which wait for you.

## A. Gate and track-level (from DECISIONS.md)

| ID | Question | Default | Answer |
|---|---|---|---|
| Q-012 | Approve the design package (Gate G0)? | HOLD: no default | |
| Q-002 | Track A vertical: A (contractor licensing repositioned, then liquor), B (liquor first), C (stop Track A). See design 08. | A | |
| Q-003 | Track A product name, domain, legal entity, Stripe. | Codename `records-platform`; LLC before first paid customer | |
| Q-006 | Track B art budget: CC BY icons now, commissioned set before Steam, roughly $2K to $5K. | Yes | |
| Q-008 | Accounts, opened as milestones need them. | Follow the digest | |
| Q-010 | Rename the `azeroth-idle` repo at the final name. | At M3 | |
| Pricing | Design 07 keeps design 01's $299 and $799 tiers; the scorecard (design 08) proposes Free, $149, $499, $999 and enterprise from $15K after seeing TradesAPI at $99. Plans are configuration (`billing/plans.yaml`), so this is an M3 decision. | Adopt the design 08 tiers as the M3 hypothesis; test in the buyer conversations | |

## B. Track A technical (design 07, Section 14)

| ID | Question | Default | Answer |
|---|---|---|---|
| T-01 | Neon or Supabase for Postgres | Neon | |
| T-02 | Fly.io or Render | Fly.io | |
| T-03 | Feed credits: 100 events per credit (versus one per event in design 01) | Keep 100:1; revisit at M3 | |
| T-04 | Public pages for sole proprietors | No pages; API-only with an `is_individual` flag until counsel says otherwise | |
| T-05 | Auto-accept very high-confidence fuzzy entity merges | No, until 200 human decisions exist | |
| T-06 | Residential-address heuristic versus paid USPS lookup | Heuristic now | |
| T-07 | Snowflake or BigQuery first for warehouse shares | Snowflake | |
| T-08 | Grafana now or later | Fly metrics plus Sentry until M3 | |
| T-09 | Domain and bot contact address for the crawler's User-Agent | Needed before the first live crawl; HOLD until Q-003 | |
| T-10 | Who approves recorded fixtures | You merge `fixture-update` PRs | |
| T-11 | Arizona sources in M2 | Exclude until counsel review | |
| T-12 | Raw document retention | 7 years with object lock | |

## C. Track B technical (design 10, Section 14)

| ID | Question | Default | Answer |
|---|---|---|---|
| Q-B1 | Parity first (exact legacy numbers), then tune | Parity first | |
| Q-B2 | New content ids or legacy slugs | New ids with `legacyId` provenance | |
| Q-B3 | Offline cap flat 24 hours or upgradeable | Flat 24 hours in M1 | |
| Q-B4 | Fate of the legacy Thieving skill | Fold its risk mechanic into Trapping and Prospecting | |
| Q-B5 | Which 10 dungeons port first | By mechanic variety, chosen in M2 | |
| Q-B6 | Tick loop on main thread or worker | Main thread with drift correction | |
| Q-B7 | Export-code format | Compressed `DR1.` with uncompressed fallback | |
| Q-B8 | Steam Cloud via API or Auto-Cloud | API | |
| Q-B9 | Disable achievements under balance-altering mods | Yes | |
| Q-B10 | Where the new web build deploys while the legacy file stays at the root | `/next/` on a Pages branch until the rename | |
| Q-B11 | Telemetry host | Cloudflare Worker and D1, or off if the privacy text is not approved | |
| Q-B12 | React state library | Zustand with selectors | |
| Q-B13 | Monster families: legacy ten or new nine | Nine Dry River families | |
| Q-B14 | Simulation policy decision interval | 10 simulated seconds | |

## D. Track B game design (design 09, Section 17)

| ID | Question | Default | Answer |
|---|---|---|---|
| GD-01 | Fifteen skills with Cartography leveled, or fourteen | Fifteen | |
| GD-02 | Is a Monsoon ever forced, or always optional | Optional, with an overfill cap at 300% | |
| GD-03 | Steam price $7.99, $8.99 or $9.99 | $8.99 | |
| GD-04 | Tool tiers persist through the first monsoon free, or as the first cheap watermark purchase | First purchase | |
| GD-05 | Flooded areas last 20 active hours, or until the next monsoon | 20 active hours, then they become Silt Flats | |
| GD-06 | Ranged weapons: bows, slings and knives only, or admit one late "lightning rod" | Admit one, from the Rookery | |
| GD-07 | Keep "Dry River" as the final title if the trademark search is clean | Yes, with a subtitle decided at M3 | |
| GD-08 | Ledger quest text agent-drafted and human-edited, or human-written only | Agent-drafted to a brief; every line edited and logged | |
| GD-09 | Web cloud saves with accounts before Steam | No; file export until Steam Cloud | |
| GD-10 | Ranged skill named Marksmanship or Hunting | Marksmanship | |

## E. What needs a lawyer (design 11, design 08)

Not questions for you to answer, but money to approve: an FCRA memo on sole-proprietor licensees sold to lenders and insurers; Arizona's commercial-purpose statute and computer-tampering statute as applied to the Registrar of Contractors; the no-CAPTCHA policy under DMCA 1201; defamation exposure for stale "revoked" events; Vermont's 2027 data-broker scope change. Budget a two-week turnaround before Track A's M3 billing goes live. HOLD on counsel engagement until you say so.
