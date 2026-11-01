# Track A technical design: the records platform

Status: design, 2026-11-01. Detail for `design/01-track-a-public-records-data-product.md` (the product design) built the way `design/00-studio-pipeline.md` prescribes. Evidence: `WHAT-TO-BUILD.md` Sections 4.1, 5 and 7; `research/05-validate-ideas-6-10.md` (Idea 7 and the wildcard). Repo: `sebob2491/records-platform` (D-007).

The vertical is undecided (Q-002). Everything below is vertical-agnostic; contractor licensing and enforcement events across US state boards and the major city regimes is the worked example because it is the leading hypothesis. Where the example leaks into a name (for example `LicenseStatus`), the name is kept because every candidate vertical (STR registrations, liquor licenses, business licenses, code enforcement) is also a licensing-or-case record with a status, a holder, a jurisdiction and a history.

## 1. Scope, assumptions, non-goals

**In scope.** The data platform (adapters, storage, event derivation, quality gates), the serving layer (REST API, change feed, bulk, MCP connector), billing, the public pages, and the operations needed to run them with about an hour of owner time a week.

**Assumptions.**

1. Python 3.12, pyright strict, ruff, pydantic v2, FastAPI, httpx, Playwright only for JavaScript portals, pdfplumber, Postgres 16 on Neon (Supabase is the fallback; nothing below depends on Supabase-only features), Cloudflare R2 and Pages, Fly.io for API and workers (Render is interchangeable), Stripe metered billing, Sentry, Plausible.
2. One repository, one Python package `records/` with sub-packages `adapters/`, `normalize/`, `store/`, `events/`, `quality/`, `api/`, `billing/`, `pages/`, `mcp/`, `ops/`. The mutation floors in design 01 apply to `normalize/` and `billing/`.
3. First-year ceiling: 60 sources, 5 million licenses, 50 million snapshot rows, 20 million events, 200 API keys, 50 requests per second peak; the schema is sized for ten times that, and sharding is a non-goal.
4. A source is enabled only after an approved legal register entry exists (Section 11). The owner approves; agents draft.
5. Agents build everything in S and M issues (Section 13); judgments outside the spec are listed in Section 14 with defaults.

**Non-goals.** Geocoding beyond published coordinates (later than M2). Cross-jurisdiction entity matching other than by deterministic keys (Section 2.4). Search ranking. Real-time crawling (hourly minimum, daily normal). Any record about an individual beyond what the source publishes as holder or qualifier. A SaaS UI (steady state per design 01).

## 2. Domain model

Conventions: all ids are `uuid` v7 (time-ordered, generated in the application) unless stated. All timestamps are `timestamptz` in UTC. "Prov" means the four provenance columns `source_id`, `document_id`, `fetched_at`, `parser_version`, present on every canonical row that derives from a source document. Every table also carries `created_at` and `updated_at`. Text is `text`; there are no `varchar(n)` limits in Postgres, limits live in pydantic.

### 2.1 Entities and fields

**Jurisdiction**

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| slug | text unique | no | `us-ca`, `us-ny-new-york-city`, `us-il-chicago` |
| name | text | no | Display name |
| level | enum `state, county, city, special` | no | |
| state_code | char(2) | no | USPS code; `US` for federal |
| fips | text | yes | State or county FIPS; place code for cities |
| geoid | text | yes | Census GEOID when known |
| timezone | text | no | IANA name, used to interpret source dates |
| parent_id | uuid | yes | City to state |

**Source**

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| slug | text unique | no | `ca-cslb-bulk`, `nyc-dca-opendata`, `il-idfpr-pdf` |
| jurisdiction_id | uuid | no | |
| name | text | no | Agency and dataset name |
| url | text | no | Entry point |
| kind | enum `bulk_file, open_data_api, html_portal, pdf` | no | Adapter tier |
| record_class | text | no | `contractor_license`, `str_registration`, later verticals |
| schedule_cron | text | no | 5-field cron, UTC |
| sla_class | enum `A, B, C` | no | Section 6.2 |
| legal_register_id | uuid | no | FK; a source cannot be created without one |
| adapter_module | text | no | `records.adapters.ca_cslb` |
| adapter_version | text | no | Semver of the adapter |
| enabled | bool | no | Default false |
| publish_state | enum `draft, publishing, paused, retired` | no | Section 6.5 |
| config_yaml | text | no | Validated `SourceConfig` (Section 3.2) |

**Entity** (the license holder as a business)

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| entity_key | text unique | no | Deterministic key, Section 2.4 |
| legal_name | text | no | As published, whitespace-normalized |
| legal_name_norm | text | no | Uppercased, punctuation stripped, suffix normalized (`INC`, `LLC`) |
| entity_type | enum `corporation, llc, partnership, sole_proprietor, individual, government, unknown` | no | Mapped from source-native with native kept in `entity_type_native` |
| entity_type_native | text | yes | |
| dba_names | text[] | no | Default empty |
| primary_jurisdiction_id | uuid | no | Jurisdiction of the key source |
| phone | text | yes | E.164 when parseable, as published otherwise; business only |
| email | text | yes | Only if published as a business contact and the legal register permits it |
| website | text | yes | |
| is_individual | bool | no | True when the holder is a natural person (Section 11.4 gates publication) |
| external_ids | jsonb | no | `{source_slug: native_id}` |
| Prov | | | fetched_at of the most recent contributing document |

**Address** (separate table; one entity has many)

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| entity_id | uuid | no | |
| role | enum `business, mailing, premises` | no | |
| line1, line2, city, state, postal_code | text | line1 and state no, others yes | Normalized by `normalize.address` |
| country | char(2) | no | Default `US` |
| lat, lon | double | yes | Only if published |
| is_residential_flagged | bool | no | Set by heuristic; residential addresses of individuals are excluded from pages, Section 11.4 |
| address_hash | text | no | For dedupe |
| Prov | | | |

**License**

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| license_key | text unique | no | `{jurisdiction_slug}:{license_type_code}:{license_number_norm}` |
| entity_id | uuid | no | |
| jurisdiction_id | uuid | no | |
| source_id | uuid | no | Primary source |
| license_number | text | no | As published |
| license_number_norm | text | no | Uppercased, separators removed |
| license_type | text | no | Normalized family, Section 2.2 |
| license_type_native | text | no | |
| classifications | text[] | no | Normalized codes, default empty |
| classifications_native | text[] | no | |
| status | enum `LicenseStatus` | no | |
| status_native | text | no | |
| status_detail | text | yes | Free text from source (for example "Suspended: workers comp") |
| issued_on | date | yes | |
| expires_on | date | yes | |
| last_renewed_on | date | yes | |
| bond_amount_cents | bigint | yes | |
| bond_status | enum `active, cancelled, none, unknown` | no | Default `unknown` |
| insurance_status | enum `active, lapsed, exempt, none, unknown` | no | Default `unknown` |
| workers_comp_status | same enum | no | |
| source_url | text | yes | Deep link to the source record when stable |
| first_seen_at | timestamptz | no | |
| last_seen_at | timestamptz | no | |
| current_snapshot_id | uuid | no | FK to `license_snapshot` |
| Prov | | | |

**Qualifier** (person named on a license, as published)

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| license_id | uuid | no | |
| person_name | text | no | Exactly as published, no parsing into parts beyond `name_norm` |
| name_norm | text | no | |
| role | enum `qualifying_individual, responsible_managing_officer, officer, owner, partner, other` | no | |
| role_native | text | yes | |
| associated_on, disassociated_on | date | yes | |
| Prov | | | |

**Event**

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| event_key | text unique | no | Section 5.4 |
| license_id | uuid | yes | One of license_id or entity_id is set |
| entity_id | uuid | yes | |
| kind | enum `EventKind` | no | |
| field | text | yes | Canonical field that changed, when kind is a field-change |
| old_value, new_value | jsonb | yes | |
| effective_on | date | yes | Date the source states; null when unknown |
| observed_at | timestamptz | no | fetched_at of the snapshot that first showed the change |
| confidence | enum `stated, inferred` | no | `stated` when the source publishes the event itself, `inferred` when derived from a diff |
| details | jsonb | no | Source-native detail, for example the disciplinary case number |
| prev_snapshot_id, snapshot_id | uuid | prev yes, snapshot no | The two snapshots that bound the change |
| Prov | | | |

**Document**

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| source_id | uuid | no | |
| run_id | uuid | no | The crawl run |
| url | text | no | |
| fetched_at | timestamptz | no | |
| http_status | int | yes | |
| content_type | text | yes | |
| content_sha256 | text | no | |
| byte_length | bigint | no | |
| r2_key | text | no | Section 4.1 |
| etag, last_modified | text | yes | For conditional requests |
| parser_version | text | yes | Set when parsed |
| parse_status | enum `pending, ok, failed, skipped_unchanged` | no | |

**LicenseSnapshot** (one per license per document in which it appears; the input to event derivation)

