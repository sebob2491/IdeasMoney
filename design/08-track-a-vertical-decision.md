# Track A vertical decision brief

Status: decision brief for Q-002, 2026-11-01. Source: `research/phase0-scorecard.md` (53-row source table, vendor and buyer tables, six alternatives, legal pass) and `research/phase0-legal-register-draft.yaml` (64 verified source entries). This page is what you need to decide; the scorecard is the evidence.

## 1. What Phase 0 found

- **The verification niche filled in during 2026.** TradesAPI sells contractor-license verification for 50 states plus DC with daily re-check webhooks at $99 per month for 50 monitored licenses and $1 per extra license (pricing and status feed verified). Mesh sells the same to Angi, Houzz, Amazon and LendKey on an enterprise, sales-led basis. At least six smaller entrants sell per-lookup verification at $0.12 to $0.25. Kill criterion 3 in the design ("a vendor already sells a 50-state API with events under $500 a month") is triggered on its literal terms.
- **The dataset niche is still empty.** Nobody sells the normalized record set: bond and insurance carriers and expirations, qualifying individuals, discipline as coded events, cross-state entity resolution, bulk Parquet and warehouse shares, change events derived from official files. TradesAPI's status field is "the board's own wording" and 7 of its 51 state scrapers were broken on the day of checking. Shovels has license dates but no status, bond, insurance or discipline.
- **The sources favor a bulk-first builder.** 19 jurisdictions publish free bulk files or open datasets (Washington alone publishes five linked datasets three times a day with bond, insurance, principals and signers). The search forms are hostile (23 gated by CAPTCHA or bot management from a datacenter address) and the files are not. Only 8 states are gated with no bulk or list channel, and 5 of those have no state general-contractor license to begin with.
- **Buyers.** Three categories show documented spend: home-services marketplaces (Google Local Services Ads mandates state-level license checks; Houzz and Yelp sell verified badges; Angi and Houzz are Mesh customers), point-of-sale home-improvement lenders (LendKey via Mesh), and property-management vendor credentialing (NetVendor). Insurers, the headline buyer in design 01, show no documented spend anywhere; that assumption has to go until a conversation proves otherwise.
- **Legal.** No blocker found; three items for counsel before billing: FCRA scoping for sole-proprietor licensees sold to lenders, Arizona's commercial-purpose statute and computer-tampering statute as applied to the Registrar of Contractors, and DMCA 1201 for any CAPTCHA-gated portal after Reddit v. SerpApi (policy: never solve a CAPTCHA; every gated state but New Mexico has an ungated file or list channel). Also a correction: the September 2026 Reddit v. Anthropic ruling was a state-court demurrer after remand, not a federal ruling; the research file has been annotated.

## 2. Scores

| Vertical | Score of 100 | One-line read |
|---|---|---|
| Liquor licenses | 68 | Most uniformly open sources, widest incumbent gap, but buyers are sales teams buying lead lists, not compliance teams buying monitoring |
| Contractor licensing (repositioned) | 67 | Three buyer categories already pay for monitoring; the architecture produces the asset the incumbents lack; best adjacency (permits, code enforcement, business licenses all name contractors) |
| City business licenses | 64 | Good daily city feeds; Enigma sells the same signal at $0.05 per lookup |
| Permits in the uncovered tail | 59 | Head-on with Shovels and BuildZoom in the hardest sources |
| Code enforcement | 58 | Excellent in seven big cities, absent elsewhere; payers are cheap investor tools |
| STR registrations | 56 | A government-software market, not a data market |
| Food inspections | 49 | Already solved by Hazel Analytics for Yelp |

## 3. The options

**Option A (recommended): contractor licensing, repositioned, then liquor licenses on the same framework.** The product is the normalized national record set with bond, insurance, qualifiers, discipline, entity resolution and change events from official files, delivered as API, webhooks, bulk and warehouse shares. Verification lookups are a feature, not the product. Liquor follows in M3 or M4 because its adapters are nearly identical in shape and it diversifies buyers toward distributors and KYB vendors.

**Option B: liquor licenses first.** Highest raw score and the cleanest sources. Weaker because the buyers with evident spend are prospecting teams, and because it forfeits the adjacency that lets later record sets share one entity graph.

**Option C: stop Track A.** Defensible if you do not want to compete on data depth against two monitoring vendors. The scorecard's own words: if you are not willing to compete on depth and bulk delivery rather than per-lookup verification, pick liquor or stop.

## 4. What changes in the Track A design if you pick Option A

- **Positioning and headline buyers.** Marketplaces, point-of-sale lenders and credentialing vendors first; insurers removed from the target list until one pays.
- **Pricing hypothesis (replaces design 01 Section 3.3).** Free: 500 credits and 25 monitored licenses. Starter $149: 10,000 credits, 500 monitored, polling feed. Growth $499: 50,000 credits, 5,000 monitored, webhooks, bond and insurance fields, daily Parquet for subscribed states. Scale $999: 200,000 credits, 25,000 monitored, national bulk. Enterprise from $15,000 a year with warehouse share and SLA. The structural bet: monitoring costs this product almost nothing because events come from diffing official files, so it can price monitoring at $0.04 to $0.10 per license per month against TradesAPI's $1 live re-check while charging more for depth.
- **First ten sources, all bulk or open data, no search forms.** Washington L&I, Oregon CCB, Florida DBPR, Texas TDLR, Virginia DPOR, Connecticut, Colorado, California CSLB lists, New York City (DOB and DCWP), Illinois IDFPR. Together about 2.4 million credential rows. Gated states come later by records request; HTML-only boards last.
- **Month-one tests of the three riskiest assumptions.** (1) Ten buyer conversations with a free 60-day change feed for four states; pass if two sign a pilot at a stated price and one names bond or insurance events as the reason. (2) Thirty days of daily diffs on four states, 50 derived events hand-checked against the live portals; pass under 2% false events and a median lag under 48 hours. (3) Three records requests (Arizona, Georgia, Hawaii) and the FCRA and Arizona memos from counsel; pass if files arrive machine-readable within 30 days for under $500 and the memo supports the entity-keyed, business-purpose structure.
- **Arizona specifically.** The Registrar of Contractors blocks datacenter traffic and the statute attaches a commercial-purpose statement and fee to its posting list. Arizona is a records-request source, not a scrape, and waits for counsel.

## 5. What this does not change

The platform architecture in design 01 and the technical design in design 07 are vertical-agnostic and were built for exactly this: bulk first, Playwright last, a legal register before every source, events derived by diffing snapshots, a normalized vocabulary with native values preserved. The scorecard confirms the architecture is the differentiator.

## 6. The decision

Answer Q-002 in `DECISIONS.md` with A, B or C. The default if you want to defer is A, because it is the only option that both passes the buyer test and compounds into adjacent record sets. Nothing is built until Gate G0 regardless.
