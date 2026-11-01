# Track A design: a public-records data product sold by usage

Status: design, 2026-11-01. Evidence: `WHAT-TO-BUILD.md` Sections 4.1 and 7, `research/05-validate-ideas-6-10.md` (Idea 7 and the wildcard), `research/03-tailwinds-2026.md` Section 6.

## 1. What it is

A continuously maintained, normalized dataset of one class of government records across every US jurisdiction that publishes them, sold self-serve by usage: a REST API with a free tier, a change feed with webhooks, bulk exports and warehouse shares for enterprise, per-state scrapers listed on Apify, and a free MCP connector so agents can query it. Later, a thin SaaS on top for the people who feel the pain directly.

The model is Shovels (permits, $599 and $999 per month, about $3M ARR in three years with a team, covering 2,770 of 20,000+ jurisdictions) and Zoneomics (zoning, $134 to $549 per month, sold to lenders and proptech). The moat is coverage, normalization and update cadence, which is exactly what months of scheduled agent work produces.

## 2. Phase 0: choose the vertical by evidence (weeks 1 to 3)

The architecture below is source-agnostic, so the vertical is a configuration choice, not a rewrite. Phase 0 is an agent-run scouting pipeline that scores candidates and produces a one-page recommendation for your sign-off (open question Q-002).

**Leading hypothesis: contractor licensing and enforcement events.** License status, classifications, qualifying individuals, bonds and insurance on file, disciplinary actions, complaints and lapses, across the state licensing boards plus the large city-level licensing regimes (New York City, Chicago and others). The research found no vendor offering this as a clean 50-state API with change events; the only Apify actor covers five cities. Buyers with documented spend on adjacent data: insurers writing contractor policies, construction and fix-and-flip lenders (Kiavi buys zoning data), home-services marketplaces and lead-gen companies that verify licenses, building-materials suppliers extending trade credit, solar and telecom installers vetting subcontractors, and general contractors tracking subcontractor compliance. Several boards appear to publish bulk files (California, Florida, Oregon, Washington, Texas trade licenses, Virginia, Utah) and New York City publishes license data on its open-data portal; Phase 0 verifies each.

**Alternatives scored in the same pass:** short-term-rental registrations by city; permits in the 17,000+ jurisdictions Shovels does not cover (head-on competition); code-enforcement cases; city business licenses (Middesk territory); liquor licenses; food-service inspections.

**Scorecard (weights):** buyer concentration and documented willingness to pay (25); source accessibility, bulk files or APIs versus HTML versus PDF, anti-bot posture (15); jurisdiction count and normalization difficulty, which is both moat and cost (15); legal posture: facts and edicts versus compiled databases, terms on portals, state copyright claims, personal-data sensitivity (15); change velocity, since events are what people pay to be alerted about (10); incumbent gap in coverage, API quality and price (10); adjacency to future record sets that share entities (10).

**Kill criteria for the hypothesis:** fewer than three buyer categories with documented spend; more than half the states lacking machine-readable or scrapable lookups; a vendor already selling a 50-state API with events below $500 per month; counsel flags the FCRA or state-copyright exposure as unmanageable.

**Deliverables:** `research/phase0-scorecard.md`, a legal register per source (terms, robots.txt, copyright posture, state law, personal-data fields), and a draft `SPEC.md`.

## 3. Product definition

### 3.1 Canonical data model

| Entity | Key fields |
|---|---|
| `Jurisdiction` | id, name, level (state, county, city), FIPS or geoid, timezone |
| `Source` | id, jurisdiction, url, kind (bulk file, API, HTML search, PDF), schedule, legal register entry, adapter version |
| `Entity` | id, legal name, DBA names, entity type, addresses (business only), phone (as published), external ids per source |
| `License` | id, entity, jurisdiction, license number, type and classification (normalized vocabulary plus source-native), status (normalized vocabulary), issued, expires, last renewed |
| `Qualifier` | license, person name as published, role |
| `Event` | id, license or entity, kind (issued, renewed, expired, suspended, revoked, reinstated, disciplinary action, bond cancelled, insurance lapsed, complaint filed), effective date, observed date, details, source document |
| `Document` | id, source, fetched at, content hash, storage key, parser version |
| `Provenance` | every row carries source id, document id, fetched at, parser version |