| Field | Type | Null | Notes |
|---|---|---|---|
| id | uuid | no | |
| license_key | text | no | |
| document_id | uuid | no | |
| fetched_at | timestamptz | no | |
| canonical | jsonb | no | The normalized `LicenseRecord` |
| canonical_hash | text | no | Hash of `canonical` with volatile fields removed |
| Unique | (license_key, document_id) | | |

### 2.2 Vocabularies

Every mapped field keeps the source-native value beside it (`status` and `status_native`, `license_type` and `license_type_native`, and so on). Mapping is a pure function in `normalize/vocab/` from `(source_slug, native_value)` to a family, defined in `vocab/<field>.yaml` with one explicit row per source-native value. Unmapped values map to `unknown`, increment `vocab_unmapped_total{source,field}` and open an issue (Section 6.1) without blocking publication, because the native value is still served.

`LicenseStatus`: `active`, `active_conditional` (probation, restricted), `inactive` (voluntary, not expired), `expired`, `suspended`, `revoked`, `cancelled`, `pending`, `denied`, `deceased_closed`, `unknown`. Philosophy: the family answers "can this holder lawfully work today, and if not, why not" in eleven answers; nuance stays in `status_native` and `status_detail`. A native value maps to exactly one family; a compound value such as "Expired - Suspended" follows the precedence rule `revoked > suspended > denied > expired > cancelled > inactive > active_conditional > active > pending > unknown`.

`EventKind`: `issued`, `renewed`, `expired`, `reinstated`, `suspended`, `revoked`, `cancelled`, `status_changed` (fallback for any other status transition), `classification_added`, `classification_removed`, `qualifier_added`, `qualifier_removed`, `bond_cancelled`, `bond_restored`, `insurance_lapsed`, `insurance_restored`, `workers_comp_lapsed`, `workers_comp_restored`, `disciplinary_action`, `complaint_filed`, `complaint_closed`, `citation_issued`, `address_changed`, `name_changed`, `expiry_changed`, `first_observed`, `no_longer_published`. Kinds from `issued` through `cancelled` are derived from `status` transitions by the table in Section 5.2. `disciplinary_action`, `complaint_*` and `citation_issued` are `stated` events parsed from enforcement listings when a source publishes them.

License types and classifications use the same mechanism with a two-level family: `license_type` family (`general_building`, `general_engineering`, `specialty_trade`, `home_improvement`, `residential`, `other`) and `classifications` codes drawn from a cross-walk file `vocab/classification.yaml` that maps, for example, California `C-10` and Florida `EC` both to `electrical`. The cross-walk is the normalization moat and is human-reviewed: an agent proposes rows in a PR; the file is under the human-only path protection like fixtures.

### 2.3 Identifiers

`license_key = f"{jurisdiction.slug}:{license_type_code}:{license_number_norm}"` where `license_type_code` is the source-native type code when the jurisdiction reuses numbers across types (Florida does) and the literal `-` otherwise. The per-source config declares which (`license_number_scope: per_type | global`).

`entity_key`: when the source publishes a stable business identifier (California CSLB license number is per business; NYC publishes a business id), `entity_key = f"{source_slug}:{native_business_id}"`. Otherwise `entity_key = f"{jurisdiction_slug}:name:{legal_name_norm}:{postal_code5}"`, which is deterministic and conservative: two businesses with the same normalized name in the same ZIP collide, two spellings of one business split. Both failure modes are visible and fixable in the review queue.

### 2.4 Entity resolution

Rules, in order:

1. Deterministic key match is the only automatic merge. Same `entity_key` means the same entity.
2. Fuzzy matching is allowed only to propose, never to merge. The proposer runs nightly over new entities and emits `entity_merge_candidate` rows when, within one jurisdiction, two entities have Jaro-Winkler similarity over 0.94 on `legal_name_norm` and either the same `address_hash` or the same normalized phone, and the entities are not individuals (`is_individual = false`). Cross-jurisdiction proposals require an exact `legal_name_norm` match plus the same phone or the same qualifier `name_norm`.
3. Human review queue: table `entity_merge_candidate(id, entity_a, entity_b, score, reasons jsonb, status enum pending|accepted|rejected, decided_by, decided_at)`. Accepting writes an `entity_alias(alias_entity_id, canonical_entity_id)` row that the API resolves on read; nothing is physically merged, so deleting the alias reverses a wrong decision. The queue appears in the Sunday digest when over 20 rows are pending.
4. Individuals (sole proprietors, qualifiers) are never fuzzily matched across jurisdictions. A person's name appears only within the license it was published on.

## 3. Source adapter framework

### 3.1 Interface

Every adapter is a module under `records/adapters/<slug>/` exposing one class that implements `SourceAdapter`. Simple HTML and bulk sources are fully declarative (`adapter.yaml` plus the generic `DeclarativeAdapter`); anything else subclasses and overrides only the step it needs.

```python
class SourceAdapter(Protocol):
    config: SourceConfig
    parser_version: str                      # semver, bumped on any parse change

    async def discover(self, ctx: RunContext) -> AsyncIterator[FetchTarget]:
        """Yield the URLs or request specs to fetch this run (a bulk file,
        a paginated listing, or one search request per seed)."""

    async def fetch(self, ctx: RunContext, target: FetchTarget) -> RawDocument:
        """Fetch through ctx.http (policy-enforced client). Never bypass it."""

    def parse(self, doc: RawDocument) -> Iterator[NativeRecord]:
        """Pure. Bytes in, source-native dict rows out. No network, no clock."""

    def normalize(self, rec: NativeRecord) -> LicenseRecord | EnforcementRecord:
        """Pure. Native row to canonical pydantic model, vocab-mapped,
        native values preserved."""

@dataclass(frozen=True)
class FetchTarget:
    url: str
    method: Literal["GET", "POST"] = "GET"
    params: Mapping[str, str] = field(default_factory=dict)
    body: bytes | None = None
    render_js: bool = False                  # Playwright only when true
    cache_key: str | None = None

class RawDocument(BaseModel):
    document_id: UUID
    target: FetchTarget
    fetched_at: datetime
    http_status: int | None
    content_type: str | None
    body: bytes
    sha256: str
    etag: str | None
    last_modified: str | None
```

`RunContext` carries `run_id`, `source`, `http` (the policy client), `clock`, `fixture_mode` (`live | record | replay`) and `metrics`. `parse` and `normalize` are pure so they property-test and run in CI offline.

### 3.2 SourceConfig YAML

```yaml
slug: ca-cslb-bulk
jurisdiction: us-ca
name: California Contractors State License Board, public data extract
record_class: contractor_license
kind: bulk_file                 # bulk_file | open_data_api | html_portal | pdf
entry_url: https://www.cslb.ca.gov/...
schedule: "15 9 * * *"          # UTC cron; daily
sla_class: A                    # A daily, B weekly, C monthly (Section 6.2)
legal_register: LR-0001         # must be status approved before enabled: true
enabled: false
license_number_scope: global    # or per_type
fetch:
  rate_limit_rps: 0.5
  concurrency: 1
  politeness_window_utc: ["02:00", "12:00"]   # optional; fetch only in window
  timeout_s: 60
  max_retries: 5
  respect_robots: true          # cannot be set false; present for audit only
  conditional_requests: true
  render_js: false
parse:
  format: csv                   # csv | json | html | pdf | xlsx | fixed_width
  encoding: latin-1
  csv:
    delimiter: ","
    header_row: 0
  html:                         # for html_portal
    listing_selector: "table#results tr"
    detail_link_selector: "td a"
    pagination: {kind: next_link, selector: "a.next", max_pages: 5000}
  pdf:
    strategy: table_then_text   # pdfplumber tables; text regex fallback
    table_settings: {vertical_strategy: lines, horizontal_strategy: lines}
fields:                         # native column -> canonical field, with normalizer
  LicenseNo: {to: license_number, normalizer: license_number}
  BusinessName: {to: legal_name, normalizer: name}
  PrimaryStatus: {to: status, normalizer: vocab, vocab: status}
  IssueDate: {to: issued_on, normalizer: date, formats: ["%m/%d/%Y"]}
  Classifications: {to: classifications, normalizer: classification_list, sep: "|"}
canaries:                       # Section 6.3
  - {license_number: "123456", expect: {status: active}}
  - {license_number: "654321", expect_present: true}
anomaly_overrides:              # optional; defaults in Section 6.1
  record_count_drop_pct: 20
```

`records validate-config` (pydantic `SourceConfig`) runs in the fast gate and fails on unknown keys, `enabled: true` without an approved register entry, or `respect_robots: false`.

### 3.3 Adapter tiers

