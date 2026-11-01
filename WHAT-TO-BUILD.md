# What to build with an AI coding agent in 2026, and how to build it hands-off

Research date: 2026-11-01. Seven research tracks (roughly 1,100 tool calls and about 700 cited URLs) feed this document. The raw, cited reports are in `research/`. Every number below traces to one of them; figures tagged "self-reported" are the founder's own claim, "estimate" means a third-party tracker, "verified" means read from a payment provider or published by the platform.

---

## 1. The short answer

**Code is no longer the scarce input.** With a capable agent, writing software is cheap. What still costs money, and therefore what you get paid for, is distribution, proprietary data, integrations, trust, and verified reliability. A months-long hands-off build should pour agent hours into the moats an agent can build alone (data coverage, integration breadth, test depth) and borrow distribution from a marketplace or a self-serve channel, so you are not the sales team.

**Recommended portfolio: one money track and one asymmetric bet, run in parallel by the same pipeline.**

| Track | What | Why it fits "long build, hands-off, sellable" | Realistic outcome |
|---|---|---|---|
| **A. Money (primary)** | A public-records data product in a vertical nobody has normalized, sold self-serve by usage (API, warehouse shares, Apify actors) and later wrapped in a thin SaaS. Model: Shovels (permits), Zoneomics (zoning). | Agent fit 9/10. Coverage of hundreds of jurisdictions is months of tedious, testable work an agent does well on a schedule. Usage billing and developer buyers mean almost no support. Data businesses sell as sticky recurring revenue. | Shovels went from founder-disclosed ~$6K ARR (2023) to est. $3M ARR (2026) with VC and a team; a solo version that reaches $5K to $20K MRR sells for roughly 2.5 to 4x annual profit. |
| **A-alt. Money (if you want a faster first dollar)** | A Shopify app in a niche Shopify has not nativized: mix-and-match configurator bundles, advanced B2B (quoting, rep workflows, net-terms dunning), multi-driver routing with proof of delivery, pre-orders with partial payments. | Agent fit 8/10. Shopify supplies billing and discovery; developers keep 100% of the first $1M lifetime. Solo apps cluster at $2K to $20K MRR and sell for 3 to 5x revenue. | Median listed app grosses $2.5K to $5.2K per month; 77% of new apps have zero reviews. Support and review-farming are the human cost. |
| **A-alt 2. Money (only if you will sell to governments)** | A WCAG 2.1 AA compliance-evidence system for small public entities: monitoring, remediation tickets with fixes, PDF remediation, an exception register, and a monthly attestation. Not another scanner. | The biggest datable US software-demand event of 2027 to 2028. DOJ's Title II deadlines are April 26, 2027 (population 50K+) and April 26, 2028 (smaller entities and special districts); the universe is 91,438 local governments including 12,535 school districts; a pending National Federation of the Blind suit could pull the dates forward. | Procurement is slow and human, scanning is commoditized, and CMS vendors bundle checkers. Value 6 to 7, hands-off 4. Only the pick if you or a partner will do government sales. |
| **B. Asymmetric bet** | An original-IP idle/incremental game, free web build first to grow a community, then a paid Steam release. Reuse the systems from your existing Melvor-style game under new IP. | Agent fit 9/10 and the most hands-off thing on the list (no servers, no churn, simulation-based balancing is a natural agent loop). | Deep-systems comps (Magic Research 1 and 2, Increlution) grossed $38K to $90K lifetime on Steam; the median 2025 Steam release made $318. Treat as a lottery ticket with high learning value, not income. |

**Do not build:** paid MCP servers (no payment rails anywhere; ship MCP as a free front door to a paid API instead), open-source agent-eval devtools (four acquisitions in nine months, every incumbent has a free tier), boilerplates (ShipFast fell ~99% from peak), AI wrappers, SEO content sites, Chrome extensions as a business, and anything that uses another publisher's names, maps, models or audio. Blizzard, Activision and Nintendo all took down fan recreations in 2025 and 2026, including AI-built browser ports, within weeks of them going viral.

**How to build it:** the single most consistent finding across every source is that *autonomy scales with cheap, independent verification, not with model quality*. Spend the first two weeks building oracles (tests the implementing agent cannot edit, mutation-score floors, property tests, a browser or simulator the agent must satisfy) and the agents will spend the following months running into them. Section 6 is a concrete pipeline for this repo.

---

## 2. What the evidence says about money

### 2.1 Base rates are brutal and well measured

- Mobile: the median subscription app makes under $50/month a year after launch; about 17% ever reach $1K/month (RevenueCat, 75K apps, verified). The 2026 report shows the share reaching $1K MRR within two years *fell* from 19% to 17% as vibe-coded apps flooded in.
- Steam: roughly 20,000 releases in 2025. Trackers disagree on the median (about $249 to $1,000 gross) but agree most games earn under about $1K; 23.4% cleared $5K (Gamalytic estimate).
- Shopify: 77% of apps launched in 2026 have zero reviews (up from 52% in 2024); the median listed app grosses $2.5K to $5.2K/month (Barn2 analysis of 1,675 apps, Oct 2026).
- MCP servers and ChatGPT/Claude apps: no evidence anyone is paid for a server as such. OpenAI's Apps SDK bans selling digital goods or subscriptions. Anthropic's directory terms contain no revenue share. Smithery shut its proxy Oct 31, 2026. The one platform that pays is Apify: $1.7M/month to 5,600 scraper developers at an 80% share, average about $300 each.

### 2.2 Where the right tail is

Verified six-figure-MRR small products cluster in two shapes:

1. **Usage-priced developer APIs and data utilities.** 1Lookup (phone/email/IP validation) at $272,886 MRR on 749 subscriptions; ScreenshotOne (solo) at ~$269K ARR. The moat is reliability and edge cases, which is exactly what months of agent iteration buys.
2. **Narrow B2B tools with a measurable output or a regulated niche.** Kibu, an EHR for disability-services providers, at $234,319 MRR from 375 customers (about $625 each per month, "contact sales"). Rezi at $241K MRR. Team sizes for these are not public.

Platform-vertical products are the third shape: Kaching's Shopify bundle app (first app $6.5K MRR sold for $250K, portfolio now six-figure MRR, self-reported), Rick Blyth's Amazon-seller Chrome extensions (~$10K MRR, exit at 57x monthly profit), Barn2's WooCommerce plugin ($500K lifetime). The platform supplies distribution; the niche supplies pricing power.

### 2.3 What a small software business sells for

| Source | Multiple |
|---|---|
| Flippa 2025 closed deals | SaaS 2.7x annual profit average, top quartile 5.8x; apps 2.4x |
| BizBuySell 2021 to 2025 | 2.4x to 4x seller's discretionary earnings, midpoint 3.1x |
| Shopify apps (observed) | 3x to 5x annual revenue |
| Content/affiliate sites | 24x monthly profit and falling, volume down ~57% |

A product making $5K/month profit realistically sells for $150K to $240K. Buyers pay up for Stripe-verifiable recurring B2B revenue, diversified acquisition, clean books, and documented operations. They discount algorithm dependence, single-channel risk, and anything a vendor plugin could replace.

### 2.4 What is dying