Status and classification vocabularies are the hard normalization work and the moat: each source's native values map to a shared vocabulary with the native value preserved.

### 3.2 Products on top of the model

1. **REST API.** Lookup by license number, entity name, address, qualifier; search with filters; history per license; jurisdiction coverage and freshness endpoints. Versioned, OpenAPI-documented, with generated TypeScript and Python SDKs.
2. **Change feed.** Webhooks and a polling endpoint for events, subscribable by jurisdiction, entity list or license list. This is the product insurers and marketplaces pay for.
3. **Bulk.** Daily Parquet and CSV snapshots in object storage; Snowflake and BigQuery shares for enterprise.
4. **Apify actors.** One actor per state publishing the raw normalized records at per-result pricing. Zero sales effort; Apify pays about 80% to the developer.
5. **Free pages.** One page per jurisdiction with coverage, freshness and how to verify a license there, and one factual page per licensed business. These are the organic-traffic engine Zoneomics and BuildZoom use. Each page carries a correction and removal request link.
6. **MCP connector.** Free, wraps the API with the user's key, listed in the Anthropic directory for distribution.

### 3.3 Pricing hypothesis

Free: 500 credits per month, attribution required. Basic: $299 per month, 25K credits. Pro: $799 per month, 100K credits, webhooks. Enterprise: warehouse share and SLA, quoted. Reference points: Shovels $599 and $999; Zoneomics $134 to $549; Middesk's median buyer pays $14,375 per year. A credit is one lookup or one event delivered. Pricing is a Phase 0 output and a decision for you.

## 4. Architecture

**Language and framework.** Python 3.12 throughout, strictly typed (pyright strict, ruff), pydantic v2 models for every schema. Python has the strongest scraping and parsing ecosystem and agents write it fluently. FastAPI for the API. One language keeps the agents' context small.

**Source adapter framework.** Every source is a module implementing `discover()`, `fetch()`, `parse()` and `normalize()`, with a declarative YAML config for simple HTML sources and Python for the rest. Fetching uses httpx with per-source rate limits, a descriptive user agent with a contact address, caching, and Playwright only for JavaScript-rendered portals. PDFs go through pdfplumber with a table-extraction fallback. Each adapter ships with recorded fixtures (raw responses) and expected normalized output, so tests run offline and deterministically. High-value sources get two independently written parsers whose outputs are diffed in CI.

**Data flow.** Raw snapshot (immutable, content-hashed, stored in Cloudflare R2) to staging (parsed, source-native) to canonical tables (normalized) to event derivation (diff of consecutive snapshots per license, producing `Event` rows) to serving (Postgres for the API; Parquet in R2 for bulk; DuckDB for analytics and QA).

**Scheduling.** A Postgres-backed job queue and a worker process. Each source has a schedule (daily for event-heavy sources, weekly for slow ones), a freshness SLA, and anomaly rules: record count drops over 20%, schema drift, or parse-error spikes auto-open an issue and pause publication for that source.

**Serving.** FastAPI behind Cloudflare; API keys with per-key credit metering; Stripe metered billing; rate limits; OpenAPI spec is the source of truth for SDKs and the MCP server. Static pages are generated nightly and served from Cloudflare Pages.

**Infrastructure.** Fly.io or Render for the API and workers, Neon or Supabase for Postgres, Cloudflare R2 for objects, Cloudflare Pages for the site, Sentry for errors, an uptime monitor, Plausible for site analytics. Expected early run cost: $50 to $150 per month.

**Compliance by design.** A legal register entry per source before it is enabled. robots.txt and rate limits enforced in the fetch layer, not by convention. Data minimization: business-level records; individuals only as published qualifiers; never sensitive categories. A public correction and removal process with a 10-business-day SLA. Customer terms forbid resale and define permissible use. Two items for counsel before launch: Arizona's commercial-use rules for state public records, and FCRA scoping, since selling data about sole proprietors to insurers for eligibility decisions can make a vendor a consumer reporting agency. The product is structured as business-entity data with permissible-use terms to stay on the right side of that line; counsel confirms.