| Tier | Kind | Discover | Fetch | Parse | Example (hypothesis) |
|---|---|---|---|---|---|
| 1 | `bulk_file` | One or a few file URLs, possibly behind a listing page | httpx streaming to R2, conditional GET | csv, xlsx, fixed width, zip of same | California CSLB extract, Florida DBPR, Oregon CCB, Washington L&I, Texas TDLR, Virginia DPOR, Utah DOPL |
| 2 | `open_data_api` | Dataset id, SoQL or OData paging | httpx with `$limit`/`$offset` or cursor | json | NYC Open Data (DCWP licenses), Chicago data portal |
| 3 | `html_portal` | Seed enumeration: license number ranges, name prefixes, or ZIP lists declared in config; detail pages per hit | httpx first; Playwright only when `render_js: true` after a documented check that the page needs it | html via selectolax; one `NativeRecord` per detail page | State boards with vendor-hosted lookup portals |
| 4 | `pdf` | Monthly disciplinary action lists, board meeting minutes | httpx | pdfplumber tables, text regex fallback, page-level fixtures | Boards that publish enforcement only as PDF |

A source may have two adapters (a bulk file for the roster and a PDF for enforcement); each is its own `Source` row with its own schedule, and both write to the same `license_key` space.

### 3.4 Fetch policy

Enforced in `records.fetch.PolicyClient`, the only HTTP client adapters receive. Tests assert that `httpx.AsyncClient` is not imported anywhere under `adapters/` (a Semgrep rule).

| Rule | Implementation |
|---|---|
| robots.txt | Fetched and cached 24 hours per host; `urllib.robotparser` with our agent token and `*`; a disallowed URL raises `RobotsDisallowed` and the run is marked `blocked_by_policy`. Not configurable off. |
| Rate limit | Token bucket per source (`rate_limit_rps`, default 0.5) and a global per-host bucket of 2 rps shared by all sources on that host. `Retry-After` and HTTP 429 double the interval for 10 minutes. |
| User-Agent | `RecordsPlatformBot/1.0 (+https://<domain>/bot; data@<domain>)`; `From:` header with the same address. |
| Retries | 5 attempts, exponential backoff `1.5^n` seconds plus jitter up to 1 second, on 5xx, 429, connect and read timeouts. 4xx other than 429 are not retried. |
| Conditional requests | Send `If-None-Match` and `If-Modified-Since` from the last `Document` for the same `cache_key`; a 304 writes a `Document` with `parse_status = skipped_unchanged` and no body. |
| Caching | Local on-disk cache keyed by `cache_key` for the duration of a run so that a crashed run resumes without refetching. |
| Politeness windows | Optional `politeness_window_utc`; the scheduler only dispatches inside the window. Default for HTML portals: 02:00 to 12:00 UTC (agency night). |
| Budgets | Per run: `max_requests` (default 50,000) and `max_bytes` (default 5 GB); exceeding either stops the run with `BudgetExceeded`. |
| Playwright | Only when `render_js: true`; headless Chromium, same UA, same rate limit; no form submission except declared search forms; never solves CAPTCHAs. A CAPTCHA page raises `AntiBotEncountered`, the source is paused and an issue opens. |

### 3.5 Fixtures and offline replay

`fixture_mode = record` writes every `RawDocument` for a run under `tests/fixtures/recorded/<slug>/<yyyy-mm-dd>/<sha256-prefix>.<ext>` plus `manifest.json` (targets, headers, hashes) and the expected normalized output `expected.jsonl`. `replay` serves fetches from the manifest and fails on any target that is not recorded. The directory is human-only (guard-paths hook); agents add fixtures only in a PR labeled `fixture-update` that the owner merges. Each adapter ships one recorded run of 50 or more records (or the full set) and one edge case (empty page, malformed row, split PDF table).

### 3.6 Parser versioning

`parser_version` is semver in the adapter module: patch for identical fixture output, minor when some records change (a new field mapped), major when field semantics change. CI diffs adapter output against `expected.jsonl`; a diff without a version bump fails, and a bump requires the diff summary in the PR. Snapshots store `parser_version`, so re-parsing (Section 5.5) can be scoped to older versions.

### 3.7 Error taxonomy

| Class | Meaning | Handling |
|---|---|---|
| `PolicyError` (`RobotsDisallowed`, `BudgetExceeded`, `AntiBotEncountered`, `OutsidePolitenessWindow`) | We must not proceed | Run status `blocked_by_policy`; source paused; issue opened; never retried automatically |
| `FetchError` (`Timeout`, `ServerError`, `RateLimited`, `NotFound`) | Transient or source-side | Retried per policy; after exhaustion the target is recorded as failed; run continues; run fails if over 5% of targets fail |
| `ParseError` (`SchemaDrift`, `MalformedRow`, `EmptyDocument`, `UnsupportedFormat`) | Our parser cannot read the document | Row-level errors are counted and the row skipped; document-level errors mark `parse_status = failed`; thresholds in Section 6.1 |
| `NormalizeError` (`UnmappedVocab`, `InvalidDate`, `InvalidIdentifier`) | Data does not fit the canonical model | `UnmappedVocab` degrades to `unknown` plus metric; the others skip the row and count |
| `StoreError` | Database or R2 failure | Retried 3 times; run fails; no partial publication |

Every error carries `source_slug`, `run_id`, `document_id`, `target.url` and, for row errors, the row index and first 200 bytes, enough for an agent to write a failing fixture test.

## 4. Storage

### 4.1 Raw snapshots in R2

Bucket `records-raw` (immutable; object lock on, 7-year retention for the first version of every key). Key scheme:

```
raw/{source_slug}/{yyyy}/{mm}/{dd}/{run_id}/{sha256}.{ext}
raw/{source_slug}/{yyyy}/{mm}/{dd}/{run_id}/manifest.json
```

The manifest lists every document with URL, headers, hash and length. Unchanged content is still stored per run (storage is cheap, auditability is not); `Document.content_sha256` lets the pipeline skip re-parsing it. Bucket `records-bulk` holds the exports of Section 4.4; fixtures live in git.

### 4.2 Staging

Staging is Postgres schema `staging`, one table per source generated from the adapter's native columns: `staging.src_<slug>(document_id, row_index, native jsonb, parse_error text)`. Staging is truncated per source after a successful canonical load (the raw document in R2 is the durable copy); it exists so an agent can inspect parser output with SQL and rerun `normalize` without refetching.

### 4.3 Canonical schema

Schema `core`. Tables follow Section 2 one to one: `jurisdiction`, `source`, `legal_register`, `entity`, `entity_alias`, `entity_merge_candidate`, `address`, `license`, `qualifier`, `license_snapshot`, `event`, `document`, `crawl_run`, `canary`, `quality_check`. Serving tables in schema `api`: `api_key`, `api_key_scope`, `usage_event`, `webhook_subscription`, `webhook_delivery`, `customer`, `plan`, `credit_ledger`. Correction flow in schema `ops`: `correction_request`, `removal_request`, `issue_link`.

Indexes that matter:

| Table | Index | Purpose |
|---|---|---|
| license | unique (license_key); (entity_id); (jurisdiction_id, status); (license_number_norm) ; GIN (classifications) | Lookup by number, holder, status filters |
| entity | unique (entity_key); GIN trigram on legal_name_norm (pg_trgm); (primary_jurisdiction_id) | Name search |
| address | (entity_id); (postal_code, state) ; trigram on line1 | Address lookup |
| qualifier | (license_id); trigram on name_norm | Qualifier search |
| event | unique (event_key); (license_id, observed_at); (jurisdiction_id, observed_at desc); (kind, observed_at desc); BRIN (observed_at) | Change feed, history |
| license_snapshot | unique (license_key, document_id); (license_key, fetched_at desc) | Diff against previous |
| document | (source_id, fetched_at desc); (content_sha256) | Skip unchanged |
| usage_event | unique (idempotency_key); (api_key_id, occurred_at) | Metering |
| webhook_delivery | (subscription_id, status, next_attempt_at) | Retry worker |

`event` is partitioned by range on `observed_at` by month from the first production month; partitions are created 3 months ahead by a migration-free maintenance job. `license_snapshot` is partitioned by month on `fetched_at`.

### 4.4 Parquet for bulk

Written nightly after publication gating by a DuckDB job reading Postgres:

```
bulk/v1/{dataset}/jurisdiction={slug}/snapshot_date={yyyy-mm-dd}/part-0000.parquet
bulk/v1/events/jurisdiction={slug}/observed_month={yyyy-mm}/part-0000.parquet
bulk/v1/{dataset}/latest/jurisdiction={slug}/part-0000.parquet   # overwritten nightly
```

Datasets: `licenses`, `entities`, `addresses`, `qualifiers`, `events`. Row groups of 128 MB, zstd compression, a `_schema.json` beside each dataset root with the pydantic-exported JSON Schema and the `parser_version` range. CSV mirrors the `latest` partition only. Warehouse shares (Snowflake, BigQuery) read the same Parquet through external tables; nothing is copied.

### 4.5 Retention

| Data | Retention |
|---|---|
| Raw documents in R2 | 7 years, then owner decision; never deleted while a snapshot references them |
| Staging | Until the next successful load of that source |
| license_snapshot | Forever for rows that bound an event; otherwise 24 months, then collapsed to one row per license per quarter |
| event | Forever |
| document rows | Forever (small) |
| API request logs | 30 days raw, 13 months aggregated per key per day |
| usage_event | 7 years (billing records) |
| webhook_delivery bodies | 14 days; status rows 13 months |
| Removal requests | Suppression list forever; request detail 3 years |