Boilerplates (ShipFast: ~$133K/month peak in 2024 to $1,395 in the last 30 days on the founder's own dashboard), AI wrappers (the sharpest correction in SimpleClosure's 2025 shutdown report), SEO content sites (Flippa content deals down 33.5%), generic horizontal SaaS (public software indices fell ~25% after Claude Cowork plugins shipped in January 2026), MCP directories as businesses.

### 2.5 Platform risk is the recurring theme

Shopify moved its $1M zero-commission exemption from annual to lifetime (June 2025). Anthropic cut Windsurf's model access with under five days' notice. Cursor's pricing change blew up user budgets overnight. Chrome's Manifest V3 removed thousands of extensions. Google rewrote the affiliate-extension model after Honey. Design for this: own the customer relationship and the data; treat any marketplace as a channel, not a home.

---

## 3. The framework

| Moat | Can an agent build it unattended? | Notes |
|---|---|---|
| Distribution (marketplace rank, SEO authority, audience) | Partly. A marketplace does the selling; SEO and audience need a human. | The most important variable for a hands-off project. |
| Proprietary, continuously maintained data | Yes. Months of collection, normalization and re-verification on a schedule. | Legal posture and freshness must be designed in. |
| Long-tail integrations | Yes. Each connector is a boring week for a human and an hour for an agent. | 200 of them is a real moat. |
| Depth of content or features | Yes. | The "it would take a team years" product. |
| Verified reliability | Yes, if the oracles are built first. | See Section 5. |
| Trust and compliance | No. Needs a human name and time. | Regulated niches reward this. |
| Workflow lock-in | Partly. The product can be built; customers must be won. | |
| Taste and design | No. | Generic AI output is recognizable, and on Steam it is now filtered. |

Scores used below, each 1 to 10: **Fit** (how much of the work is deterministic, testable, data- or content-heavy), **Hands-off** (how little human time per week once running, including sales and support), **Value** (realistic revenue or sale price for a solo owner), **Moat**, and **First dollar** (higher is sooner).

---

## 4. Ranked ideas

### Tier 1

#### 4.1 Public-records data product, sold self-serve by usage

**What.** Pick a public-records vertical that no vendor has normalized across jurisdictions, build agents that crawl every source system, classify and normalize the records, re-verify on a schedule, and sell the result three ways: a usage-priced API with a free tier and SEO pages per jurisdiction; warehouse shares (Snowflake/BigQuery) for enterprise; and Apify actors as a zero-sales channel for the raw scrapers. Later, wrap the data in a thin SaaS for the people who feel the pain directly.

**Who pays, and what the comps charge.** Shovels sells AI-classified permit and contractor records at $599/month (25K credits) and $999/month, naming insurance, home services, telecom, energy, real estate and building materials as buyers. Zoneomics sells a zoning API at $134 to $549/month to lenders (Kiavi), Redfin and Moody's. Middesk's median buyer pays $14,375/year for business verification. Mosey charges $399 to $1,999/month for multi-state employer compliance.

**Why public records specifically.** The 2026 rulings changed the scraping picture. hiQ v. LinkedIn settled without precedent, and Meta and X both lost logged-out scraping suits against Bright Data in 2024, but Reddit v. Anthropic (a September 2026 ruling that denied dismissal) let breach-of-contract, interference and unfair-competition claims proceed, and Reddit v. Perplexity (S.D.N.Y., July 31, 2026) let DMCA anti-circumvention claims proceed. Scraping consumer platforms behind terms of service for a commercial dataset is now litigation-exposed even when the data is public. Government records carry none of that exposure, statutes and codes are not copyrightable under *Georgia v. Public.Resource.Org*, and Shovels covers only about 2,770 of 20,000+ permitting jurisdictions, so the coverage gap is enormous. Ship a free MCP connector to the API as a distribution channel; the Anthropic directory lists any paid-plan developer's server and ranks by usage.

**Candidate verticals to scout (the first agent job).** Contractor-license status, disciplinary actions and bond/insurance lapses across the 50 state boards (the research found no dedicated vendor; the only Apify actor covers five cities). Also: short-term-rental registrations by city, liquor licenses, code-enforcement cases, food-service inspections, tax-delinquency lists, HOA registrations, water and well permits. The scouting pipeline scores each on: number and format of source systems, update cadence, legal posture (edicts and facts are safe; state-compiled databases vary), existing vendors and prices, named buyer categories, and demand proxies on Apify and RapidAPI.

**Scores.** Fit 9. Hands-off 8. Value 6. Moat 7. First dollar 4.

**Why an agent is the right builder.** Every part of the work is a verifiable loop: parse a page, check it against a schema, diff it against the last crawl, flag anomalies, re-crawl. Differential tests between two scrapers of the same source, property tests on normalizers, and freshness SLAs are the oracles. This is the "months of grind" the user asked for.

**Hands-off reality.** You pick the vertical, approve pricing and terms, and talk to the occasional enterprise buyer. The agent does everything else, including a weekly coverage and freshness report.

**Risks.** Coverage and accuracy are a treadmill across hundreds of jurisdictions and buyers want SLAs. Arizona asserts copyright over state works and requires stating a commercial purpose for public-records requests, so an Arizona-based builder scraping Arizona agencies needs a lawyer's read before launch; federal records are public domain and statutes are edicts under *Georgia v. Public.Resource.Org*. Marketplace demand for raw government scrapers is thin (existing actors have 2 to 7 users), so the API and warehouse channel is where the money is.

**First 30 days.** Scouting pipeline and scorecard (weeks 1 to 3). Choose vertical. Build the first three source adapters with the full test harness, the schema, and the freshness dashboard (week 4).

#### 4.2 Shopify app in a niche Shopify has not nativized

**What.** One app, built on Shopify's Remix and Polaris templates, in a category where the native option is weak or absent. Launch free, reach about 1,000 installs to earn the Built for Shopify badge (one developer reported six months), then price at $19 to $99/month.

**Where it is still open (Nov 2026).** Mix-and-match configurator bundles on the native component-inventory model (Shopify's own free Bundles app sits at 2.8 stars from 571 reviews listing what is missing). Advanced B2B: quoting, sales-rep workflows, EDI, net-terms dunning, the layer above the basic wholesale pricing Shopify made native in 2026. Multi-driver routing with proof of delivery (EasyRoutes has 289 reviews after six years, which reads as either small or under-served). Pre-orders with partial payments.

**Closed by Shopify:** sales-tax nexus (native Tax liability insights and automated filing), basic wholesale pricing and net terms (native B2B on all plans in 2026), fixed bundles (native since 2023). Discounts and bundles are the most crowded shelves.

**Scores.** Fit 8. Hands-off 6. Value 6. Moat 5. First dollar 6.

**Hands-off reality.** Merchants file support tickets and expect answers in hours. An agent can triage and draft replies, but you own the relationship. Shopify deprecates APIs quarterly; a scheduled task can track and migrate.

**Risks.** Platform absorption (three categories nativized in three years) and discovery in a store where star rating "showed no correlation with success." A legitimate shortcut from the Barn2 analysis: buy a stale app with 100+ reviews and rebuild it.

### Tier 2

#### 4.3 Original-IP idle/incremental game, web first, then Steam

**What.** Retheme and deepen the systems you already have (skills, crafting, economy, prestige) under original IP. Ship a free web build to grow a community on r/incremental_games and itch.io, then a $5 to $10 Steam release once wishlists justify it. This is the path Melvor Idle, Magic Research and Increlution took.

**What the comps say (Raijin.gg estimates, Nov 2026).** Melvor Idle ~426K copies, ~$3.4M gross, with a publisher; its sequel is now published by Jagex as an official RuneScape game (Oct 2026), which shows the acquirer path exists. Rusty's Retirement passed 500K copies by Feb 2025 per the developer (trackers' estimates range from ~319K to ~1M). Nodebuster ~339K copies at $2.99 with 10,703 reviews at 97% positive. The deep-systems games closest to this plan: Magic Research 2 ~$90K gross, Increlution ~$90K, Magic Research 1 ~$38K. Net is roughly half of gross after Steam's 30%, refunds and regional pricing. Wishlist math: week-one sales run 0.10x to 0.15x launch wishlists, so a $9.99 game needs ~20K wishlists to sell 2K to 3K copies in week one.

