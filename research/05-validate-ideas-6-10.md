# Validation report: Ideas 6-10 (solo builder + AI coding agent, Arizona)

Research date: 2026-11-01. Sources: WebSearch plus direct fetches of vendor pricing pages, filings, blog posts and review sites. Two constraints: the shared search budget ran out mid-way (later work used direct fetches of known URLs), and G2, Capterra US, Software Advice, TrustRadius and Reddit blocked fetches, so incumbent complaints come from third-party digests or search summaries of those pages. Every number has a URL; estimates, competitor claims and inconsistent figures are labelled.

---

## Idea 6 - Paid MCP servers / vertical agent toolkits

### (a) Competitor and channel landscape

**First-party directories (the obvious distribution, but no payment rails).**
- Anthropic Connectors Directory: anyone on a paid Claude plan can submit a remote MCP server; listings are auto-published as "Community" after a policy scan, ranked by usage "similar to other app stores", and eligible for in-chat "Suggested Connectors" ([claude.com/docs/connectors/directory](https://claude.com/docs/connectors/directory), [submission guide](https://claude.com/docs/connectors/building/submission.md)). The Software Directory Terms contain no fee, revenue-share, or charging-users clauses at all; the only money language is a royalty-free license to Anthropic ([terms](https://support.claude.com/en/articles/13145338-anthropic-software-directory-terms)).
- OpenAI Apps SDK: the submission guidelines state "Currently, plugins may conduct commerce only for physical goods" and "Selling digital products or services-including subscriptions, digital content, tokens, or credits-is not allowed" ([developers.openai.com](https://developers.openai.com/apps-sdk/app-submission-guidelines)). A Jan 30, 2026 community reply confirms: "You cannot submit an app with monetization for any digital products/services" ([community thread](https://community.openai.com/t/chatgpt-app-monetization-apps-sdk/1372343)).

**Aggregators / tool platforms (they monetize the gateway, not your server).**
- Composio: Hobby $0 (100,000 tool calls/mo), Pro $29/mo with $0.0003 per extra call, Enterprise custom ([composio.dev/pricing](https://composio.dev/pricing)).
- Pipedream (Connect/MCP): Free dev tier; Startup $99/mo annual or $150/mo monthly with 10,000 credits, then $0.012/credit; 1 credit per 30 s of compute per MCP tool call ([pipedream.com/pricing](https://pipedream.com/pricing)).
- Zapier MCP: "included in every Zapier plan"; each tool call consumes two tasks from the normal task quota; no third-party seller program is mentioned ([zapier.com/mcp](https://zapier.com/mcp)).
- Smithery: pricing page returns 404 as of this check; third-party listings describe a free registry with paid hosting. Glama (Dec 2025): "We (Glama) are working on a revenue sharing model for open-source MCP server authors that will be announced soon" and labels mcp.run "(dead)" ([glama.ai state of MCP 2025](https://glama.ai/blog/2025-12-07-the-state-of-mcp-in-2025)); mcp.run now 301-redirects to turbomcp.ai, an enterprise MCP gateway ([turbomcp.ai](https://turbomcp.ai/)).

**The one marketplace that demonstrably pays developers: Apify.**
- Developer banner: "$1.7M paid out last month", "5,600" community developers, "Many developers earn over $3k", 106,352 tools in the Store ([apify.com/partners/actor-developers](https://apify.com/partners/actor-developers)). Apify keeps a 20% commission (plus platform compute costs on pay-per-result/event) and says "the most successful independent creators on Apify Store make over $10,000 monthly recurring revenue" and "Many others make more than $1,000 every month" ([help.apify.com](https://help.apify.com/en/articles/8684010-how-does-actor-monetization-work)). Earlier (Sep 2023) the top creator was at "over $4,000 MRR", so payouts have grown fast ([blog.apify.com](https://blog.apify.com/programmer-passive-income-web-scraping/)).
- Ground truth from the long tail: one developer's niche actor after three months had 64 lifetime users, 13 monthly actives and "roughly $200/month after Apify's 20% platform cut"; his $0.30 per-run start fee was "killing 58% of customers" ([dev.to](https://dev.to/boo_n/3-months-shipping-my-first-apify-actor-64-users-200mo-and-everything-i-got-wrong-52i7)).

**Payment rails that could change this:** Stripe now documents "Accept payments from agents" and "Billing for LLM tokens ... or API calls" plus a Stripe MCP server ([docs.stripe.com/agents](https://docs.stripe.com/agents)); x402 (HTTP 402 stablecoin payments) claims "millions of transactions" and shows Stripe, AWS, Cloudflare, Vercel logos but does not mention MCP ([x402.org](https://www.x402.org/)).

**M&A and funding (the money is in tooling and infra, not in individual servers):** Anthropic acquired Stainless (SDK/MCP-server generator) on May 19, 2026, price undisclosed by Techzine ([techzine.eu](https://www.techzine.eu/news/applications/141437/anthropic-acquires-stainless-to-better-integrate-agents-and-tools/)); a "$300M+" figure circulates only in secondary blogs ([dev.to](https://dev.to/gentic_news/anthropic-acquires-stainless-for-300m-owns-mcp-toolchain-28pg)) and is unconfirmed. Snyk bought Invariant Labs (MCP security scanning) June 24, 2025 ([snyk.io](https://snyk.io/news/snyk-acquires-invariant-labs-to-accelerate-agentic-ai-security-innovation)). Smaller deals: Keycard-Runebook, Nov 2025 ([salestechstar](https://salestechstar.com/?p=255811)); Usercentrics-MCP Manager, Jan 2026 ([techintelpro](https://techintelpro.com/AI/Enterprise-AI/usercentrics-acquires-mcp-manager-to-govern-ai-consent-data)); Sentry-XcodeBuildMCP, Feb 2026 ([pulse.bot](https://www.pulse.bot/ai/deals/sentry-acquires-xcodebuildmcp-c7e94024-ea14-42b8-b887-fc932ace49ae)). Glama counts "at least $73 million" raised by MCP-ecosystem companies in 2025.

### (b) Demand signals
- Ecosystem scale: 11,415 MCP servers registered in 2025; r/mcp 80k members and 7.3M visitors in 2025 (peak 1.19M in July); 31M weekly npm downloads of MCP packages; Google Trends interest "peaked around August and declined in early December" ([Glama](https://glama.ai/blog/2025-12-07-the-state-of-mcp-in-2025)).
- Monetization: the widely repeated "95% of MCP servers make zero revenue" stat traces to a promotional post with no data ([dev.to](https://dev.to/t49qnsx7qtkpanks/95-of-mcp-servers-make-zero-revenue-the-protocol-gap-thats-causing-it-7m2)). A second write-up (author sells an agent ad network, figures self-described as "directional estimates") puts servers earning >$1,000/mo at "<0.5%" and names one paid marketplace, Agent Bazaar, with an 18% fee ([dev.to](https://dev.to/nicolas_fainstein/the-state-of-mcp-monetization-in-2026-28i4)). Glama's founder notes that Bright Data, Exa and Tavily "are effectively charging for the MCP server usage based on the underlying API usage", i.e. the MCP server is a free wrapper around a paid API.
- Verdict: no evidence of anyone paying for an MCP server *as such*; people pay for the API/data behind it or for scrapers on Apify. Neither directory has a revenue share, and OpenAI forbids selling digital services inside ChatGPT.

### (c) What customers/builders complain about
- Builders: "the protocol has no payment primitive"; abandoned servers ("If we don't solve this problem, we will end up with a lot of abandoned MCP servers" - Glama). Half of 81 tracked MCP companies had dead domains or pivoted by Dec 2025.
- Marketplace sellers (Apify): "Silent success: runs 'succeeded' with 0 items"; dashboards track process success, not whether customers got data ([dev.to](https://dev.to/boo_n/3-months-shipping-my-first-apify-actor-64-users-200mo-and-everything-i-got-wrong-52i7)).

### (d) GTM a solo person could use
Free Anthropic directory listing (usage ranking plus Suggested Connectors is real distribution); the same logic published as paid Apify Actors (80% to you); Smithery/Glama for discovery; your own hosted endpoint gated by API keys with Stripe usage billing; vertical content in accountant/real-estate/legal communities.

### (e) Biggest risk
You do not own the data. A bookkeeping, MLS or court-docket MCP server is a thin wrapper over Intuit, MLS boards or PACER/UniCourt, who can ship their own connector (Stainless-style tooling makes it trivial). With no payment primitive in either directory, only proprietary data or a hosted workflow can capture value.

### (f) Scores
- AI-build fit: **8/10** - MCP servers are spec-driven, unit-testable code with an official conformance suite; an agent can grind through dozens of tools autonomously.
- Monetary value: **3/10** - the only proven payouts are for Apify scrapers (median unknown, long tail ~$200/mo), and both Anthropic and OpenAI directories offer distribution without any way to charge.

---

## Idea 7 - Niche structured-data API from continuous agent-driven scraping

### (a) Competitors, prices, URLs
| Vendor | What | Published price |
|---|---|---|
| Zoneomics | Zoning API | Starter $134/mo (10k tiles), Builder $284/mo, Growth $549/mo; "Data is for internal business and research use only... resale requires written authorization" ([pricing](https://www.zoneomics.com/pricing/zoning-data-apis)). Zoning-search seats $92/mo (25 searches) and $279/mo (150) ([pricing](https://www.zoneomics.com/pricing)). Logos: Stanford, Cushman & Wakefield, Kiavi, Redfin, Moody's. |
| Regrid | Parcels + standardized zoning (built with Zoneomics) | Self-serve "2,000 Parcel records per month plus additional per record pricing"; Enterprise above 10,000/mo; no dollar figures published ([plan options](https://support.regrid.com/reference/plan-options), [API blog](https://regrid.com/blog/introducing-regrids-updated-self-serve-api)). |
| Gridics | Zoning API | "Pay-Per-Use", Basic vs Detailed tiers, no figures ([developer.gridics.com](https://developer.gridics.com/plans)). |
| Shovels | Permits + contractors (AI-classified public records) | Free 500 credits; Basic $599/mo (25k credits); Pro $999/mo (50k); Enterprise custom; industries listed: Building Materials, ConTech, Energy & Climate, Home Services, Real Estate, Telecom, Research, Insurance ([shovels.ai/pricing](https://www.shovels.ai/pricing)). |
| Middesk | Business verification (KYB) | Quote-only; Vendr: "Median buyer pays" $14,375/yr, range $10,000-$206,196; startups <1,000 verifications/mo typically $10k-$30k/yr ([vendr.com](https://www.vendr.com/marketplace/middesk)). |
| Avalara | License Guidance / registration | License Guidance "as low as $119"; Sales Tax Registration $403 per location ([avalara.com](https://www.avalara.com/us/en/products/pricing.html)). |
| Mosey | Multi-state employer compliance | Essential $399/mo (up to 15 states), Standard $999/mo, Advanced $1,999/mo, plus matching implementation fees ([mosey.com/pricing](https://mosey.com/pricing)). |
| Harbor Compliance | License Manager | No public pricing found (pricing URLs 404). |
| PolicyMap | Mapping/data licensing | "Fill in the form above to receive a custom quote" ([policymap.com](https://www.policymap.com/pricing)). |
| Deckard Rentalscape / Granicus Host Compliance | STR compliance for governments | Priced "on the total number of short-term rentals" ([deckard.com](https://deckard.com/rentalscape-product-packages)); Berkeley contract with Granicus not-to-exceed $450,000 ([berkeleyca.gov](https://berkeleyca.gov/sites/default/files/2026-05/2026-05-19%20Item%2004%20Contract%20Granicus%20for%20Short-Term.pdf)); Deckard claims 400+ jurisdictions, 15M listings ([webull news](https://www.webull.com/news/14054596395369472)). |
| Docket Alarm / UniCourt / CourtListener | Court records | Docket Alarm Free; PAYG $39.99/mo + $4/doc; Flat-Fee $99/user/mo ([docketalarm.com](https://www.docketalarm.com/pricing)). UniCourt quote-based; Capterra lists $1,851/user/mo (unverified) ([capterra](https://www.capterra.com/p/232162/UniCourt/)). CourtListener/RECAP free API. |
| DSIRE (NC Clean Energy Tech Center) | Incentives | API "allows subscribers to access the full content of the database in real time"; no price published, contact required ([dsireusa.org](https://dsireusa.org/dsire-api/)). |

UnifyGov, Clearly, OpenGov and CivicPlus are government-side permitting/records software, not data APIs; Sacra lists OpenGov, Accela and Tyler as the "source-system vendors" Shovels scrapes around ([sacra.com](https://sacra.com/c/shovels/)). I could not independently verify UnifyGov or Clearly before the search budget ran out.

### (b) Demand signals
- The strongest proof point is Shovels: founder-disclosed ~$6K ARR in Aug 2023, Sacra-estimated $500K ARR end-2024, "$1M ARR" May 2025, "$3M ARR in August 2026", on $6.5M total funding (seed $5M, Base10, June 2025) ([sacra.com](https://sacra.com/c/shovels/), [Commercial Observer](https://commercialobserver.com/?p=523692)). Buyers: developers/data teams, lead-gen, and "Fortune 500 telecom, technology, and homebuilding firms"; target expansion into insurers, utilities, financial services.
- Zoneomics' logo wall (Kiavi - a lender; Redfin; Moody's) shows lenders/proptech/credit buy zoning data.
- Self-serve marketplace demand for raw government scrapers is thin: Apify's DSIRE crawler has "7 total users, 1 monthly" ([apify](https://apify.com/jungle_synthesizer/dsire-energy-incentives-crawler)); a permits + contractor-license actor has "2 total users" with only 5 cities parsed and 28 "pending" ([apify](https://apify.com/seibs.co/building-permits-intel)); a CourtListener-based court-records scraper has 80 users / 12 monthly at ~$1.20 per 1,000 records ([apify](https://apify.com/automation-lab/court-records-scraper)).
- Regulatory drivers are real: STR ordinances (400+ jurisdictions on one vendor), multi-state employer registration (Mosey at $399-1,999/mo), and sales-tax/licensing (Avalara).

### (c) Complaints about incumbents
Review quotes were not obtainable (sites blocked). Structural gripes in primary sources: Zoneomics forbids redistribution, Middesk and Harbor hide pricing, Regrid publishes quotas but no prices, DSIRE requires emailing for access, and Avalara moved License Guidance from $99 to $119 without saying whether it is one-time or recurring.

### (d) GTM for a solo builder
Shovels' playbook in miniature: free tier plus self-serve plans, SEO pages per jurisdiction (as Zoneomics does), data-marketplace listings (Shovels is on Cherre - [cherre.com](https://cherre.com/vendors/shovels/)), Snowflake/BigQuery shares for enterprise, and Apify Actors as a zero-sales channel for the scrapers.

### (e) Legality of scraping government sites and open-data licensing
- CFAA: *Van Buren* (2021) held that "exceeds authorized access" means reaching areas of a computer that are off-limits, not misusing data you may access ([Wikipedia](https://en.wikipedia.org/wiki/Van_Buren_v._United_States)); *hiQ v. LinkedIn* was vacated, re-affirmed on remand in 2022, then settled, so there is no final merits ruling, but public-data scraping is unlikely to be a CFAA violation ([Wikipedia](https://en.wikipedia.org/wiki/HiQ_Labs_v._LinkedIn)). Contract (ToS) and trespass claims remain live.
- Copyright: federal works are public domain and agencies are "encouraged to use CC0" ([resources.data.gov](https://resources.data.gov/open-licenses/)). *Georgia v. Public.Resource.Org* (2020) confirmed the government-edicts doctrine: statutes, codes and official annotations are not copyrightable. State/local *data* varies wildly: Florida records are public domain; California is contested; and **Arizona** state works "are not in the public domain and are protected by copyright" and "Commercial use of public records requires permission" ([Wikipedia summary](https://en.wikipedia.org/wiki/Copyright_status_of_works_by_subnational_governments_of_the_United_States)). For an Arizona-based builder scraping Arizona agencies that is a material legal footnote; zoning ordinances themselves are edicts (safe), but county GIS layers and compiled databases may not be.
- Practically: scrape primary portals, not vendors (Zoneomics' "internal use only" clause), keep provenance, respect robots/rate limits.

### (f) Scores (two business models)
- **Sell the API**: AI-build fit **9/10** (scraping, schema normalization, QA and re-crawl scheduling are exactly what a coding agent can run for months); value **5/10** (Shovels proves the model but needed VC, a team and three years to reach ~$3M ARR; marketplace demand for raw scrapers is tiny; buyers pay for coverage and accuracy SLAs, which are expensive to promise solo).
- **Use the data inside a SaaS** (e.g., STR-rule checker for hosts/investors, "what licenses do I need" wizard undercutting Avalara's $119, backflow/permit tracker for contractors): AI-build fit **7/10**; value **6/10** - the data becomes a moat behind a product people already pay for.

---

## Idea 8 - Field-service micro-SaaS for one trade

### (a) Incumbent pricing (primary pages unless noted)
- **Horizontal**: Jobber Core $29 (annual) to $49 (monthly) for 1 user, Connect $99-139, Grow $149-199, Plus $399-499 for 5 users, +$29/user ([getjobber.com](https://www.getjobber.com/pricing/)). Housecall Pro Basic $59/$79, Essentials $149/$189 (5 users), Max $299/$329 (8 users, +$35/user), cards from 2.59% ([housecallpro.com](https://www.housecallpro.com/pricing/)). ServiceTitan quote-only; third-party estimates $245-$498 per tech/mo plus $5,000-$50,000 implementation ([getonecrew.com](https://www.getonecrew.com/post/servicetitan-pricing)).
- **Pool**: Skimmer "Getting Started" $1 per serviced location/mo ($49 minimum, up to 49 locations) and "Scaling Up" $2 per location/mo ($98 minimum, 50-1,000 locations); add-ons AI Phone $99/mo, Marketing Suite $150/mo, Bookkeeping from $198/mo, CRM integrations $49/mo ([getskimmer.com/pricing](https://www.getskimmer.com/pricing)). Capterra 4.9/5 on 228 reviews ([capterra](https://capterra.com/p/177014/Skimmer/)). Challengers: PoolDial $2/pool, no minimums (founder-run) ([pooldial.com](https://www.pooldial.com/resources/articles/software-reviews/skimmer-alternatives)); Paythepoolman "$50 a month, plus $15 per technician", "no per-pool charge", "Built by a pool contractor", "Independent. Not venture-backed", 10+ years ([paythepoolman.com](https://www.paythepoolman.com/)); Pool Brain "$65 per tech" (base fee not rendered) ([poolbrain.com](https://poolbrain.com/pricing)); PSS and ProValet custom/per-pool.
- **Pest**: FieldRoutes (ServiceTitan-owned) "Growth" and "Corporate" packages, quote-only, "based on the number of active customers", no free trial ([fieldroutes.com](https://www.fieldroutes.com/pricing)); G2 lists "$350.00 per month per 1,000 active customers" ([g2](https://g2.com/products/fieldroutes-a-servicetitan-company/pricing)). PestPac (WorkWave) three quote-based tiers; G2 4.1/5 (126 reviews), Capterra 3.9/5 with ease-of-use 3.6 ([connecteam digest](https://connecteam.com/reviews/pestpac/), [capterra](https://www.capterra.in/software/23130/pestpac)). Briostack Capterra 4.0/5 (71 reviews), "$50/mo" starting price unconfirmed ([capterra](https://www.capterra.com/p/160267/Briostack/)). GorillaDesk Basic $49, Pro $99, Growth $149 per route (not per user), SMS $5/mo + credit packs; "7,200+" teams; founded 2013 by a field tech who took over the family business ([gorilladesk.com/pricing](https://gorilladesk.com/pricing), [about](https://gorilladesk.com/about-us/)).
- **Window cleaning / exterior**: The Customer Factor, launched 2004 by a window cleaner ([thecustomerfactor.com](https://www.thecustomerfactor.com/)); ResponsiBid quoting $199/mo flat ([capterra](https://www.capterra.com/p/175241/ResponsiBid/)); ServiceMonster Basic $79.99-99.99, Grow $149.99-199.99, Premier $229.99-279.99 for carpet/window/pressure washing ([servicemonster.com](https://www.servicemonster.com/pricing)).
- **Mobile detailing**: Urable Express $70, Pro $110, Enterprise $183/mo ([urable.com](https://urable.com/pricing)). Orbisx pricing page 404.
- **Septic**: ServiceCore quote-based "per truck"; 3.9/5 on 46 reviews; a 2026 roundup still lists it as the specialist ([rapidworks](https://www.rapidworks.com/blog/best-septic-liquid-waste-software-2026), [softwarereview](https://softwarereview.com/s/servicecore-servicecore)).
- **Chimney**: no US-specific tool surfaced; only UK "We Sweep" on Capterra ([capterra.co.uk](https://www.capterra.co.uk/software/1111184/We-Sweep-Chimney-Sweep-Management)).
- **Landscape / irrigation**: Service Autopilot $49/$199/$499 "+ sign up fee", annual billing ([serviceautopilot.com](https://www.serviceautopilot.com/pricing/)); LMN Starter $297/mo, Professional $648/mo plus onboarding ([granum.com/lmn](https://granum.com/lmn/pricing/)); SingleOps $220/$385/$550/mo plus $55-150 per office user ([granum.com/singleops](https://granum.com/singleops/pricing/)); Yardbook free-with-ads plus $34.99-49.99 tiers (sources disagree) ([capterra](https://www.capterra.com/p/207272/Yardbook/)). **No irrigation-repair-specific tool appeared in any result**; everyone sells general landscaping software.

### (b) Demand signals
- Pool: Skimmer's Scaling Up tier doubled from $1 to $2/pool, "a point of frustration for some existing customers", with one departing customer switching to PSS ([pooldial review, competitor-authored](https://www.pooldial.com/resources/articles/software-reviews/skimmer-review)). Skimmer bundling a $99/mo AI Phone add-on signals that AI receptionist features are now a priced line item in the trade.
- Pest: FieldRoutes reviewers "find it expensive, report hidden costs, and dislike limited pricing transparency" (GetApp digest) and "often feel unsupported, needing to troubleshoot problems themselves due to inconsistent or ineffective help from the support team" (Software Advice digest) ([softwareadvice](https://www.softwareadvice.com/field-service/fieldroutes-profile/), [getapp](https://www.getapp.com/industries-software/a/fieldroutes/)). PestPac's Capterra ease-of-use of 3.6 is the lowest among the set.
- Solo/small-team wins exist in every one of these niches: Paythepoolman (one contractor, 10+ years), GorillaDesk (field tech to 7,200 teams), The Customer Factor (window cleaner, since 2004), PoolDial (founder-led).

### (c) Complaints quoted
- PestPac (G2 user via Connecteam digest): "Every module is an upgrade with additional pricing involved." Another: "I dislike that PestPac logs me out constantly amidst workflows." Cons listed: "Steep learning curve", "Outdated interface in areas", "No built-in GPS or time tracking" ([connecteam.com](https://connecteam.com/reviews/pestpac/)).
- FieldRoutes (Capterra digest): "persistent technical problems and unreliable payment handling", interface "functional but inconsistent" ([capterra](https://capterra.com/p/146076/FieldRoutes)).
- Skimmer (Capterra digest): "some find the pricing steep"; one 5-star reviewer's only wish was "auto-billing for routine maintenance" ([capterra](https://capterra.com/p/177014/Skimmer/)). Competitor PoolDial asserts "Many users report stability problems with the Android app" (unverified, interested party).
- Reddit (r/poolpros, r/pestcontrol) could not be fetched; treat the absence of Reddit quotes as a gap, not as absence of complaints.

### (d) GTM for a solo builder
Undercut the incumbent's floor (PoolDial and Paythepoolman both attack Skimmer's $98 minimum); trade Facebook groups and podcasts (The Pool Guy Podcast covers Skimmer pricing - [buzzsprout](https://www.buzzsprout.com/110832/episodes/18789912-lose-a-few-earn-more-smart-pricing-strategy)); app stores (Skimmer has 769 Google Play reviews); distributor co-marketing; or sell an AI phone/dispatch add-on into Skimmer/GorillaDesk/Jobber shops instead of replacing them.

### (e) Biggest risk
Switching cost and saturation. Customer data, routes and card-on-file payments live in the incumbent, so you win only new companies or the enraged (Skimmer's price-hike cohort). Pool already has five cheap founder-built challengers against a 4.9-rated incumbent; pest has GorillaDesk at $49-149. The thin-tooling trades (septic, chimney, irrigation repair) are small with unproven willingness to pay (ServiceCore's 46 lifetime reviews suggest a tiny market).

### (f) Scores
- AI-build fit: **7/10** - scheduling, routing, invoicing and a customer portal are well-trodden CRUD, but a polished offline-capable mobile app, payments compliance and daily support are where solo builders bleed time.
- Monetary value: **6/10** - recurring SMB revenue with proven solo precedents, but the realistic ceiling is a GorillaDesk-sized niche, not a ServiceTitan. Best trade by evidence: **pest control small operators** (worst-rated incumbents, highest price floors) or an **irrigation-repair/backflow niche in arid states** (no dedicated tool found; WTP unverified).

---

## Idea 9 - Local-first, one-time-purchase desktop/mobile apps

### (a) Comparable products and prices
- Personal finance: Actual Budget is free/open-source ([actualbudget.org](https://actualbudget.org)); Buckets "costs $64. Once.", "no monthly fee", one license for the household, signed "Matt & Christy", "a keep-going, not a start-up" ([budgetwithbuckets.com](https://www.budgetwithbuckets.com/)); YNAB $109/yr or $14.99/mo, up from $84 to $99 (2021) to $109 (2024) ([costbench](https://costbench.com/software/personal-finance/ynab), [price history](https://pocketclear.app/blog/ynab-price-increases-history.html)).
- Journaling: Day One Basic free, Silver $49.99/yr, Gold $74.99/yr, owned by Automattic ([dayoneapp.com](https://dayoneapp.com/pricing)).
- Notes: Obsidian free, optional $50/user/yr commercial license, Sync $4-5/user/mo ([obsidian.md](https://obsidian.md/pricing)).
- Photo culling: Narrative Select $10-$60/mo (annual) with no one-time option ([narrative.so](https://narrative.so/pricing)); Aftershoot's pricing page did not render for fetch.
- Channels and fees: App Store Small Business Program 15% under $1M proceeds (10% for year-two subscriptions / EU alt terms) ([developer.apple.com](https://developer.apple.com/app-store/small-business-program/)); Setapp membership 70/30 with +20% to partners who bring users, single-app "15% on subscription sales and 25% on one-time sales", "around 30k of unique impressions just in the first days" for an average release ([setapp.com/developers](https://setapp.com/developers)); Paddle "5% + 50c" per checkout ([paddle.com](https://www.paddle.com/pricing)); Gumroad 10% + $0.50 direct, 30% via Discover ([gumroad.com](https://gumroad.com/pricing)).

### (b) Revenue evidence (post-2023 where available)
- Alin Panaitiu (Lunar, Clop, rcmd), 2023: Paddle sales $93.5k vs Mac App Store sales $14.9k; combined $108.4k sales, $84.8k proceeds, net profit $69,960 after Romanian tax ([notes.alinpanaitiu.com](https://notes.alinpanaitiu.com/Costs-of-running-a-macOS-app-studio-business)).
- Jeff Johnson (Lapcat), Aug 2026 retrospective: "Underpass was a flop, its lifetime revenue under $3000"; StopTheMadness "accounts for more than 93% of my lifetime App Store proceeds"; "At the time, I was about a month away from going broke"; "I'm basically a one-hit wonder" ([lapcatsoftware.com](https://lapcatsoftware.com/articles/2026/8/3.html)).
- Thomas Tempelmann (Dec 2024): "I sell about twice as much in MAS than over my website" and "us small devs usually only pay 15% now" ([mjtsai.com](https://mjtsai.com/blog/2024/12/09/selling-outside-of-the-mac-app-store/)).
- MacPaw developer survey (n about 400): multi-channel Mac devs earn 52% direct / 48% Mac App Store (direct was 44% in 2023). Disclosed indie figures compiled by the same report: Sensei ~$300K (solo, 2021), Screen Studio ~$30K/mo (2023), Xnapper ~$4-6K MRR sold for $150K (2024), RocketSim ~$4.5K MRR; "a typical popular utility" is $1-8K/month and "pure one-time licensing tends to cap out" ([state-of-mac-software report](https://state-of-mac-software.pages.dev/market.html)).
- RevenueCat 2025 (75k subscription apps): top 5% of new apps make over $8,880 after year one, bottom 25% no more than $19; "35%+" of apps now mix subscriptions with lifetime purchases or consumables ([revenuecat.com](https://www.revenuecat.com/state-of-subscription-apps-2025/), [MacRumors](https://www.macrumors.com/2024/03/13/most-subscription-apps-do-not-make-money/)).

### (c) Complaints
Subscription fatigue (YNAB's three price increases) is the pitch for one-time pricing, but developers' gripes point the other way: discovery ("Deluge of Fake Mac App Store Reviews" - Lapcat, Jul 2024 - [index](https://lapcatsoftware.com/articles/)), Apple bundle-payment errors (Lapcat, 2024), and one-time income that "makes it hard to fund server costs or ongoing development" ([setapp.com](https://setapp.com/app-reviews/app-monetisation-strategies)).

### (d) GTM
Setapp (30k launch impressions), Mac App Store features, Product Hunt / Show HN (local-first finance apps launch there constantly, e.g. FinBodhi - [HN](https://hn-qhrho4vm7-svelte.vercel.app/item/45800117)), bundle and deal threads ([MacPowerUsers](https://talk.macpowerusers.com/t/im-an-indie-mac-dev-bundled-all-27-of-my-apps-summer-deal/45735)), r/macapps, YouTube reviewers, and a Paddle storefront to keep 90%+.

### (e) Biggest risk
Distribution, not code: a power law where one hit funds everything, many launches earn under $3k lifetime, and the apps that scaled used recurring revenue. A local-first budget app also lacks the feature people pay YNAB for (bank sync), and Apple can sherlock journaling/notes at any WWDC.

### (f) Scores
- AI-build fit: **8/10** - self-contained, no backend, fully testable; an agent can ship cross-platform (Buckets ships Mac/Windows/Linux/mobile with two people). Deducted for App Store review and design taste.
- Monetary value: **4/10** - one-time pricing is viable as a hobby-plus income ($1-8K/mo is the documented norm for a popular utility) but caps out; Setapp's 70/30 usage-share and a lifetime+subscription hybrid are the realistic ways to raise the ceiling.

---

## Idea 10 - Open-source agent-testing devtool with a paid tier

### (a) Competitors, funding, pricing
| Company | Funding / status | Pricing |
|---|---|---|
| Braintrust | $5M seed (Dec 2023), $36M Series A led by a16z (Oct 2024), $80M Series B led by ICONIQ (Feb 17, 2026); customers Notion, Replit, Cloudflare, Ramp, Dropbox ([Series A](https://www.braintrust.dev/blog/announcing-series-a), [Series B](https://www.braintrust.dev/blog/announcing-series-b)) | Starter $0 (10k scores, 1 GB), Pro $249/mo, Enterprise custom ([pricing](https://www.braintrust.dev/pricing)) |
| LangSmith (LangChain) | - | Developer $0 (5k traces), Plus $39/seat/mo (10k traces), Enterprise custom ([langchain.com](https://www.langchain.com/pricing)) |
| Langfuse | Acquired by ClickHouse Jan 16, 2026 alongside ClickHouse's $400M Series D at $15B; MIT core stays self-hostable ([BusinessWire](https://www.businesswire.com/news/home/20260116008883/en), [Built In](https://builtin.com/articles/clickhouse-raises-400m-series-d-20260116)) | Hobby $0 (50k units), Core $29, Pro $199, Enterprise $2,499/mo ([langfuse.com](https://langfuse.com/pricing)) |
| Promptfoo | $18.4M Series A led by Insight (Jul 2025); **agreed to be acquired by OpenAI, Mar 9, 2026**; "More than 350k developers have used it", "130k are active each month", 23 people, founded 2024 by two co-founders ([blog](https://www.promptfoo.dev/blog/promptfoo-joining-openai/), [Built In](https://builtin.com/articles/promptfoo-raises-18m-series-a-20250730)) | Community free (10k red-team probes/mo), Enterprise and On-Prem custom ([pricing](https://www.promptfoo.dev/pricing/)) |
| Arize (Phoenix OSS) | $70M Series C Feb 20, 2025, ~$131M total ([arize.com](https://arize.com/blog/arize-ai-raises-70m-series-c-to-build-the-gold-standard-for-ai-evaluation-observability.md)) | AX Free (25k spans), AX Pro $50/mo, Enterprise custom ([pricing](https://arize.com/pricing)) |
| Helicone | Banner on pricing page: "Helicone is no longer part of Mintlify. Details to come soon." | Hobby free, Pro $79, Team $799/mo ([helicone.ai](https://www.helicone.ai/pricing)) |
| Laminar (YC S24) | - | Free $0 (1 GB), Starter $30, Pro $150/mo ([laminar.sh](https://laminar.sh/pricing)) |
| Humanloop | Raised $7.91M; told customers (Duolingo, Gusto, Vanta) in July 2025 it was shutting down; team joined Anthropic Aug 2025, which "did not acquire Humanloop's assets or its intellectual property" ([TechCrunch](https://techcrunch.com/2025/08/13/anthropic-nabs-humanloop-team-as-competition-for-enterprise-ai-talent-heats-up)) | retired |
| E2B | $21M Series A led by Insight (Jul 2025) ([Built In](https://builtin.com/articles/e2b-raises-21m-series-a-20250730)) | Hobby $0 (+$100 one-time credit), Pro $150/mo + usage, Enterprise $3,000/mo minimum; vCPU $0.000014/s ([e2b.dev](https://e2b.dev/pricing)) |
| Daytona | $24M Series A led by FirstMark (Feb 2026) ([lavx](https://news.lavx.hu/article/daytona-raises-24m-to-build-agent-native-infrastructure-for-large-scale-workloads)) | pay-as-you-go, $200 free, vCPU $0.0504/h ([daytona.io](https://www.daytona.io/pricing)) |
| Modal | $87M Series B led by Lux at $1.1B (Sep 2025) ([Built In](https://builtin.com/articles/modal-raises-87m-series-b-1b-valuation-20251001)) | Starter $0 + $30/mo credits, Team $250/mo + compute; sandboxes 3x list ([modal.com](https://modal.com/pricing)) |
| Antithesis (DST) | $105M Series A led by Jane Street (Dec 3, 2025); "increased revenue by over 12x over the last two years"; customers Jane Street, Ethereum, MongoDB ([PR Newswire](https://tools.prnewswire.com/en-us/live/20823/release/20251203EN37581)) | No free/OSS tier ("Sadly, no"), sold via AWS Marketplace private offers ([antithesis.com](https://antithesis.com/pricing/)) |
| Confident AI (DeepEval) | - | Free, Starter $200, Team $2,000/mo ([confident-ai.com](https://www.confident-ai.com/pricing)) |
| Lunary | small team, open source | Team $20/user/mo (50k events) ([lunary.ai](https://lunary.ai/pricing)) |
| Invariant Labs | acquired by Snyk Jun 24, 2025 (agent guardrails, MCP scanning) ([snyk.io](https://snyk.io/news/snyk-acquires-invariant-labs-to-accelerate-agentic-ai-security-innovation)) | - |
| MCPJam | Open Core Ventures Catalyst ($6k) Sep 2025; now claims "50,000+ developers", "200+ enterprises", customers Asana, Bright Data, IBM, Apollo ([OCV](https://www.opencoreventures.com/blog/postman-for-mcp-mcpjam-launches), [mcpjam.com](https://www.mcpjam.com/)) | OSS inspector/CLI free; paid client matrix, swarms, UAT, reporting; prices not published |

### (b) Demand signals
Four acquisitions in nine months (Invariant to Snyk, Humanloop to Anthropic, Langfuse to ClickHouse, Promptfoo to OpenAI) plus $100M+ rounds (Antithesis, Braintrust) show buyers exist, but every exit went to a platform, not to customers paying a solo vendor. Promptfoo's 350k developers prove appetite for OSS eval/red-team tooling. The official `modelcontextprotocol/conformance` repo (140 stars, GitHub Action, OAuth suites, tier assessment) now exists ([github](https://github.com/modelcontextprotocol/conformance)), alongside community tools mcp-probe, mcp-tester, mcp-halflist and mcp-conform ([mcp-probe](https://github.com/castrocrest/mcp-probe-cli), [mcp-halflist](https://pypi.org/project/mcp-halflist/0.2.0/)).

### (c) Complaints about incumbents
Usage-pricing opacity (LangSmith shows seat prices but no per-trace rate), Braintrust's "no free self-host path" (third-party comparison), ownership churn (Helicone's Mintlify banner), and vendor shutdowns stranding customers (Humanloop). An r/mcp thread on end-to-end MCP testing exists but Reddit blocked fetching.

### (d) GTM
GitHub/npm/PyPI, Show HN, GitHub Actions marketplace, MCP registry listing, OCV-style catalysts ($6k, as MCPJam took), content. Solo open-core precedents exist outside AI: ChartDB "over $9,400 in MRR in 16 months" ([stormy.ai](https://stormy.ai/blog/how-to-scale-developer-tool-9k-mrr)); Sidekiq reportedly "closer to $10M than $1M" (secondary, unverified) ([saasblog](https://saasblog.pages.dev/posts/2025/12/saas-businesses-built-by-solo-founders)); SSOReady ~$660K with six people ([Latka](https://getlatka.com/companies/ssoready.com)). Promptfoo was not solo (two founders, 23 staff).

### (e) Gap check and biggest risk
- MCP conformance testing: **taken** (official suite + MCPJam with 200 enterprises).
- Agent permission/policy testing: **taken** (Invariant inside Snyk; Promptfoo red-teaming inside OpenAI).
- Cost regression testing in CI: no dedicated vendor surfaced, but LangSmith, Langfuse, Helicone and Braintrust all report cost per trace; a feature, not a product (absence not exhaustively verified).
- Deterministic replay of agent runs: Antithesis (not agent-specific, no free tier) and Laminar/LangSmith traces; credible but narrow.
Biggest risk: competing with free, VC-subsidized, platform-owned OSS (OpenAI, ClickHouse, Snyk and Anthropic each own one); a solo paid tier must beat "free and bundled."

### (f) Scores
- AI-build fit: **9/10** - pure code, dogfoodable, agents can write their own test suites.
- Monetary value: **3/10** - crowded, consolidating, and every incumbent has a free tier; the solo open-core wins on record are in unglamorous infra (Sidekiq, SSOReady), not LLM evals.

---

## Wildcard (evidence-backed)

**"Shovels for X" via API + Apify, where X is a public-records vertical nobody has normalized.** Shovels went from founder-disclosed ~$6K ARR (Aug 2023) to an estimated $3M ARR (Aug 2026) by AI-classifying permits into structured records sold at $599/$999/mo plus enterprise table shares, naming Insurance, Home Services, Telecom and Energy as buyers ([sacra.com](https://sacra.com/c/shovels/), [shovels.ai/pricing](https://www.shovels.ai/pricing)). Apify pays "$1.7M" a month to 5,600 developers at an 80% share ([apify](https://apify.com/partners/actor-developers)), yet the only permits+contractor-license actor on the Store covers five cities with 28 jurisdictions "pending" ([apify](https://apify.com/seibs.co/building-permits-intel)). Candidate X with a regulatory driver and no vendor found: contractor-license status, disciplinary actions and bond/insurance lapses across the 50 state boards, sold to insurers, lenders and marketplaces. Caveats: Arizona's copyright posture on public records, and thin self-serve demand on Apify (7 and 2 users for existing gov-data actors), so the API/warehouse channel is where the money would be.

**Secondary wildcard:** an AI phone/dispatch add-on sold *into* Skimmer/GorillaDesk/Jobber shops. Skimmer prices AI Phone at $99/mo and Housecall Pro lists "CSR AI" and "Voice" add-ons ([getskimmer.com](https://www.getskimmer.com/pricing), [housecallpro.com](https://www.housecallpro.com/pricing/)): proof of willingness to pay for a capability a solo LLM-native builder can ship without fighting switching costs. Risk: incumbents bundle it themselves.

---

## Comparison table

| Idea | Top competitors | Typical price | Demand signal strength | Biggest risk | AI-build fit | Value |
|---|---|---|---|---|---|---|
| 6. Paid MCP servers / vertical toolkits | Composio, Pipedream, Zapier MCP, Apify Store, Bright Data/Exa/Tavily (API-backed), first-party connectors | Composio $0-29/mo; Pipedream $99-150/mo; Apify actors $20-50/mo rentals or ~$1-5 per 1k results; directories $0 | Weak for servers, strong for ecosystem (11k servers, 7.3M Reddit visits) | No payment primitive in Claude/ChatGPT directories; data owners ship their own connector | 8 | 3 |
| 7. Niche public-data API | Shovels, Zoneomics, Regrid, Gridics, Middesk, Avalara, Mosey, Deckard/Granicus, Docket Alarm/UniCourt | $134-549/mo (Zoneomics API), $599-999/mo (Shovels), $10k-206k/yr (Middesk), $399-1,999/mo (Mosey) | Moderate-strong (Shovels ~$3M ARR est.; lenders/insurers on logo walls); weak on marketplaces | Coverage/accuracy treadmill across 1,800+ jurisdictions; Arizona copyright on public records | 9 (API) / 7 (SaaS) | 5 (API) / 6 (SaaS) |
| 8. One-trade field-service SaaS | Skimmer, PoolDial, Paythepoolman, FieldRoutes, PestPac, GorillaDesk, Jobber, Housecall Pro, ServiceTitan, Urable, ServiceMonster | $1-2/pool/mo; $49-149/route; $29-329/mo horizontals; $350/mo per 1k customers (FieldRoutes) | Moderate (Skimmer 100% price hike; PestPac 3.6 ease-of-use; FieldRoutes "hidden costs") | Switching costs plus saturation in pool; tiny TAM in septic/chimney | 7 | 6 |
| 9. Local-first one-time apps | YNAB, Actual, Buckets, Day One, Obsidian, Narrative, Setapp catalog | $64 once (Buckets); $109/yr (YNAB); $49.99-74.99/yr (Day One); Setapp 70/30 | Moderate (subscription fatigue) but power-law outcomes (<$3k lifetime to $300k) | Discovery; one-hit-wonder economics; one-time cap | 8 | 4 |
| 10. OSS agent-testing devtool | Braintrust, LangSmith, Langfuse (ClickHouse), Promptfoo (OpenAI), Arize, Helicone, Laminar, E2B, Daytona, Modal, Antithesis, MCPJam | Free tiers everywhere; $29-249/mo self-serve; $2,000-2,499/mo team; enterprise custom | Strong investor demand ($105M, $80M rounds; 4 acquisitions) but all free-to-start | Competing with free, VC-subsidized, platform-owned tools | 9 | 3 |
| Wildcard: Shovels-style public-records vertical (e.g., contractor licensing) | Shovels, BuildZoom, Cotality, Construction Monitor; Apify actors | $599-999/mo self-serve; enterprise table shares | Moderate (Shovels trajectory; buyers named) | Same treadmill as Idea 7; AZ copyright | 9 | 6 |

### Gaps in this research (be aware)
Harbor Compliance and UniCourt pricing unpublished/blocked; Reddit unreachable; Aftershoot pricing not rendered; Stainless deal value unconfirmed; Smithery pricing 404; irrigation-trade willingness to pay unverified.