## 5. Event derivation

### 5.1 Algorithm

Runs once per successful crawl run after canonical load, inside one transaction per batch of 1,000 licenses.

```
for each license_key K that appears in run R (document D, snapshot S_new):
    S_prev = latest snapshot for K with fetched_at < S_new.fetched_at
            (any source for the same jurisdiction; prefer the same source)
    if S_prev is None:
        emit Event(kind=first_observed, snapshot=S_new, prev=None,
                   confidence=inferred, effective_on=S_new.issued_on)
        continue
    if S_prev.canonical_hash == S_new.canonical_hash:
        continue                                   # no change, no event
    for each field F in DIFF_FIELDS (Section 5.2), in table order:
        if S_prev.F != S_new.F:
            emit the event(s) the rule for F yields
for each license_key K published by source X in the previous run of X
    but absent from run R of X, when R was a full crawl (not incremental):
    if absence has persisted for 2 consecutive full runs:
        emit Event(kind=no_longer_published, confidence=inferred)
```

A full crawl is a run whose `discover()` enumerated the whole roster; incremental runs (seeded lookups, a disciplinary PDF) never produce `no_longer_published`.

### 5.2 Per-field rules

| Field | Rule | Kind |
|---|---|---|
| status | Transition table: `* -> active` from `pending` is `issued`; from `expired, inactive, suspended, revoked, cancelled` is `reinstated`; `* -> suspended` is `suspended`; `* -> revoked` is `revoked`; `* -> expired` is `expired`; `* -> cancelled` is `cancelled`; anything else is `status_changed` | as listed |
| expires_on | Later date is `renewed` when the new value is at least 180 days after the old; otherwise `expiry_changed` | `renewed`, `expiry_changed` |
| last_renewed_on | If changed and no `renewed` event emitted from `expires_on` in the same diff, emit `renewed` | `renewed` |
| classifications | Set difference; one event per code | `classification_added`, `classification_removed` |
| qualifiers (by name_norm and role) | Set difference | `qualifier_added`, `qualifier_removed` |
| bond_status | `active -> cancelled` is `bond_cancelled`; `cancelled -> active` is `bond_restored`; transitions to or from `unknown` emit nothing | |
| insurance_status, workers_comp_status | Same pattern with `lapsed` | |
| legal_name | Changed `legal_name_norm` | `name_changed` |
| addresses (business role) | Changed `address_hash` | `address_changed` |
| dba_names, phone, website, status_detail | No event; the current value is served | |

`effective_on` is the source-stated date when the field has one (`issued_on` for `issued`, `last_renewed_on` for `renewed`, a disciplinary action date for `disciplinary_action`); otherwise null, and `observed_at` is the only time. The API documents this distinction, because insurers care about the gap.

Stated events (enforcement PDFs, complaint datasets) bypass the diff: each `EnforcementRecord` becomes an `Event` with `confidence = stated` and an `event_key` from the native case identifier, so re-parsing never duplicates.

### 5.3 Transitions to and from `unknown`

A change from any status to `unknown` emits nothing and raises `vocab_unmapped_total`. A change from `unknown` to a known status emits `status_changed`, not `issued` or `reinstated`, because the prior state is not known.

### 5.4 Idempotence

`event_key = sha256(license_key | kind | field | prev_snapshot_id | snapshot_id)` for inferred events and `sha256(source_slug | native_case_id | kind)` for stated ones. Inserts are `ON CONFLICT (event_key) DO NOTHING`. Re-running derivation for any run is therefore safe, and the invariant test "derive twice, count once" runs in CI on fixtures.

### 5.5 Backfill

Backfill is derivation over historical snapshots in `fetched_at` order with conflict-skip, never deletion: `records events backfill --source X --from DATE`. When a parser bump changes old output (`records parse replay --source X --min-parser-version`), new snapshot rows carry the new `parser_version`, derivation re-runs for affected licenses, and inferred events whose bounding snapshots are no longer consecutive are marked `superseded_at`; the API excludes superseded events by default.

### 5.6 Late and out-of-order data

Snapshots are ordered by `fetched_at`, never by arrival. A late document (for example a public-records-request file loaded later) with an earlier `fetched_at` re-runs derivation for affected licenses from that point; because `event_key` includes both bounding snapshot ids, split pairs become `superseded_at` and the correct intermediate events are inserted. Webhook payloads carry `superseded_event_ids` so consumers can reconcile.

## 6. Data quality and freshness

### 6.1 Anomaly rules

Evaluated per run before publication. Each rule writes a `quality_check(run_id, rule, value, threshold, passed)` row.

| Rule | Threshold (default, overridable per source) | Consequence |
|---|---|---|
| Record count drop | Count under 80% of the median of the last 5 successful runs | Fail |
| Record count spike | Count over 150% of that median | Warn (publish, open issue) |
| Schema drift | Any expected native column missing, or over 2 new columns | Fail |
| Parse error rate | Row-level `ParseError` plus `NormalizeError` over 1% of rows, or over 50 rows for small sources | Fail |
| Unmapped vocab | Any new `(field, native_value)` not in the vocab file | Warn |
| Status distribution shift | Share of any `LicenseStatus` family moves over 10 percentage points versus the previous run | Fail |
| Event burst | Inferred events over 5 times the median of the last 5 runs and over 500 | Fail (the likely cause is a parser change, not reality) |
| Fetch failure rate | Over 5% of targets failed after retries | Fail |
| Canary failure | Any canary assertion false | Fail |
| Required-field coverage | Any required canonical field (license_number, legal_name, status) below 99% non-null, or any field declared `expected` in config below its historical minimum minus 5 points | Fail for required, warn for expected |

### 6.2 Freshness SLA classes

| Class | Sources | Schedule | Stale after | Watchdog action |
|---|---|---|---|---|
| A | Bulk files and APIs with daily updates; enforcement feeds | Daily | 48 hours since last successful published run | Issue at 48 h, `needs-human` at 96 h |
| B | Weekly bulk, portal enumerations | Weekly | 10 days | Issue at 10 d, `needs-human` at 17 d |
| C | Monthly PDFs, slow boards | Monthly | 40 days | Issue at 40 d |

Jurisdiction pages and `/v1/coverage` publish `last_published_at` and `sla_class`, so buyers see honest staleness.

### 6.3 Canary records

Each source config declares 3 to 10 canaries: known licenses with expected `status` (chosen among long-stable records such as large firms) and at least one `expect_present`. After every run the canary check asserts each; a status expectation that fails twice in a row because the license genuinely changed is a `needs-human` item (the owner updates the canary; the file is human-only).

### 6.4 Coverage metrics

Per source and jurisdiction, computed nightly and stored in `coverage_daily`: `licenses_total`, `licenses_active`, `required_field_coverage` (per field), `events_30d`, `last_published_at`, `sla_met` (bool), `vocab_unmapped`. These feed the dashboard, the pages, the `/v1/coverage` endpoint and the watchdog.

### 6.5 Publication gating

Canonical rows from a run stay `pending` (`license.published_run_id` lags) until all Fail-class checks pass; `publish_run(run_id)` then flips the pointer in one transaction and derives events, which only then reach the feed and webhooks. A failed run sets `source.publish_state = paused`, opens an issue titled `[quality] <slug>: <rule> <value> vs <threshold>` labeled `data-source`, and the previous published state keeps serving. Resuming is `records source resume <slug>` by a human, or an agent PR whose parser fix passes the recorded fixture and a fresh replay.

### 6.6 Watchdog consumption

Watchdog A (every 6 hours, design 00) runs `records watchdog` which: lists sources past SLA; lists runs with Fail checks lacking an open issue and opens one; checks API 5xx rate (Section 10.6); checks webhook delivery failure rate; checks Stripe webhook lag; posts a one-paragraph summary to the digest file. It never modifies data.

## 7. API

### 7.1 Resources

Base `https://api.<domain>/v1`. Resources: `jurisdictions`, `sources`, `coverage`, `licenses`, `entities`, `qualifiers`, `events`, `search`, `webhooks`, `bulk`, `me`. JSON only; all responses carry `request_id`, and lists carry `next_cursor`.

### 7.2 Endpoints