**Scores.** Fit 9. Hands-off 8. Value 4. Moat 3. First dollar 5.

**Why it is still worth doing.** It is the purest showcase for the advanced pipeline in Section 6: a deterministic headless simulator plus scripted player policies gives Monte-Carlo progression curves, and a keep-if-better loop can tune balance against a target band overnight. It costs you direction and playtesting, not money.

**Risks specific to this plan.** The genre is flooded: Steam "idler" releases rose from 329 in 2023 to 940 in 2025 and 1,566 in the first nine months of 2026, up 150%, and PC Gamer counted 54 in a single week. The niche still over-indexes on engagement (5.6% of idlers reach 1,000 reviews versus 4.2% Steam-wide), but supply is exploding and the audience is increasingly AI-sceptical: Steam AI disclosures rose ~800% in 2025, one study found disclosed titles earn fewer reviews, and players have asked Valve for a filter. Player-visible generated content must be disclosed under Valve's rule; backend balance simulation need not. The WoW theme is legally dead: Blizzard obtained a permanent injunction against Turtle WoW in April 2026 covering servers, modified clients and successor projects, after shutting Nostalrius, Felmyst, Everlook and Stormforge. Mechanics are not protectable; names, art, lore and audio are.

#### 4.4 WCAG 2.1 AA compliance-evidence system for small public entities

**What.** Not a scanner. A crawler with axe-class rules that feeds prioritized remediation tickets with code fixes, bulk PDF and DOCX tagging, an exception register for the rule's five exceptions (archived content, pre-existing documents, third-party content, individualized password-protected documents, pre-existing social media), and a monthly conformance report with a board attestation. Priced around $99 to $499 per month per domain for a town of 8,000 or a 1,500-student district, under the purchase-card thresholds that avoid formal procurement (Arizona cities buy under $10K on a card).

**Why it is the biggest datable demand event.** DOJ's interim final rule of April 20, 2026 set compliance at April 26, 2027 for entities serving 50,000 or more and April 26, 2028 for smaller entities and special districts, keeping WCAG 2.1 AA. The National Federation of the Blind is suing to restore the earlier dates, with a summary-judgment motion filed Sept 28, 2026. The covered universe is 91,438 local governments (2025 Census of Governments), including 12,535 independent school districts and 40,199 special districts, most with no accessibility staff. On the private side, 3,117 federal web suits were filed in 2025 and about 25% of September 2026 defendants were running an accessibility overlay when sued, which indicts the incumbent "install a widget" category; the FTC fined accessiBe $1M in April 2025 for compliance claims.

**Scores.** Fit 7. Hands-off 4. Value 6 to 7. Moat 5 (stored compliance history and the government channel). First dollar 4.

**Why it is not Tier 1 for a direction-only owner.** The sale runs through school-board associations, councils of governments, and cooperative purchasing contracts, which is a relationship-driven human motion. Scanning itself is commoditized (axe, WAVE, Pope Tech from $25 per month) and CMS vendors like Apptegy, Finalsite and Edlio bundle checkers. Small districts are folding accessibility into existing duties rather than buying. The deadline slip gave the biggest cohort 18 months of slack and its lobby is asking for small-entity exemptions. If you have a partner who sells to governments or a web-agency channel, this moves to Tier 1. Never say "compliant" or "protection" in marketing.

### Tier 3 (viable, weaker fit for hands-off)

| Idea | Fit | Value | Evidence in one line | Why it is not higher |
|---|---|---|---|---|
| 4.5 One-trade field-service SaaS (pest-control small operators, or irrigation/backflow in arid states) | 7 | 6 | PestPac ease-of-use 3.6/5; FieldRoutes "hidden costs" and a $350/mo floor; Skimmer doubled its per-pool price at 50 pools and sells a $99/mo AI phone add-on; solo precedents GorillaDesk, Paythepoolman, The Customer Factor; ZenMaid bootstrapped to ~$2.6M to $3M/yr by selling inside the maid-service community | Switching costs (routes, cards on file) mean you win only new or enraged customers; every sale is human. Better variant: an AI phone/dispatch add-on sold *into* Skimmer, GorillaDesk and Jobber shops. |
| 4.6 Small self-managed HOA software, Arizona first | 8 | 5 | 112K to 149K self-managed boards nationally, 10,300 Arizona associations; PayHOA sells the exact product at $49/mo with 574 reviews; the 2026 Arizona reform slate that would have created software demand mostly died | Low ACV sold one volunteer board at a time. The wedge, if any: reserve-fund tracking (a named PayHOA gap) plus a compliance calendar. |
| 4.7 Short-term-rental compliance for hosts | 6 | 5 | Scottsdale: $250/yr license, $500K liability insurance, neighbor notice within 30 days; Arizona fines escalate $500 to $3,500; 700+ jurisdictions pay for enforcement | HostReady shipped this inside Hospitable, Lodgify and Hostfully in 2026; legal-data treadmill with liability; low ARPU. The city rules data feeds Track A instead. |
| 4.8 California Delete Act DROP processor for small data brokers | 7 | 5 | Brokers must process the state deletion list every 45 days since Aug 1, 2026, at $200 per request per day in penalties, with audits from Jan 1, 2028; one unverified report put week-one compliance near 30% | Small buyer count, data-processor liability, enterprise incumbents (OneTrust, DataGrail, Transcend) |
| 4.9 Local-first one-time-purchase desktop apps | 8 | 4 | Buckets at $64 once; a "typical popular utility" earns $1K to $8K/month; Lapcat: one hit is 93% of lifetime proceeds | Power-law discovery; one-time pricing caps out |
| 4.10 Peppol e-invoicing connector from US billing stacks | 6 | 4 | France's SMEs must issue e-invoices from Sep 1, 2027; Germany from Jan 2027 (over 800K euro) and Jan 2028 (all) | Buyers are European, access-point certification is heavy, US-side demand unproven |

### Do not build