## 5. Track-specific oracles

- Schema validation on every record (pydantic), with coverage of required fields per source reported as a metric.
- Recorded fixtures per source: raw input to expected normalized output. Fixtures are human-only to update.
- Property-based tests (Hypothesis) on normalizers: names, addresses, dates, statuses, license-number formats; round-trip and idempotence properties.
- Differential parsers for the top ten sources by record count.
- Canary records: a fixed set of known licenses per source checked after every crawl.
- Freshness and coverage dashboards that are also tests: a source past its SLA fails the nightly watchdog.
- Event-derivation invariants: no event without two snapshots; no status change without a corresponding event; event counts within historical bands.
- API contract tests generated from the OpenAPI spec (Schemathesis); authorization invariant tests (one key never sees another's usage); load tests with k6 before each pricing change.
- Mutation testing (mutmut) with a 70% floor on `normalize/` and `billing/`.
- Playwright plus axe on the docs site and dashboard.

## 6. Roadmap

| Milestone | Weeks | Deliverables | Exit criterion |
|---|---|---|---|
| M0 Scout | 1 to 3 | Phase 0 scorecard, legal register, draft SPEC, your decision on the vertical | Vertical chosen |
| M1 Platform | 3 to 6 | Adapter framework, canonical schema, raw storage, three sources live (one bulk file, one HTML portal, one PDF), the full gate, CI, nightly builder running | Three sources pass fixtures, properties and canaries nightly |
| M2 Coverage and API | 6 to 12 | Fifteen states; event derivation; API v0 with keys; docs; internal dashboard; freshness SLAs | Fifteen sources within SLA for two consecutive weeks |
| M3 Monetize | 12 to 18 | Thirty-plus states; webhooks; bulk exports; Stripe billing; free tier; jurisdiction pages; MCP connector; Apify actors for five states | First paying self-serve customer |
| M4 Complete | 18 to 26 | All states plus the major city regimes; enterprise warehouse share; security and privacy documentation; outreach to ten named buyers | Ten paying customers or two enterprise pilots |
| Steady state | 26 onward | Adjacent record sets through the same framework (permits in uncovered jurisdictions, STR registrations, code enforcement), expansion by scorecard | Each new set follows M1 to M3 in compressed form |

## 7. Go-to-market: what runs itself and what needs you

Runs itself: jurisdiction and business pages, docs, free tier, Apify listings, the MCP directory listing, API marketplace listings, a monthly coverage report published as a changelog.

Needs you: outbound to roughly thirty named buyers (agent-drafted emails, your name on them), two or three buyer conversations in Phase 0 to validate price, enterprise contract signatures, and the counsel review. Expect the first paying customers to come from the outbound, not the pages; the pages compound later.

## 8. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Coverage and accuracy treadmill across hundreds of sources | Adapter framework, per-source SLAs, anomaly pausing, maintenance routine; publish coverage honestly |
| Portal terms or anti-bot measures on vendor-hosted systems | Prefer bulk files and official APIs; legal register per source; public-records requests as a fallback channel |
| Arizona commercial-use and state copyright posture | Counsel review before enabling Arizona state sources; facts and edicts first |
| FCRA scope creep | Business-entity framing, permissible-use terms, counsel confirmation |
| Incumbent adds the same feed | Speed to coverage, change events as the differentiator, enterprise relationships |
| Thin self-serve demand | Outbound in M3 and M4; Apify and warehouse channels; thin SaaS wedge in steady state |

## 9. Your involvement

Phase 0: one decision and two or three buyer conversations. Then the weekly hour from the pipeline design, plus signatures and the counsel conversation.

## 10. Decisions and accounts

Decide: the vertical after Phase 0 (Q-002), product name and domain (Q-003), pricing (M3). Open: a legal entity and Stripe account, Cloudflare, Fly or Render, Neon or Supabase, Apify developer account, Sentry. Budget: $50 to $150 per month infrastructure, rising with traffic.