| Method and path | Purpose | Credits |
|---|---|---|
| GET /v1/jurisdictions | List jurisdictions with coverage summary | 0 |
| GET /v1/jurisdictions/{slug} | One jurisdiction, sources, freshness | 0 |
| GET /v1/coverage | Per-source freshness and counts | 0 |
| GET /v1/licenses/{license_key} | One license with current fields, holder, qualifiers | 1 |
| GET /v1/licenses/{license_key}/history | Snapshots and events for one license | 1 |
| GET /v1/licenses?jurisdiction=&number=&status=&classification=&expires_before=&updated_since=&cursor=&limit= | Filtered list | 1 per page |
| GET /v1/entities/{id} | Entity with addresses and licenses | 1 |
| GET /v1/entities?name=&jurisdiction=&postal_code=&phone= | Entity search (trigram) | 1 per page |
| GET /v1/qualifiers?name=&jurisdiction= | Qualifier search | 1 per page |
| GET /v1/search?q=&jurisdiction= | Combined lookup across number, name, qualifier | 1 per page |
| GET /v1/events?jurisdiction=&kind=&license_key=&entity_id=&since=&until=&cursor= | Polling change feed | 1 per 100 events |
| GET /v1/events/{id} | One event | 1 |
| POST /v1/webhooks | Create subscription | 0 |
| GET, PATCH, DELETE /v1/webhooks/{id} | Manage | 0 |
| GET /v1/webhooks/{id}/deliveries?since= | Delivery log | 0 |
| POST /v1/webhooks/{id}/replay | Re-deliver events in a window | 1 per 100 events |
| GET /v1/bulk/datasets | List datasets, partitions, schema URLs | 0 |
| POST /v1/bulk/datasets/{dataset}/download | Signed R2 URLs for a partition set | Plan-gated, 0 credits |
| GET /v1/me | Key, scopes, plan, credits used and remaining | 0 |
| GET /v1/openapi.json | The spec | 0 |

Example:

```
GET /v1/licenses/us-ca:-:1012345
Authorization: Bearer rk_live_...

200 OK
{
  "license_key": "us-ca:-:1012345",
  "jurisdiction": "us-ca",
  "license_number": "1012345",
  "license_type": "general_building", "license_type_native": "B",
  "classifications": ["general_building", "electrical"],
  "classifications_native": ["B", "C-10"],
  "status": "active", "status_native": "Active", "status_detail": null,
  "issued_on": "2018-03-02", "expires_on": "2026-03-31", "last_renewed_on": "2024-02-10",
  "bond_status": "active", "insurance_status": "unknown", "workers_comp_status": "exempt",
  "entity": {"id": "...", "legal_name": "EXAMPLE BUILDERS INC", "entity_type": "corporation",
             "addresses": [{"role": "business", "city": "FRESNO", "state": "CA", "postal_code": "93721"}]},
  "qualifiers": [{"person_name": "J DOE", "role": "responsible_managing_officer"}],
  "provenance": {"source": "ca-cslb-bulk", "fetched_at": "2026-11-01T09:20:11Z",
                 "parser_version": "1.3.0", "source_url": "https://..."},
  "links": {"history": "/v1/licenses/us-ca:-:1012345/history"}
}
```

### 7.3 Pagination, filtering, sorting

Cursor pagination only (`cursor` opaque base64 of the sort key and id; `limit` 1 to 200, default 50). Filters are exact-match query parameters, with `_since`, `_before` suffixes for dates and comma lists for enums. Sorting: `sort=updated_at|-updated_at|expires_on|-expires_on` on lists; events are always ascending `observed_at, id`. `updated_since` on licenses is the cheap way to sync without events.

### 7.4 Errors

RFC 9457 problem details: `{"type": "https://<domain>/errors/credit-exhausted", "title": "...", "status": 402, "detail": "...", "request_id": "..."}`. Codes: 400 validation, 401 bad key, 402 credits exhausted (free tier), 403 scope or plan, 404, 409 webhook conflict, 422 unprocessable filter, 429 rate limit (with `Retry-After`), 5xx.

### 7.5 Versioning

Path version `v1`. Additive changes (new fields, enum values flagged `x-may-grow: true`) do not bump; breaking changes ship as `v2` beside `v1` for at least 12 months with `Sunset` headers. Clients must treat unknown enum values as `unknown`.

### 7.6 Keys and scopes

Keys `rk_live_<32 bytes base62>` and `rk_test_...`, stored as SHA-256; the plaintext is shown once. Scopes: `read:licenses`, `read:entities`, `read:events`, `write:webhooks`, `read:bulk`, `admin:keys`. A customer can hold several keys; usage is per key, credits per customer. Test keys hit the same API against a fixed sample jurisdiction `us-xx-sample` with synthetic data and never meter.

### 7.7 Credit metering

A credit is one lookup, one list page, or 100 events delivered (feed page or webhook batch), matching design 01's "one lookup or one event" at a 100:1 batching discount so that a feed is cheaper than polling by hand. Zero-credit: coverage and jurisdiction endpoints, 304 responses, 4xx responses, the test key. Metering writes `usage_event(idempotency_key = request_id, api_key_id, credits, kind, occurred_at)` asynchronously through the job queue, so a metering outage never fails a request (Section 8.3 covers reconciliation).

### 7.8 Rate limits

Per key, sliding window: Free 60 requests per minute, Basic 600, Pro 1,200, Enterprise negotiated; burst 2x for 10 seconds. Per IP unauthenticated: 20 per minute (only `/v1/openapi.json` and coverage are reachable). Headers `RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset`.

### 7.9 Webhooks

Subscription: `{url, secret (optional; generated if absent), filters: {jurisdictions: [...], kinds: [...], license_keys: [...], entity_ids: [...]}, batch_max: 100, active: true}`. Pro and Enterprise only. Payload:

```json
{"id": "dlv_...", "subscription_id": "whs_...", "sent_at": "...", "attempt": 1,
 "events": [{"id": "...", "event_key": "...", "kind": "suspended", "license_key": "...",
             "entity_id": "...", "field": "status", "old_value": "active", "new_value": "suspended",
             "effective_on": "2026-10-30", "observed_at": "...", "confidence": "inferred",
             "superseded_event_ids": [], "provenance": {...}}],
 "next_cursor": "..."}
```

Signing: `X-Records-Signature: t=<unix>,v1=<hex hmac-sha256(secret, t + "." + body)>`; receivers reject when `|now - t| > 300 s`. Retry schedule on non-2xx or timeout (10 s): 1 min, 5 min, 30 min, 2 h, 8 h, 24 h, 48 h, then the subscription is marked `failing` and the customer emailed; after 7 days failing it is `disabled`. Deliveries are at-least-once and ordered per subscription (a failing batch blocks later ones). `POST /v1/webhooks/{id}/replay {since, until}` re-enqueues matching events.

### 7.10 Bulk

`POST /v1/bulk/datasets/{dataset}/download {jurisdictions, snapshot_date | latest, format: parquet|csv}` returns signed R2 URLs valid 1 hour. Enterprise gets a Snowflake share or BigQuery external table pointed at the same prefixes, provisioned by hand once (Section 14).

### 7.11 OpenAPI, SDKs, MCP

The FastAPI app exports `openapi.json`; a CI step diffs it against the committed `openapi/v1.json` and fails on uncommitted changes, so the spec is the reviewed artifact. SDKs: TypeScript via `openapi-typescript` plus a thin fetch wrapper, Python via `openapi-python-client`, both generated in CI, published on tag. The MCP server (`records/mcp/`) is a stdio and streamable-HTTP server that wraps the API with the user's key and exposes:

| Tool | Arguments (JSON Schema, all strings unless noted) | Maps to |
|---|---|---|
| `lookup_license` | `license_key` or (`jurisdiction`, `license_number`) | GET license |
| `search_entities` | `name`, `jurisdiction?`, `postal_code?`, `limit?: int` | GET entities |
| `search_qualifiers` | `name`, `jurisdiction?` | GET qualifiers |
| `license_history` | `license_key`, `since?: date` | GET history |
| `list_events` | `jurisdiction?`, `kinds?: string[]`, `license_key?`, `since: datetime`, `limit?: int` | GET events |
| `coverage` | `jurisdiction?` | GET coverage |
| `verify_license` | `jurisdiction`, `license_number`, `expected_name?` | Composite: lookup plus a plain-language summary with the provenance line and the "verify at the board" URL |

Every tool result includes `provenance` and `as_of`; the server never caches across users.

## 8. Billing

### 8.1 Stripe objects

| Object | Design |
|---|---|
| Products | `records-basic`, `records-pro`, `records-enterprise`, `records-overage` |
| Prices | Basic $299 per month flat (includes 25,000 credits); Pro $799 per month flat (100,000 credits, webhooks); Overage: metered price $0.015 per credit on Basic, $0.010 on Pro, billed monthly in arrears; Enterprise: invoice-only, no Stripe price, usage reported for the record |
| Meter | One Stripe Billing Meter `api_credits` with `event_name = api_credits`, aggregation `sum`, keyed by `stripe_customer_id` |
| Free | No Stripe subscription; 500 credits per month tracked only in `credit_ledger`; attribution required by terms; hard stop at 0 credits (402) |
| Enterprise | Stripe customer plus manual invoices; the metering job still reports usage so the owner has the number |

Pricing values are the design 01 hypothesis and are configuration in `billing/plans.yaml`, not code; changing them is a PR with a k6 load test attached (design 01 Section 5).

### 8.2 Credit accounting