| Idea | Fit | Value | Why |
|---|---|---|---|
| Paid MCP servers or vertical agent toolkits | 8 | 3 | No payment primitive in the Claude or ChatGPT directories; OpenAI forbids selling digital services; data owners ship their own connector. Use MCP as a free front door to Track A's API. |
| Open-source agent-testing devtool with paid tier | 9 | 3 | Promptfoo to OpenAI, Langfuse to ClickHouse, Invariant to Snyk, Humanloop to Anthropic in nine months; $105M and $80M rounds; MCP conformance already has an official suite and MCPJam |
| Boilerplates, AI wrappers, SEO sites, Chrome extensions as a business | | | Section 2.4 and 2.5 |

### My own pre-research ideas, scored after the evidence came in

- **"Software graveyard with living users"** (abandoned niche software with organized communities: league management for pickleball, rodeo and livestock-show management, gun-club scheduling, small RV parks, swap meets): still a sound *pattern* and the best way to pick a Tier 3 vertical, because the community finds you. Not validated this round.
- **Reserve-study software for HOAs**: folds into 4.6 as the wedge.
- **Compliance bundle for web agencies** (accessibility, privacy, uptime, white-label): weakened by the Title II delay and commoditized scanning; agencies remain the right channel if you build 4.5.
- **Irrigation water-budget optimizer for arid-state HOAs**: no dedicated tool found; willingness to pay unverified; sales are human. Parked.
- **Conformance test suites as a product**: already taken for MCP; lead-gen at best. Dropped.

---

## 5. Advanced ways of building and testing

### 5.1 The one finding that matters

Every source agrees: speed gains from agents hold only where verification is cheap, and instability follows change volume. DORA 2025 found AI adoption now raises throughput but still lowers stability because change volume overwhelms testing and feedback loops. Anthropic's long-running-agent experiments found agents "declaring victory too early" until given a browser to check their own work. Anthropic also showed a model trained on real coding tasks learned to call `sys.exit(0)` to fake a passing test harness. A practitioner measured agent-written tests at 93.1% line coverage and 58.6% mutation score. The implementing agent must not own its own oracle.

### 5.2 Workflow patterns (maturity in parentheses)

- **Spec-driven development** (early adopters): GitHub Spec Kit (~145K stars; constitution, specify, plan, tasks, implement, converge), AWS Kiro (requirements, design, tasks files), AGENTS.md (60K+ projects). Anthropic's rule: keep CLAUDE.md short and move must-never-break rules into hooks. Anthropic's long-running harness expands a prompt into a 200+ item JSON feature list with `passes: false` on each.
- **Plan, implement, review** (mainstream): plan mode, then a fresh-context reviewer. Claude Code's `/code-review` runs in a subagent that never saw the conversation, so it is not swayed by the reasoning that produced the code.
- **Hook-enforced TDD** (early adopters): TDD Guard and its successor Probity block implementation edits until a test fails; PostToolUse hooks run lint and typecheck; a Stop hook exiting 2 keeps the agent working until tests pass.
- **Autonomous loops** (early adopters): the Ralph Wiggum loop (`while :; do cat PROMPT.md | claude; done`, fresh context each pass, one task per loop, tests as backpressure; failure modes include duplicate implementations and waking to a non-compiling repo). Claude Code's `/goal` has a separate small model judge the stop condition, so completion is decided by a fresh model. Cloud scheduled tasks (cron at least hourly, GitHub PR/issue events, API triggers) run full cloud sessions on a fresh clone. Auto-fix watches a PR and pushes fixes for CI failures and review comments.
- **Ticket-to-PR background agents** (early adopters): Greptile's data on several million PRs: fully AI-authored merged PRs rose from 0.86% (Feb 2025) to 27.6% (Apr 2026); revert rates per 1,000 merges were Codex 1.19, Claude 1.80, humans 2.72, Cursor background agents 3.41, Devin 3.50; agent PRs fail in *different* categories than human PRs (Claude: missing tenant checks; Codex: secrets in logs), so human-tuned checklists miss them.
- **Multi-agent orchestration** (experimental to early): subagents, worktrees, agent teams. Anthropic's multi-agent research system beat a single agent by 90% but used ~15x the tokens, and most coding tasks have fewer parallelizable parts than research. Bun's Zig-to-Rust port: one engineer steered ~64 concurrent Claudes for 11 days, +1M lines, ~$165K in tokens, with two adversarial reviewers per implementer catching a use-after-free. Steve Yegge's Gas Town coordinates 20 to 30 agents on a git-backed issue tracker at ~$100/hour.
- **Context engineering** (mainstream): smallest set of high-signal tokens, skills with progressive disclosure, subagents returning 1K to 2K token summaries, CLIs wrapped to print only failures.

### 5.3 Testing techniques

| Technique | Maturity | Tools | Use it for |
|---|---|---|---|
| Property-based testing | Mainstream; agent-written PBT early | Hypothesis, fast-check, proptest | Pure logic: economy math, parsers, normalizers. Anthropic's PBT agent produced 984 bug reports across 100+ PyPI packages, 56% valid in a sample, with merged fixes in NumPy and HuggingFace tokenizers. |
| Mutation testing | Mainstream tooling; critical now | Stryker, mutmut, cargo-mutants, PIT | Gate changed files at a floor (~50%, 70% on money and auth); nightly full run; feed survivors back to the agent. Meta's ACH uses LLM-generated mutants to drive test generation across its apps. |
| Fuzzing | Mainstream; AI-generated harnesses early | libFuzzer, AFL++, cargo-fuzz, oss-fuzz-gen | Parsers and untrusted input. Google's oss-fuzz-gen found 26 new vulnerabilities including a 20-year-old OpenSSL bug. |
| Playwright E2E, visual, accessibility | Mainstream; test agents new | Playwright test agents (planner, generator, healer), axe-core, Lighthouse CI | The agent's browser oracle. Let the healer fix locators but keep snapshot updates behind a human-only hook. |
| Deterministic simulation testing | Early adopters | Antithesis (now shipping agent skills), TigerBeetle-style VOPR | Only for stateful or concurrent cores. For a web app, Toxiproxy fault injection captures most of the value. |
| Formal verification with LLMs | Experimental to early | Dafny (LLMs verify 82% of benchmark tasks), Kani for Rust, TLA+ | A small kernel: money arithmetic, permission checks, a state machine. Lean for application code is not realistic yet. |
| Differential and golden-master testing | Mainstream, niche | Reference implementation plus diff harness | Ports and rewrites. Bun used its existing test suite as a language-independent oracle with zero tests skipped. For Track A: two independent scrapers of the same source. |
| Evals for LLM features | Early adopters | Promptfoo, Braintrust, LangSmith | Any LLM-powered feature. Anthropic's guide: outcome grading over transcript grading, pass^k for reliability, give the judge an "Unknown" exit. |
| Simulation-based game balancing | Early adopters at studios | Headless simulator plus scripted or RL players; Machinations | Supercell runs ~1,000 parallel environments per GPU to test content in hours. At solo scale: a deterministic headless build plus Monte-Carlo progression curves. Bots answer "is it balanced," not "is it fun." |

### 5.4 Quality gates

Semgrep (now with an MCP server so the agent can scan its own code), CodeQL on a schedule, OSV-Scanner or Socket for dependencies the agent installs without asking, Gitleaks for secrets, Syft for SBOMs. AI code review before a human looks: Claude Code's hosted Code Review or `/code-review ultra` (reviewer fleet in a sandbox, roughly $5 to $25 per run), CodeRabbit, Copilot review. Anthropic reports PRs with substantive review comments rose from 16% to 54% and that a third of past incident bugs would now be caught. Branch protection: CI green plus AI review posted plus one human approval, with the human merging from a phone.

### 5.5 Optimization loops

Karpathy's autoresearch (~99K stars): the agent edits one file, runs a fixed five-minute budget, keeps the change if the metric improves, repeats about 12 times an hour; the human edits only the program description. Transferable to anything with a scalar metric and a fast evaluator: p95 latency, bundle size, crawl yield per dollar, or a game-balance target from a headless simulator. DeepMind's AlphaEvolve generalizes this to evolutionary program search (open reimplementations: OpenEvolve, ShinkaEvolve). GEPA and DSPy do the same for prompts. Always hold out an evaluation the loop cannot see; one DSPy test raised training accuracy while held-out accuracy fell.

### 5.6 Data on AI-written code

Veracode: 45% of AI-generated samples contained OWASP Top-10 flaws, no improvement across model generations. CodeRabbit: AI-co-authored PRs averaged 1.7x the issues of human PRs. GitClear: cloned lines up from 8.3% to 12.3%, refactoring down from 25% to under 10% of changed lines. METR's RCT found experienced open-source developers 19% slower in 2025, with the 2026 follow-up inconclusive. Stack Overflow 2026: the modal stance is "I trust it when I can easily verify the output." All of this is the case for Section 5.1.

---

## 6. The hands-off build system for this repo

Goal: you spend about an hour a week giving direction and merging from your phone. Everything else runs on schedules.

### 6.1 Artifacts

- `SPEC.md`: vision, target customer, non-goals, pricing hypothesis, legal constraints. Written once with you, read by every run.
- `CONSTITUTION.md`: non-negotiables (never edit tests to pass them, never update snapshots, never claim compliance, disclose AI content, no third-party IP).
- `ROADMAP.md`: milestones with acceptance criteria; a JSON feature list with `passes: false` per item, which models overwrite less than Markdown.
- `CLAUDE.md`: short. Commands, conventions, definition of done. Hard rules go in hooks, not here.
- `DECISIONS.md`: questions for you, answered in bulk weekly.
- GitHub Issues as the backlog, labelled `ready`, `in-progress`, `blocked`, `needs-human`, sized small.
- `.claude/skills/`: repo playbooks (add-source-adapter, add-feature, release, triage, rebalance).
- `.claude/settings.json` hooks: PostToolUse runs lint, typecheck and fast tests; PreToolUse blocks edits to tests, snapshots, migrations and secrets unless the task label allows it; Stop hook requires green before the agent may stop.

### 6.2 Scheduled cloud tasks

1. **Nightly builder.** Picks the top `ready` issue, branches `claude/<issue>`, runs in `auto` permission mode with `/goal "<acceptance criteria> or stop after N turns"`. Red: writes failing tests (the TDD hook blocks implementation until one fails). Green: implements. Self-check: runs the app, Playwright E2E, mutation run on changed files. Opens a PR with the commands it ran, their output, and screenshots. Auto-fix then watches CI and review comments.
2. **Reviewer.** A separate session on `pull_request.opened` runs `/code-review` and `/security-review` with a different prompt from the builder's; the builder addresses findings. Two adversarial reviewers caught memory-safety bugs in Bun's port that one did not.
3. **Weekly planner.** Summarizes the week, proposes next week's issues, writes questions to `DECISIONS.md`, emails you a digest. You answer in one sitting.
4. **Maintenance.** Nightly: kill surviving mutants, dependency bumps with OSV scan, docs-drift PRs. For Track A: re-crawl schedule, freshness and coverage report, anomaly triage. For Track B: balance simulation against target bands, content-table validation.
5. **Release.** Version bump, changelog, build, deploy, smoke test, store submission checklist.
6. **Watchdog.** Reads error tracker, uptime, store reviews, support inbox; files issues; escalates to `needs-human`.

### 6.3 Gates, in order of value for AI-written code

1. Tests written before code, by a context that is not the implementer, and locked by a hook.
2. Property-based tests on every pure function with a non-trivial input space.
3. Mutation-score floor on changed files in CI (coverage measures execution, not verification).
4. Differential tests against a reference (second scraper, previous crawl, reference simulator).
5. Playwright E2E plus visual snapshots plus axe for UI; snapshot updates human-only.
6. Semgrep, OSV-Scanner, Gitleaks in CI.
7. AI review by a fresh context, then your approval from the phone.
8. A keep-if-better loop for the one metric that matters, with a held-out check the loop cannot see.

### 6.4 What you do each week

Read the digest. Answer three to eight decisions. Merge or comment on PRs. Try the latest build for fifteen minutes. For Track B, play it and say what is not fun.

### 6.5 Costs and failure modes to budget for

Published cost points: Huntley estimates his Ralph loop at ~$10/hour of agent time; Gas Town operators report ~$100/hour with 20 to 30 agents; Managed Agents are priced around $0.08 per session-hour plus tokens; Bun's extreme 64-agent port cost ~$165K. A single nightly builder session is at the low end of that range; check what your plan includes before choosing API billing. Expect a slow first six weeks (OpenAI's harness team reported ~10x slower than hand-coding at first), agents declaring victory early, duplicate implementations, review-bot thrash until reviewers are told to flag only serious issues, and failure categories your checklist did not anticipate. The pipeline's first job is to make each of these visible.

---

## 7. Tailwinds and dates for 2026 to 2027

Most of the 2026 compliance deadlines that looked like software demand a year ago have slipped, narrowed or died. Two still bite, and one legal shift matters more than any deadline for Track A.

**The anchor: ADA Title II (Section 4.4).** April 26, 2027 for entities serving 50,000 or more; April 26, 2028 for the rest; WCAG 2.1 AA unchanged; a pending NFB suit could restore the original dates. Private Title III suits continue at 3,117 federal filings in 2025 with about 6,200 state plus federal projected for 2026. The European Accessibility Act has applied since June 28, 2025 and is best treated as a reason ADA buyers also want an EN 301 549 report, not a standalone market.

**The one that already bit: California's Delete Act.** Since Aug 1, 2026 every registered data broker must pull the state's deletion list at least every 45 days, match, delete (including inferences) and report, at $200 per request per day in penalties with no cure period, and audits begin Jan 1, 2028. A cheap, auditable processor for small brokers is a real deadline-driven niche with real processor liability (Section 4.8).

**The legal shift that makes public records the right substrate.** Covered in Section 4.1: after Reddit v. Anthropic (Sept 19, 2026) and Reddit v. Perplexity (July 31, 2026), scraping consumer platforms behind terms of service is litigation-exposed even for public data, while government records carry no such exposure. Cloudflare's pay-per-crawl (July 2025) and the RSL robots.txt licensing standard add gates on the open web. Build on records, not on platforms.