`credit_ledger(id, customer_id, period_start, kind enum grant|usage|adjustment|overage_reported, credits int, reference, created_at)`. A monthly job on each subscription's period start inserts a `grant` row; usage is aggregated hourly from `usage_event` into one `usage` row per customer per hour (the ledger stays small); `remaining = sum(credits)` for the period. When remaining goes negative on Basic or Pro, the hourly job sends the negative delta to Stripe as a meter event with `identifier = f"{customer_id}:{hour_bucket}"`, which is Stripe's idempotency key, and writes an `overage_reported` row. Free customers are blocked at zero. Stripe webhooks consumed: `customer.subscription.*`, `invoice.paid`, `invoice.payment_failed` (grace 7 days, then key scopes drop to coverage only), `checkout.session.completed`. Webhook handling is idempotent on Stripe event id stored in `stripe_event(id)`.

### 8.3 Idempotent usage recording

`usage_event.idempotency_key = request_id`; the API emits the row through the job queue after the response; the queue job upserts with conflict-skip; a nightly reconciliation compares the Cloudflare request log count per key with `usage_event` and inserts the missing rows (never deletes). The authorization invariant test asserts that `/v1/me` for key A never reflects key B's usage and that a customer's ledger only references its own keys.

## 9. Public pages

Static, generated nightly by `records pages build` into `site/` and deployed to Cloudflare Pages; the docs site and dashboard share the build.

### 9.1 Jurisdiction page `/{jurisdiction_slug}/`

Sections: title and level; what records are covered and from which sources (linked to the agency); freshness (`last_published_at`, SLA class, days since update); counts by `LicenseStatus` family; 30-day event counts by kind; how to verify a license on the official site (link, steps); vocabulary cross-walk for this jurisdiction (native status and type values and their families); API examples for this jurisdiction; correction and removal link; data notice ("facts republished from the agency as published; not a consumer report; verify with the agency before relying").

### 9.2 Entity page `/{jurisdiction_slug}/{license_type_code}/{license_number_norm}/`

One page per license for business entities (`is_individual = false`) only. Sections: legal name and DBA names; license number, type, classifications (family and native); status (family, native, detail) with `as_of`; dates (issued, expires, last renewed); bond, insurance and workers comp status as published; business address (city, state, ZIP; street line only when the address is non-residential per the heuristic); qualifiers as published (name and role only); event timeline (kind, effective or observed date); provenance (source, fetched at, link to the official record); correction and removal link; the same data notice. Deliberately excluded: individuals' licenses (sole proprietors and personal licenses get no page), phone and email, mailing addresses, any address flagged residential, complaint narratives, anything not in the canonical model, any score, grade or recommendation.

### 9.3 Correction and removal flow

Form at `/requests/` (Cloudflare Pages function to a `ops.correction_request` row): request type (correction, removal), page URL, requester role (holder, agency, other), claim, evidence upload (PDF or image, 10 MB, stored in R2 `records-requests` private bucket), contact email. Auto-acknowledgement within one hour with a ticket id. SLA: a decision within 10 business days, tracked by the watchdog (`needs-human` at day 7 if undecided). Decisions: `corrected_at_source` (re-crawl; nothing to edit), `suppressed` (`is_suppressed = true` on page and API; the record stays in bulk only under enterprise terms, or is removed entirely when the source itself removed it), or `declined` with a reason. Removal is granted by default when the page concerns a natural person. Factual fields are never hand-edited; they are fixed by re-crawl or a parser fix, because the platform republishes the source.

### 9.4 Indexing and robots

Entity pages carry `<meta name="robots" content="noindex">` when the entity has under 2 licenses and no events in 24 months (thin pages hurt more than help) or is suppressed; pages about individuals are never generated. `robots.txt` disallows `/requests/`, `/search/`, `/api/`. Sitemaps: `sitemap-index.xml` plus one sitemap per jurisdiction of at most 50,000 URLs, regenerated nightly with `lastmod` from `license.updated_at`; only indexable pages are listed.

### 9.5 Generation

The nightly build renders Jinja2 templates from `coverage_daily` and the read replica per jurisdiction, re-rendering only entities updated since the last build. Budget: incremental under 30 minutes, full rebuild of 5 million pages under 4 hours. Playwright plus axe checks 20 sampled pages per build.

## 10. Infrastructure and operations

### 10.1 Topology

```
 Internet
   |            Cloudflare (DNS, WAF, rate limit, cache for GET coverage)
   |----------- Pages: site/, docs/, dashboard/ (static), /requests/ function
   |----------- api.<domain>  -> Fly.io app "records-api" (2 x shared-cpu-1x, 2 regions)
                                       |  asyncpg pool -> Neon Postgres (primary + read replica)
                                       |  job enqueue  -> Postgres queue (schema jobs)
 Fly.io app "records-worker" (1 x shared-cpu-2x; scale by queue depth)
   |  crawl runs, parse, normalize, derive, quality, webhook delivery, metering, pages build
   |  httpx/Playwright -> agency sites            R2: records-raw, records-bulk, records-requests
 Stripe <-> api (webhooks)      Sentry <- api, worker      Plausible <- site
 GitHub Actions: CI, nightly builder, watchdog (calls `records watchdog` via Fly exec)
```

### 10.2 Environments

`dev` (local, Docker Compose with Postgres and MinIO as R2 stand-in), `staging` (Fly app `records-api-staging`, Neon branch `staging`, Stripe test mode, synthetic sources only), `prod`. Neon branching gives staging a fresh copy of prod schema and a sampled 1% of data nightly.

### 10.3 Deploy

Merge to `main` deploys to staging (GitHub Actions, `fly deploy`) and runs the smoke suite (Schemathesis quick run plus 10 canary calls); a `v*` tag from the Release routine promotes to prod. Rollback is `fly releases rollback`, safe only within the migration compatibility window below.

### 10.4 Migrations

Alembic, one revision per PR labeled `migration`, expand-and-contract (add and backfill in one release, read in the next, drop one release later). Migrations run as a Fly release command; one over 60 seconds on staging must be batched. The guard-paths hook blocks `migrations/**` without the label.

### 10.5 Backups and restore drill

Neon point-in-time restore with a 7-day window (30 on the paid tier when revenue justifies it) plus a nightly `pg_dump` of `core` and `api` schemas to R2 `records-backups` (90 days). Monthly drill, run by the Maintenance routine: restore the latest dump into a Neon branch, run `records verify-restore` (row counts within 0.1% of production, 100 random licenses identical), record the duration in `ops/restore-drills.md`. A failed drill is `needs-human`.

### 10.6 Observability

Prometheus-style metrics exported by api and worker and scraped into Grafana Cloud free tier (or Fly metrics); Sentry for exceptions with `source_slug` and `run_id` tags; uptime via Better Stack free tier on `/v1/coverage` and the site.

| Metric | Alert |
|---|---|
| `api_requests_total{status}` 5xx share | Over 1% for 10 min |
| `api_latency_seconds` p95 | Over 500 ms for 15 min |
| `crawl_run_duration_seconds{source}` | Over 3x the source median |
| `crawl_fetch_errors_total{source}` | Over 5% of targets |
| `quality_check_failed_total` | Any, opens issue (Section 6.5) |
| `source_freshness_hours{source}` | Over SLA (Section 6.2) |
| `jobs_queue_depth` and `jobs_oldest_age_seconds` | Depth over 10,000 or oldest over 2 h |
| `webhook_delivery_failures_total` | Over 10% of deliveries in 1 h |
| `usage_reconciliation_missing_total` | Over 100 per day |
| `stripe_webhook_lag_seconds` | Over 900 |
| `r2_write_errors_total`, `db_pool_wait_seconds` | Any error; wait p95 over 100 ms |

### 10.7 Watchdog checklist

Every 6 hours: freshness per source against SLA; open quality failures without an issue; API 5xx and p95; webhook failing subscriptions; Stripe webhook lag and failed invoices; correction requests approaching day 7; disk and queue depth; certificate expiry under 14 days; write the summary. Daily additionally: canary results, vocab unmapped totals, pending merge candidates over 20, cost report from Fly and Neon APIs against the budget line.

### 10.8 Monthly cost model

| Item | Low (M1 to M2, 15 sources, under 1K requests per day) | Mid (M3, 40 sources, 100K requests per day, 20 customers) | High (M4, 60 sources, 1M requests per day, 100 customers) |
|---|---|---|---|
| Fly.io api and worker | $15 | $60 | $250 |
| Neon Postgres | $0 to $19 | $69 | $300 |
| Cloudflare R2 (storage and class A ops) | $5 | $25 | $120 |
| Cloudflare Pages, DNS, WAF | $0 | $20 | $20 |
| Sentry, uptime, Plausible | $0 to $9 | $45 | $100 |
| Stripe fees | $0 | about 3% of revenue | about 3% of revenue |
| Playwright and egress | $5 | $20 | $60 |
| Total | $25 to $55 | about $240 plus fees | about $850 plus fees |

The low column sits inside design 01's $50 to $150 expectation.

## 11. Security and privacy

### 11.1 Secrets