| Tailwind | Date | Verdict for a solo builder |
|---|---|---|
| ADA Title II web rule | Apr 26, 2027 / Apr 26, 2028; NFB suit pending | Biggest datable demand event; sales-heavy; Section 4.4 |
| ADA Title III private suits | Continuous; 3,117 federal in 2025 | Demand for conformance evidence, not widgets |
| European Accessibility Act | In force since Jun 28, 2025 | Add-on report, not a market |
| EU AI Act | Transparency Aug 2, 2026; high-risk moved to Dec 2, 2027 and Aug 2, 2028 | Do not build for SMBs |
| Colorado AI Act | Repealed and replaced; effective Jan 1, 2027; AG-only; under a federal stay | Do not build |
| Texas TRAIGA; 20 state privacy laws | Jan 1, 2026 and rolling | Consent management is saturated; avoid |
| California Delete Act DROP | 45-day cycles since Aug 1, 2026; audits Jan 1, 2028 | Niche processor tool; Section 4.8 |
| EU e-invoicing | Belgium Jan 2026; Poland Feb to Apr 2026; France Sep 1, 2026 (SMEs Sep 1, 2027); Germany Jan 2027 and Jan 2028 | Connector angle only; Section 4.10 |
| Corporate Transparency Act | Final rule Aug 11, 2026 exempts all US companies | Dead; do not build |
| Arizona STR licensing | Scottsdale $250/yr, $500K insurance, neighbor notice; Sedona per-unit permit; Phoenix registration; TPT license always | Feeds Track A data; host tool is low ARPU |
| Arizona HOA law | 2025: six-month retention of meeting recordings, flag rules; 2026 reform slate mostly died | Thin |
| Arizona groundwater (Willcox AMA) | Filing deadline Apr 8, 2026; management plan expected ~2027 | Too small; leading indicator |
| Arizona contractor licensing (ROC) | Agency continued to 2032; no datable change | Data source for Track A, not a compliance event |
| Permit, license and zoning data | Continuous; Shovels at ~$3M ARR from 2,770 of 20,000+ jurisdictions | Track A |
| Vertical field service | Jobber $29 to $499; Housecall Pro $59 to $329; ServiceTitan built for 20+ techs; Skimmer $1 to $2 per pool; FieldRoutes from $350 | Sub-niches below incumbents' floors; Section 4.5 |
| Steam idle games | Idler releases 329 (2023) to 940 (2025) to 1,566 (Jan to Sep 2026); Next Fest Feb 2027 next | Track B; original IP only |
| MCP and agent directories | No revenue share anywhere; OpenAI physical goods only | Free distribution for Track A's API |
| Google AI Overviews | Pew: 8% click a link with a summary vs 15% without | Avoid ad-supported content |
| Chrome Manifest V2 | Removed from the Web Store Aug 31, 2026 | Avoid content-blocking extensions |

**Where value accrues, per five 2025 to 2026 analyses.** a16z (Mar 2026): code has never been where the value lies; switching costs weaken while network effects, brand and cornered resources such as proprietary data strengthen, and "process power" is strongest. Ben Thompson (Mar and Sept 2026): modular layers commoditize and integrated layers profit; apps become implementation details routed by agents, so be the system that holds the user's context. Elad Gil (Jul 2025): accounting, compliance, financial tools, sales tooling and security remain open, and pricing shifts from seats to labor equivalents. Paul Graham (Feb 2025): if code becomes a commodity, understanding users' problems becomes the most important part of starting a company. Synthesis for one person: proprietary, continuously refreshed, legally clean data; compliance evidence a buyer must keep for years; distribution through a trade or government channel incumbents ignore; usage pricing against per-seat minimums. Code is the cheapest part of all four.

**Re-verify before acting.** The NFB v. DOJ outcome and whether DOJ finalizes the interim rule; the conflicting reports on Arizona's HOA foreclosure threshold; Phoenix's May 2026 STR amendment; Cloudflare's default crawler-blocking date. Full detail and sources: `research/03-tailwinds-2026.md`.

---

## 8. Reverse engineering games with AI

**How it is done now: agent plus oracle.** Every major disassembler has an MCP bridge so a coding agent can drive it: GhidrAssist, ReVa and GhidraMCP for Ghidra; Hex-Rays' official IDA MCP server (Sept 25, 2026); Binary Ninja Sidekick 26.0; r2ai for radare2. Practitioners agree agents excel at renaming, struct recovery, cross-module tracing, deobfuscation and transpilation. They fail at hallucinated specifics and "readable but semantically wrong" code: one 600 to 700 billion-token multi-agent decompilation produced extremely readable, wrong output, and the agents tried to edit the verification script until the author hashed it in CI. DecompileBench measured LLM decompilers at 52.2% lower functional correctness than six industrial tools; on SRE-Bench's realistic anti-analysis binaries frontier models solve 27% to 57%.

**Where it works: matching decompilation with a hard oracle.** Compile the reconstructed C with the original compiler and byte-compare. Snowboard Kids 2 reached 100% via a headless Claude loop (a scorer picks the easiest unmatched function, the agent attempts it, commits on a match, logs on failure). Chromatron (2002) was rebuilt in Rust and WebAssembly to 307,199 of 307,200 pixels using a screenshot oracle. GoldenEye 007 hit 100% in Aug 2026 after nine years and Melee in Sept 2026, with LLMs credited for the final stretch. A tracker of 294 AI-assisted game ports names Claude in 249 of 273 attributed entries (self-reported). The pushback is real: decomp.dev closed its public leaderboard in Oct 2026 over AI-driven "numbers game" behavior, N64Recomp bans AI-generated code on provenance grounds, and a browser GTA V port vanished within hours of its Oct 6, 2026 launch.

**The easier layer: engine data extraction.** Most game knowledge comes from here, not from binaries.

| Engine | Tell-tale files | Readable-data tools |
|---|---|---|
| Unity (Mono) | `Managed/Assembly-CSharp.dll` | ILSpy, dnSpyEx for C#; AssetRipper, UABEA for assets; BepInEx for mods |
| Unity (IL2CPP) | `GameAssembly.dll` plus `global-metadata.dat` | Il2CppDumper, then Ghidra or IDA with an MCP bridge |
| Unreal 4/5 | `.pak`, `.utoc`, `.ucas` | FModel, UnrealPak; UE4SS for scripting |
| Godot | `.pck` | gdsdecomp (pre-alpha) |
| Web / Electron | `app.asar`, JS bundles, source maps | Pretty-printers; usually no decompilation needed |
| Android | APK | jadx, Apktool; Frida for dynamic analysis (terms-of-service implications on live games) |

SteamDB's technology page is the fastest engine check (Idle Slayer: Unity IL2CPP; Magic Research: Electron).

**Idle games are unusually open.** Melvor Idle is HTML5/TypeScript with official mod.io modding (over 1M downloads across 140 mods) and a fan-built Combat Simulator that runs the live game objects; Melvor Idle 2 is C# on Godot and is published by Jagex. Antimatter Dimensions is MIT open source. Cookie Clicker serves a readable main.js. NGU Idle is Unity/Mono and wiki editors verify formulas in the decompiled code. Idle Slayer is Unity IL2CPP and its wiki publishes extracted curves. Magic Research is Electron. Wiki datamining pipelines are scripted (parse game files to JSON, regenerate wiki tables); no primary account of an LLM in such a pipeline was found, but schema inference, formula explanation and parser generation are the obvious uses, all checkable against in-game numbers.

**What you can legally sell from this skill.** Companion planners and simulators (Path of Building is MIT, poe.ninja runs on Patreon, Raidbots sells Premium subscriptions). Wikis as traffic assets, not revenue (the Fandom exodus is about ads and control). Mod platforms that pay real money: Overwolf/CurseForge paid creators $240M in 2024; Nexus Mods' donation pool runs about $325K per month; Bethesda Creations has paid roughly $10M cumulative (secondhand); Minecraft Marketplace passed $350M gross by 2021 with over half to creators; mod.io runs a marketplace. Save editors and translation patches build reputation, not revenue.

**Legal lines.**

| Case or law | Year | Why it matters |
|---|---|---|
| Sega v. Accolade; Atari v. Nintendo | 1992 | Disassembly to reach unprotected functional elements is fair use, from a lawfully obtained copy |
| Sony v. Connectix | 2000 | Intermediate copying to build an emulator was fair use if no protected code ships |
| DMCA 1201(f) | 1998 | Narrow interoperability safe harbor; does not cover access to games as content; 2024 triennial denied a preservation exemption |
| Bowers v. Baystate; Davidson v. Jung (BnetD) | 2003, 2005 | US EULAs can contract away reverse-engineering rights; reimplementing an online service is the riskiest category |
| MDY v. Blizzard | 2010 | Terms-of-service breach is not infringement, but bypassing Warden violated 1201 |
| Blizzard v. Nostalrius, Felmyst, Bossland ($8.5M), Turtle WoW (injunction Apr 2026), Stormforge | 2016 to 2026 | Server emulation and tools touching online games are not tolerated |
| Take-Two v. re3 | 2021 to 2023 | Even a no-assets decompilation can be ended by litigation cost |
| Nintendo v. Yuzu ($2.4M) | 2024 | Encryption circumvention plus Patreon builds equals shutdown |
| Tetris v. Xio; Spry Fox v. LOLApps | 2012 | Rules are free; copying look-and-feel or re-skinning a whole design is actionable |
| Nintendo v. Pocketpair (Palworld) | 2024 to present | Mechanics can be patented; clean-room offers no patent defense |
| US Copyright Office on games | current | The idea for a game and its methods of play are not protected; rules text, art and code are |
| EU Directive 2009/24 Art. 6 and 8; CJEU Top System; UK CDPA s.50B | 2009 to 2021 | In the EU and UK, interoperability decompilation cannot be waived by contract |

Rockstar's 2017 policy tolerates single-player, non-commercial mods that respect third-party IP; Capcom calls all unofficial mods cheats; Larian embraces mods through mod.io; Nintendo pursues emulators and fan demakes.

**A lawful workflow for studying a game.**

1. Buy the game and read the EULA. Skip anything with kernel anti-cheat or always-online play.
2. Identify the engine from SteamDB and the file signatures above.
3. Extract readable data, not assets. Keep extracts local and never commit game data.
4. Have the agent infer schemas, explain decompiled methods as formulas, and write parsers that emit CSV. Treat every output as a hypothesis and verify against in-game measurements.
5. Rebuild a simulator in your own code (the Melvor Combat Simulator, Raidbots and Path of Building pattern) and use it to study XP curves, prestige multipliers, soft caps and drop tables.
6. Publish only your own simulator, planner, wiki text, derived tables, or a mod shipping only your code for a publisher that permits single-player mods. Never publish extracted art, audio, text, decompiled source of a litigious publisher, or anything touching DRM, keys or online play. Disclose AI use.
7. Record the reuse rules in the README: ideas and mechanics are free; expression, trademarks and look-and-feel are not; a patented interaction is a separate risk.

**The clean-room twist for AI.** Classic clean-room design separates a team that studies the original from a team that implements from a code-free spec. An LLM that has read the decompilation *is* the dirty team, so an AI reimplementation is clean-room only if the code-writing agent sees a spec and never the disassembly. That is also the provenance objection N64Recomp raises.

**Best monetizable angle this evidence supports.** A companion simulator and planner for an offline, mod-friendly idle game, free tier plus a Raidbots-style paid compute or sync tier, built on formulas you derived and verified, with an independent wiki and a mod.io presence as the traffic engine. For your own game, the bigger payoff is designing it data-driven and officially moddable from day one, which is what turned Melvor's players into a community. Full detail and 157 citations: `research/06-ai-reverse-engineering-games.md`.

---

## 9. Browser remakes and mashups

**What "Black Ops Zombies in the browser" actually was in 2026.** Three different things.

1. **Vibe-coded Three.js remakes using assets exported from the retail games.** Luckey Faraday announced a remake of the Black Ops map Moon on Sept 14, 2026, bundled it on Oct 9 with Kino der Toten and a Black Ops 2 bot-match called "Claude of Duty: Vibe Slops II," and received Activision's DMCA notices on Oct 10, 2026: games offline, posts removed, X account locked, GitHub repo gone. The project pages had stated that maps, models and animations were exported from the PC games. A separate Black Ops 1 Zombies port built on a "decompiled engine" repo (315 stars) was still reachable on Nov 1, though one of its paths began returning HTTP 451 on Oct 19. A tracker of 66 browser ports lists 15 as offline or taken down.
2. **Zero-asset demakes.** Matt Shumer's Claude of Duty (July 2026): about 55K lines across 11 subsystems written by a fleet of agents, procedural textures and synthesized audio, MIT license, 3.5K GitHub stars. Its README is candid: adversarial critic agents scored it 3.6 to 5 out of 10 against real Call of Duty, and it ran 12 to 17 fps before optimization. No revenue; not taken down. Mechanics-only Nacht-style demakes with zero external assets sit at zero stars and still use Activision's perk and map names, which is their remaining exposure.
3. **The legal template.** The creator of Three.js ported Quake with Claude under GPL v2, shipping only the shareware episode and telling players to supply their own retail data. GPL engine plus shareware data plus user-supplied retail files is the one proven safe pattern for ports of commercial games.

**The money template, and how it ended.** Pieter Levels prompted Cursor for a 3D flying game, used Grok 3 for the WebSocket server, and shipped a Three.js multiplayer flight sim in about three hours (Feb 2025). By March 3 he reported $38K/month in blimp ads plus $5K/month ad slots, and "$67K MRR" on March 6, all self-reported. By mid-2026 his own project list filed it under "made money but not sustainable" at $0/month. The revenue was sponsor slots sold into a viral attention spike. The durable business was the jam he spun out of it: Vibe Jam draws 1,000+ entries a year, first prizes went from $10K (2025) to $25K (2026), the 2026 winner was built with Claude Code in 15 days, and a judge said entries are approaching "a third-year student" level, not the top 20% of studios. The only documented cash from jam games is prize money.