Secrets live in Fly secrets (prod, staging), GitHub Actions secrets (CI) and a local `.env` protected by guard-paths and Gitleaks. Inventory: `DATABASE_URL`, `R2_ACCESS_KEY`, `R2_SECRET_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `SENTRY_DSN`, `API_KEY_HASH_PEPPER`, `WEBHOOK_SECRET_KEK`. Rotation: quarterly for R2 and Stripe keys by the Maintenance routine as a `needs-human` checklist item (the owner clicks), yearly for the pepper with dual-read; customer webhook secrets are encrypted at rest with the KEK and rotatable via PATCH.

### 11.2 Least privilege

Three Postgres roles: `records_migrate` (DDL), `records_worker` (read and write `core`, `staging`, `jobs`; read `api`), `records_api` (read `core`; read and write `api`, `ops`; no DDL). R2 tokens per bucket with write-only for raw ingestion and read-only for the API's signed URLs. Fly machines have no inbound ports except the api. Agents never hold prod secrets; CI uses staging.

### 11.3 Abuse controls

Per-key and per-IP rate limits (Section 7.8), Cloudflare WAF managed rules and bot fight mode on the site, request body limit 64 KB, search query length 200, a per-key daily hard cap on Free (1,000 requests) regardless of credits, enumeration detection (over 10,000 distinct license keys per key per hour on Free or Basic opens a review item). Terms forbid resale, redistribution, and use in consumer eligibility decisions (Section 11.7).

### 11.4 Data minimization by field

| Field | Rule |
|---|---|
| Person names | Only as published on the license (holder or qualifier); never parsed into given and family parts for search beyond `name_norm`; never joined across jurisdictions |
| Addresses | Business and premises roles stored; mailing addresses stored but never served on pages; any address flagged residential (USPS RDI not used; heuristic: matches the holder's name as an individual and no suite or business keyword) is served to the API only with scope `read:entities` on paid plans and never on pages |
| Phone, email | Stored only when published as a business contact; never on pages; served in the API only |
| Dates of birth, SSN fragments, driver license numbers, home addresses of individuals, demographic data | Never stored; the normalizer drops any column not in the canonical model and a Semgrep rule forbids these names in `fields:` mappings |
| Complaint narratives | Never stored; only case number, kind, dates and disposition |
| Lat and lon | Only when published by the source |

### 11.5 Logging

Request logs keep key id (not the key), path with query, status, latency, IP truncated to /24, for 30 days. No request bodies, no response bodies. Worker logs carry `source_slug`, `run_id`, `document_id`, never row contents beyond the 200-byte error excerpt. Sentry scrubs the `Authorization` header and all query parameters named `name`, `q`, `phone`.

### 11.6 Legal register

Table `legal_register` and a mirrored `legal/register/<LR-id>.yaml` in git:

| Field | Type |
|---|---|
| id | `LR-0001` |
| source_slug, agency, jurisdiction | text |
| record_class | text |
| access_method | enum `bulk_download, open_data_api, public_portal, public_records_request` |
| terms_url, terms_excerpt, terms_reviewed_on | text, text, date |
| robots_url, robots_status | text, enum `allows, disallows_partially, disallows, absent` |
| copyright_posture | enum `public_domain, edict, state_claims_copyright, unclear` |
| state_law_notes | text (for example Arizona A.R.S. 39-121.03 commercial-purpose statement) |
| personal_data_fields | text[] (which published fields identify a natural person) |
| restrictions | text (attribution, no bulk redistribution, and so on) |
| risk_rating | enum `low, medium, high` |
| status | enum `draft, approved, rejected, review_due` |
| approved_by, approved_on, review_due_on | text, date, date (12 months after approval) |

Rule: `Source.enabled` may only be true when the referenced entry has `status = approved`; `records validate-config` and a database check constraint both enforce it. `review_due` flips automatically at `review_due_on` and the watchdog opens a `needs-human` item; the source keeps running until the review is rejected.

### 11.7 FCRA scoping

Records are keyed and framed as business entities and licenses, not people; individual-holder records are API-only, flagged `is_individual`, and excluded from pages; no scores, grades, risk flags or recommendations are computed; customer terms define permissible use as business verification, compliance monitoring and commercial underwriting or credit, and prohibit consumer credit, employment, insurance or housing eligibility decisions about a natural person; `/v1/me` returns a `use_notice` and the docs state the product is not a consumer report. Counsel confirms before M3.

### 11.8 Arizona note

Arizona asserts copyright over state works and requires a statement of commercial purpose for public-records requests (A.R.S. 39-121.03), with a fee schedule for commercial use. No Arizona state or city source (including the Registrar of Contractors) is enabled until counsel reviews its register entry, which records whether access is via the public portal (facts republished with attribution) or a formal request with a commercial-purpose statement and fee. The owner is Arizona-based, so this sits on the pre-launch counsel list beside FCRA.

## 12. Testing map

| Oracle (design 01 Section 5) | Suite and tool | Gate |
|---|---|---|
| Schema validation on every record, required-field coverage metric | `tests/unit/test_models.py` (pytest, pydantic strict); coverage emitted by `quality` and asserted in `tests/quality/` | Fast |
| Recorded fixtures per source | `tests/adapters/test_replay.py` parametrized over `tests/fixtures/recorded/*`; human-only directory | Fast (changed adapters), nightly (all) |
| Property-based normalizers | `tests/property/` with Hypothesis: `normalize.name`, `address`, `date`, `license_number`, `vocab`; idempotence `f(f(x)) == f(x)`, round-trip for dates, case and whitespace invariance | Fast (200 examples), nightly (5,000 examples, database saved) |
| Differential parsers (top ten sources) | `tests/differential/` runs adapter A and B on the same fixture, diffs canonical output; zero tolerance on key fields | Nightly |
| Canary records | `records canary <slug>` after each run; `tests/quality/test_canary.py` on fixtures | Nightly and per run |
| Freshness and coverage as tests | `tests/watchdog/test_freshness.py` reads `coverage_daily`; fails on SLA breach | Watchdog |
| Event-derivation invariants | `tests/events/`: no event without two snapshots; status change implies event; derive twice count once; Hypothesis over random snapshot sequences; historical bands test on fixtures | Fast (unit), nightly (bands) |
| API contract | Schemathesis against staging from `openapi/v1.json`, all operations, 200 examples each; authorization invariants in `tests/api/test_tenancy.py` | Fast (unit), nightly (Schemathesis) |
| Load | k6 scripts in `loadtest/`, 50 rps lookup mix, p95 under 500 ms; required on PRs labeled `pricing` | Manual and before pricing changes |
| Mutation | mutmut on changed files at 50%; `normalize/` and `billing/` floors at 70% | Fast (changed files), nightly (full) |
| Pages and dashboard | Playwright plus axe-core on 20 sampled pages and the dashboard; visual snapshots human-only | Nightly and on `pages/` changes |

CI stages: `lint` (ruff, pyright strict, `records validate-config`, Semgrep, Gitleaks) 2 min; `unit` (pytest fast marker, Hypothesis small) 3 min; `mutation-changed` 5 min; `openapi-diff` 1 min; `e2e` (Compose Postgres, replay fixtures, API smoke) 5 min; `diff-size`. Nightly: full replay, Hypothesis large, differential, mutmut full, Schemathesis, Playwright plus axe, OSV-Scanner. The fast gate (hooks) runs lint and `pytest -m fast` under 60 seconds.

## 13. Milestone-to-issue breakdown

S is under one hour of agent work, M under three. Each becomes a `features.json` entry with `passes: false`; order is priority.

**M0 Platform skeleton**

1. Repo skeleton and tooling (S): `pyproject.toml` with ruff, pyright strict, pytest, Hypothesis, mutmut; `records` CLI entry point prints version; `make gate` runs lint and fast tests under 60 s on the empty project.
2. Constitution, CLAUDE.md, hooks, labels, issue template (S): guard-paths blocks `tests/**` in implement phase and the protected globs; gate-fast and gate-stop wired; labels and sizes created.
3. CI workflow (M): stages from Section 12 run on PR; diff-size gate at 600 lines; Semgrep, Gitleaks, OSV; a dummy PR passes.
4. Docker Compose dev environment (S): Postgres 16 and MinIO; `make db-up`; `records db ping` succeeds.
5. Alembic baseline and migration policy (S): empty baseline revision; `migration` label check in CI; docs/adr/0001-migrations.md.
6. Canonical pydantic models (M): all Section 2.1 entities as pydantic v2 models with enums; JSON Schema export to `schema/`; 100% field parity with Section 2.1 checked by a test that reads this design's tables.
7. Core schema migration (M): tables and indexes from Sections 2.1 and 4.3 excluding `api` and `ops`; partitioning for `event` and `license_snapshot`; migration under 60 s on empty database.
8. Vocabulary files and mapper (M): `vocab/status.yaml`, `vocab/event_kind.yaml`, `vocab/classification.yaml` skeletons; `normalize.vocab.map(source, field, value)` returns family plus native; unmapped returns `unknown` and increments a metric; Hypothesis test that mapping is total.
9. Normalizers: name, license number, date (M): pure functions with Hypothesis idempotence and invariance properties; 70% mutation score.
10. Normalizer: address (M): USPS-style abbreviations, unit parsing, `address_hash`, residential heuristic flag; properties as above.
11. SourceConfig model and validator (M): Section 3.2 schema; `records validate-config` fails on unknown keys, `respect_robots: false`, enabled without approved register; fixtures of valid and invalid configs.
12. Legal register model, YAML mirror and check constraint (S): Section 11.6 table; `records register lint`; a source cannot be enabled without approved entry (database test).
13. PolicyClient (M): robots cache, token buckets per source and host, UA and From headers, retries with backoff, conditional requests, budgets, politeness window; tests with a local httpx mock transport for each rule.
14. Semgrep rule: no raw httpx or Playwright in adapters (S): rule plus a failing fixture file proving it fires.
15. Fixture recording and replay (M): `fixture_mode` record and replay; manifest format; replay fails on unrecorded target; a sample synthetic source records and replays.
16. Adapter protocol and DeclarativeAdapter (M): Section 3.1 protocol; declarative csv and json parse via config `fields:`; synthetic fixture to canonical output end to end.
17. Postgres job queue and worker (M): `jobs` schema with `SELECT ... FOR UPDATE SKIP LOCKED`, retries, dead letter; worker loop; `records jobs enqueue|run`; test of concurrent workers with no double processing.
18. Scheduler (S): cron evaluation per enabled source; enqueues `crawl` jobs; respects politeness windows; unit tests on a fake clock.
19. Crawl run orchestration (M): `crawl_run` lifecycle (started, fetched, parsed, normalized, loaded, checked, published, failed); R2 raw write with Section 4.1 keys; staging load; canonical upsert; integration test on the synthetic source against Compose.
20. Error taxonomy (S): Section 3.7 exception classes with context fields; run status mapping; tests.
21. Synthetic sample jurisdiction `us-xx-sample` (M): generator for 2,000 licenses with controlled changes between two runs; used by tests and the test API key.
22. Metrics and Sentry wiring (S): Prometheus endpoint on api and worker; Sentry init with tags and scrubbing; test that `Authorization` never reaches the Sentry event.
23. Dashboard skeleton (S): static page listing sources, last run, status from `coverage_daily`; Playwright plus axe test passes.

**M1 Platform live with three sources**

24. License snapshot writer and hash (S): `canonical_hash` excludes `fetched_at` and provenance; unit tests.
25. Quality checks (M): all Section 6.1 rules with thresholds and overrides; `quality_check` rows; tests for each rule crossing its threshold.
26. Publication gating (M): pending to published pointer flip; paused state; issue creation through the GitHub API with the Section 6.5 title; test that a failed run leaves the previous state serving.
27. Canary runner (S): config canaries evaluated per run; failure marks the check; test on the synthetic source.
28. Coverage metrics job (S): `coverage_daily` computed nightly; tests on the synthetic source.
29. Watchdog command (M): Section 6.6 checks; idempotent issue creation (no duplicate issue for the same source and rule); summary file; tests with a fake GitHub client.
30. First bulk-file adapter (M): the Phase 0 pick (hypothesis: California CSLB); declarative config; recorded fixture; canaries; register entry drafted; passes fixture replay and quality checks.
31. First HTML portal adapter (M): the Phase 0 pick; seed enumeration strategy in config; selectolax parsing; recorded fixture of 50 detail pages; render_js false justified in the ADR.
32. First PDF adapter (M): an enforcement list; pdfplumber table strategy with text fallback; page-level fixtures; produces `EnforcementRecord` rows with native case ids.
33. Differential parser for the bulk source (M): second independently written parser (different builder session); diff harness; zero diff on key fields.
34. Vocab entries for the three sources (S): status, type and classification rows; unmapped count zero on fixtures.
35. Nightly builder dry run (S): a GitHub Actions workflow running `records crawl --replay` for the three sources nightly and publishing the dashboard; green for three consecutive nights.
36. Restore drill script (S): `records verify-restore`; documented runbook; drill passes against a Neon branch.
37. Fly and Neon staging deploy (M): Fly apps, Neon branch, secrets, `fly deploy` from CI on merge to main, smoke test; `records db ping` from the worker in staging.

**M2 Coverage and API v0**

38. Event derivation core (M): Section 5.1 algorithm and 5.2 rules; `event_key`; conflict-skip; tests: derive twice count once, no event without two snapshots, every status change has an event.
39. Event derivation: unknown handling, superseded, out of order (M): Sections 5.3, 5.5, 5.6; Hypothesis over shuffled snapshot sequences yields the same final event set.
40. Backfill and replay commands (S): `records events backfill`, `records parse replay`; tests on the synthetic source with a parser bump.
41. Historical bands check (S): event burst rule using medians; test.
42. FastAPI app skeleton with problem-details errors and request ids (S): `/v1/openapi.json`, `/v1/coverage`; Schemathesis smoke passes.
43. API keys, scopes, hashing (M): `api_key` tables, `rk_live` and `rk_test` formats, pepper, scope checks; tenancy invariant tests.
44. Licenses endpoints (M): get, history, list with filters and cursor pagination; tests for every filter; p95 under 100 ms on 100K synthetic rows.
45. Entities and qualifiers endpoints (M): trigram search, alias resolution; tests.
46. Search endpoint (S): combined lookup; tests.
47. Events polling endpoint (M): filters, ascending cursor, superseded exclusion; tests including the 100-events-per-credit metering.
48. Jurisdictions and sources endpoints (S): with coverage summary; zero credit; tests.
49. Rate limiting (M): sliding window per key and IP with headers; tests with a fake clock.
50. Usage metering pipeline (M): `usage_event` via queue, idempotency on request id, reconciliation job; tests: duplicate request ids counted once.
51. OpenAPI commit and diff check (S): `openapi/v1.json` committed; CI fails on drift; `x-may-grow` on enums.
52. SDK generation (M): TypeScript and Python clients generated in CI; a smoke test calls `/v1/coverage` through each.
53. Schemathesis nightly job (S): against staging; failures open issues.
54. Entity resolution proposer and review queue (M): Section 2.4 rules; `entity_merge_candidate`; `records merges list|accept|reject`; tests that individuals are never proposed.
55. Twelve additional adapters (M each, twelve issues 55a to 55l): one issue per Phase 0 source with register entry, fixture, canaries, vocab rows; each passes replay and quality checks and is within SLA for 14 days before the issue closes.
56. Freshness SLA enforcement in watchdog (S): class thresholds from Section 6.2; tests.
57. Internal dashboard v1 (M): sources, freshness, quality checks, event counts, merge queue size; Playwright plus axe.
58. API docs site (M): generated from OpenAPI with examples from Section 7; attribution and use notice; axe passes.
59. Parquet export job (M): Section 4.4 layout with DuckDB; `_schema.json`; test reads partitions back and matches Postgres counts.
60. k6 load test baseline (S): 50 rps mix on staging; results committed to `loadtest/results/`.
61. Webhook subscription model and signing (M): tables, HMAC, test key verification vector; delivery worker with the retry schedule and ordering; tests with a local receiver. (Shipped in M2 behind a feature flag so M3 only exposes it.)

Issue 55 expands to twelve entries in `features.json`, giving 72 entries in total; without the expansion the list is 61.

## 14. Open questions for the owner

| ID | Question | Recommended default |
|---|---|---|
| T-01 | Neon or Supabase? | Neon: branching makes the staging and restore-drill design cheap; nothing uses Supabase-only features |
| T-02 | Fly.io or Render? | Fly.io: `fly exec` for the watchdog, release commands for migrations, two regions on the free-ish tier |
| T-03 | Credit rule of 100 events per credit on the feed (versus one per event in design 01) | Keep 100:1; it makes the feed the cheapest way to consume and is the product insurers pay for; revisit at M3 pricing |
| T-04 | Entity pages for sole proprietors | No pages; API-only with `is_individual` flag until counsel says otherwise |
| T-05 | Should fuzzy merge proposals be auto-accepted above a very high score (0.98 plus same phone) | No until 200 human decisions exist to measure precision; then revisit |
| T-06 | Residential-address heuristic versus a paid USPS RDI lookup | Heuristic now; RDI at M4 if correction requests about addresses exceed 5 per month |
| T-07 | Warehouse shares: Snowflake first or BigQuery first | Snowflake, matching the enterprise buyers named (insurers, lenders); provision by hand for the first two customers |
| T-08 | Grafana Cloud free tier versus Fly metrics only | Fly metrics plus Sentry until M3; Grafana when alert routing is needed |
| T-09 | Domain and bot contact address for the User-Agent | Needed before the first live crawl (M1 issue 30); a neutral descriptive domain per Q-003 |
| T-10 | Who recorded fixtures are approved by | Owner merges `fixture-update` PRs; agents may record in staging sessions |
| T-11 | Arizona sources in M2's fifteen | Exclude Arizona from M2; include after counsel review (Section 11.8) |
| T-12 | Retain raw documents for 7 years or indefinitely | 7 years with object lock; the cost is small and auditability is a sales point |