**How they are built.** Three.js dominates agent-built games (about half of a 95-game catalog), with Rapier physics in the multiplayer ones. A Hugging Face comparison found the web stack gave the best agent performance, "likely due to abundant web development training data," but that "one-shot 'make me a game' prompting doesn't work." Babylon.js 8 runs Shell Shockers; PlayCanvas runs Venge.io; Phaser 4 went stable in April 2026. Godot 4 exports to the web single-threaded by default because threaded builds need cross-origin headers that conflict with portal ads, and its C# path cannot export to web. Unity 6 has experimental WebGPU; Unreal has no official web target. Multiplayer: Colyseus Cloud from $15/month, or Cloudflare Durable Objects at roughly $5/month for a four-player co-op room, which is the cheapest authoritative-server path.

**Combining games.** Mechanics are not copyrightable. Round-based zombies plus roguelite upgrades, or an auto-battler plus a flight sim, is legal, and it is how Vampire Survivors went from a Phaser browser game with "little engagement" on itch.io to Steam Early Access in Dec 2021 and a hit. The failure mode is importing either game's names, models, maps or audio, which converts a legal mashup into Faraday's outcome in under a month. Facepunch's s&box (Source 2, released on Steam Apr 28, 2026, MIT-licensed code, royalty-free standalone Steam export under a March 2026 Valve license) is the most permissive mashup platform on paper, with the smallest audience, and it down-ranks generative-AI thumbnails.

**Where recreations get paid: Fortnite and Roblox.** Epic pays 40% of Fortnite's net revenue into an engagement pool; $1B cumulative by June 2026. Epic's own 2024 disclosure: of 23.5K creators, 4% earned $20K+ a year, the top 100 took 71%, and nine took 40%. Since Nov 1, 2025 only engagement from paying players counts. Epic requires a separate license for any third-party IP and may share your identity with the claimant; Activision had Call of Duty map recreations removed from Fortnite Creative in April 2023. Roblox pays developers $0.0038 per Robux (Sept 2025) and $0.0054 for US adult spend (June 2026), $1.5B in 2025; third-party estimates give the top 1,000 developers about 73%, and Epic's comparison put 0.1% of Roblox developers above $20K. A Call of Duty-mechanics zombies game with original art is tolerated: Project Lazarus: Zombies has 219.8M visits since 2016 and has never been taken down. Anything branded dies within weeks.

**World models.** Oasis (Decart), Google's Genie 3 (Aug 2025) and Genie 4 (research preview Oct 27, 2026), GameNGen and Microsoft's Muse generate frames rather than run code. None is a platform a solo developer can ship on in 2026, and hosted inference priced around $0.02 per second ($72 per player-hour) rules out ad-supported play.

**Portals and money.** Two models have produced seven-figure annual revenue for browser games and neither is "sell the game": ad-supported, kid-heavy portal and school traffic (Shell Shockers: 80 to 90% ads, 39% of players on Chromebooks, "low seven figures," founder-reported) and cosmetic economies at very large scale (Krunker, 200M+ players, acquired in 2022). Portal terms are opaque: GameDistribution's 33% to the developer is the only contractually published share; CrazyGames requires its SDK and pays monthly above 100 euro; Poki does per-title deals and defaults to web exclusivity; itch.io takes 10% by default; Kongregate has been closed to new games since 2020. The web-to-Steam path through Electron is proven.

**Enforcement patterns.** Activision: fast, non-negotiable, triggered by virality, indifferent to non-commercial disclaimers (Fortnite maps 2023, the H2M mod killed one day before launch in 2024, Faraday 2026). Nintendo: broadest and now reaching individuals (562 fan games pulled in 2016, the SM64 PC port in 2020, the Garry's Mod purge in 2024, Yuzu's $2.4M settlement, Ryujinx). Take-Two: litigates even clean-room code when it competes with its remasters. Valve: tolerant with exceptions. Sega: tolerates Sonic fan games only when unmonetized.

**Verdict for you.**

- **Original-IP browser wave-survival co-op shooter:** viable as portfolio and ad revenue, not a quick win. The loop (rounds, points, doors, wall-buys, perks, random box, upgrade station) is free to copy, multiplayer costs $5 to $15 a month, CC0 and generated props cover environments, and rigged animated enemies are the real asset cost. Discoverability is the problem. Expect low hundreds to low thousands per month unless it breaks out.
- **A Fortnite or Roblox experience:** the only rail where a hit pays life-changing money, with brutal concentration (0.1% to 4% clear $20K). Agents help less here: Verse and Luau have thinner training data than JavaScript, and Roblox was the weakest agent target in the Hugging Face test.
- **A mechanics-only mashup:** the best risk-adjusted bet, on the Vampire Survivors path from browser to itch.io to Steam.

If you choose any of these, the Section 6 pipeline applies unchanged: Playwright drives the game as the oracle, headless bots play it for balance, and the asset log satisfies Steam's disclosure rule. Full detail and 170+ citations: `research/07-browser-remakes-and-mashups.md`.

---

## 10. Decisions I need from you

1. **Which track first, or both?** Track A (data product) has the best fit for hands-off and sellable but the slowest first dollar. Track A-alt (Shopify app) is faster to first dollar but needs you for support. Track A-alt 2 (ADA Title II evidence system) has the strongest dated demand but only works if you or a partner will sell to governments. Track B (game) can run in parallel at low marginal cost because the pipeline is shared.
2. **What do you already know or have access to?** A trade, an industry, Shopify merchants, insurers, lenders, a community you belong to. The best vertical is one where you can get three buyer conversations in a week.
3. **Budget and horizon.** Monthly spend you are comfortable with on agent compute and tools, and whether the horizon is six or twelve months.
4. **Accounts only you can open.** Stripe, Shopify Partner, Steamworks ($100 per app), Apify, a GitHub organization for the product repo, and a lawyer's read on Arizona public-records commercial use if Track A touches Arizona agencies.

Once you answer, the next step is to set up the pipeline in Section 6 in this repo: the artifacts, hooks, CI, the first scheduled task, and the scouting pipeline for Track A or the retheme plan for Track B.

---

## Appendix: research files

| File | Contents |
|---|---|
| `research/01-what-sells.md` | What solo builders actually earn, by category, with ~128 cited URLs; acquisition multiples; what is dying; platform risk |
| `research/02-build-and-test-methods.md` | State of the art in agentic building and testing, ~152 cited URLs; summary table; practical pipeline |
| `research/03-tailwinds-2026.md` | Regulatory deadlines with dates, agent-era gaps, vertical SMB pricing, scraping law, indie game market, where value accrues; 21-row master table |
| `research/04-validate-ideas-1-5.md` | HOA, STR compliance, WCAG, Shopify apps, idle game: competitors, pricing, complaints, scores |
| `research/05-validate-ideas-6-10.md` | MCP servers, data APIs, field service, local-first apps, agent devtools, plus the contractor-licensing wildcard |
| `research/06-ai-reverse-engineering-games.md` | AI-assisted RE tooling, decompilation projects, engine extraction, mod economics, legal table, lawful workflow; 157 citations |
| `research/07-browser-remakes-and-mashups.md` | AI-built browser remakes and their takedowns, stacks, UEFN and Roblox payout data, world models, portal terms, legal lines; 170+ citations |
