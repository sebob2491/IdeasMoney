# Validation research: Ideas 1–5 (skeptical pass)

*Prepared 2026-11-01. Method: ~50 web searches plus direct fetches of vendor pricing pages, statutes, Steam sales trackers, review sites and forum snapshots. Figures are quoted from the cited page; third-party estimates are labelled as such. Gaps are stated, not papered over. Reddit could not be fetched directly (blocked) and the search budget ran out before every niche query was run; where that matters it is flagged.*

---

## Idea 1 — Self-managed small-HOA software (US, Arizona-heavy)

### (a) Competitor landscape

| Vendor | Published price | Notes / source |
|---|---|---|
| PayHOA | $49/mo (1–25 units, billed yearly; $54 monthly), $59/$65 (26–50), $249/$275 (401–500); >500 units $0.55/unit/mo; management companies $275 minimum. ACH $2.45 flat per payment, cards 3.5% + $0.50 | All features in every tier; Stripe Treasury underneath. [payhoa.com/pricing](https://payhoa.com/pricing). Capterra 4.7 from 574 reviews. [Capterra](https://www.capterra.com/p/146693/PayHOA/) |
| HOA Start | Starts $39/mo; a third-party tracker lists Professional $39 / Premium $49 / White-Glove $149 (unverified) | Capterra 4.9 from 88 reviews. [Capterra pricing](https://www.capterra.com/p/215897/HOA-Start/pricing/), [PricingSaaS](https://pricingsaas.com/companies/hoastart) |
| Condo Control | Custom quote; G2 lists a $49 edition | [Capterra](https://www.capterra.com/p/143800/Condo-Control/pricing/), [G2](https://g2.com/products/condo-control/pricing) |
| Buildium | Essential $65/mo, Growth $202, Premium $420; incoming EFT $2.35/txn on Essential; cards 2.99%; "pricing for community associations please call" | [buildium.com/pricing](https://www.buildium.com/pricing/) |
| AppFolio | Core $1.49/unit/mo, 50-unit minimum, "minimum spend" applies; third parties put the floor at ~$280–298/mo | [appfolio.com/pricing](https://www.appfolio.com/pricing), [ManageCasa](https://managecasa.com/articles/managecasa-vs--appfolio-which-property-management-software-is-right-for-you) |
| HOALife | From $199/mo, quoted after demo; targets 100–5,000 homes; violations + ARC focus | [hoalife.com/pricing](https://hoalife.com/pricing) |
| TownSq | Unpublished; third party lists "Pro (up to 300 units) $90/mo" and a free communication tier | [Toolradar](https://toolradar.com/tools/townsq/pricing) |
| Neighborhood.online | Free (ad-supported) marketing site; the HOA management suite is "still under development" | [neighborhood.online/pricing](https://neighborhood.online/pricing) |
| Frontsteps, Vinteum, Zego | No public pricing found (Vinteum pricing URL 404s) | — |
| Long tail | Easy HOA ($299 setup + $39.99/mo), Effortless HOA ($3/home/mo), MoneyMinder, BoardStack, ManageCasa, QuickBooks | [GetApp](https://www.getapp.sg/software/2057878/easy-hoa), [Fitgap](https://us.fitgap.com/products/048701/payhoa) |

Takeaway: the exact product already exists at $49/mo with 574 public reviews (PayHOA); entry pricing has converged at $39–$59/mo.

### (b) Demand signals

- **Market size (primary source).** The Foundation for Community Association Research's 2025 Statistical Review: 373,000 associations, 29.6M housing units, 78.1M residents; 30–40% of associations are self-managed ("do not employ a professional manager or management company") → roughly 112,000–149,000 self-managed boards; $124.2B in assessments collected; 2,555,000 board and committee members; 9,000–10,000 management companies. [FCAR 2025 PDF](https://foundation.caionline.org/wp-content/uploads/2026/03/2025StatisticalReviewFoundation.pdf)
- **Arizona.** Same table: 10,300 associations, 911,000 units, 2,276,000 residents (11th state by count, behind CA, FL, TX, IL, NC, NY, CO, MA, GA, WA). CAI's older Arizona fact sheet (2022 data): 9,900 associations paying "$4.7 billion a year," with "75,300 Arizonans serve as volunteer leaders." [CAI AZ fact sheet](https://www.caionline.org/getmedia/227d5a83-cf6e-4214-8aa8-094bc37b499d/statefactsfiguresarizona.pdf). Applying the national 30–40% self-managed share gives ~3,100–4,100 self-managed Arizona boards (an inference, not a published figure).
- **Regulatory churn is real and annual.** 2025 session: SB1494 moved the planned-community foreclosure threshold from "12 months or $1,200" to "18 months or $10,000, whichever occurs first"; SB1039 requires boards that record open meetings to keep recordings six months and provide unedited copies; SB1378 adds political flags; all effective Sept 26, 2025. 2024: HB2662 (48-hour agendas, A.R.S. §§ 33-1248/33-1804) and HB2648 (lien definitions, §§ 33-1256/33-1807). [Attorney at Law Magazine, Sept 11, 2025](https://attorneyatlawmagazine.com/public-articles/hoa-law/what-arizona-hoa-boards-need-to-know-as-the-law-continues-to-evolve). 2026 session: CHDB Law lists seven HOA bills "passed into law" (SB1290 executive sessions, HB2397 resale disclosure, HB4011 board duty, SB1246 foreclosure, SB1184/SB1808 flags, HB2342 shade structures). [CHDB legislative updates](https://www.chdblaw.com/legislative-updates/). A compliance calendar therefore has a genuine maintenance story, which also means a maintenance burden.
- **Search volume / Reddit:** not obtained (no keyword tool; Reddit blocked). Review counts are the proxy: PayHOA's 574 Capterra reviews vs HOA Start's 88 implies a few thousand paying communities for the leader, not tens of thousands.

### (c) What customers complain about

- PayHOA, Software Finder reviews: "the only downside is the limited accounting capabilities" (Carrie, Sept 2023); "my customer service requests have been unanswered for over a month with minimal response" (James, Aug 2023); "I cannot save the filter options, which becomes a significant time-waster" (Matthew, Aug 2023). 100 reviews, 4.6 overall. [Software Finder](https://softwarefinder.com/property-management-software/payhoa/reviews)
- PayHOA, GetApp cons summary: "Cumbersome transaction handling," "Unreliable bank connectivity." [GetApp](https://www.getapp.com/real-estate-property-software/a/payhoa/reviews/)
- PayHOA, PissedConsumer (May 20, 2025): "Your website offers no way for me to pay you for fees my bank mistakenly took that were meant for you. Please call!" [PissedConsumer](https://www.pissedconsumer.com/payhoa/RT-F.html)
- PayHOA per a competitor's write-up (treat accordingly): fees "can be higher than expected or increase over time," BBB complaints about "unclear or escalating transaction fees," "mobile functionality is more limited than desktop," and "No reserve fund management module." [ManageCasa, Aug 2026](https://managecasa.com/articles/payhoa-reviews-features-pros-cons)
- HOA Start, Capterra: "there is a bug in the late fee process"; "household data import/upload has been a little complex." [Capterra](https://www.capterra.com/p/215897/HOA-Start/reviews/)
- Positive signal for the segment: an Easy HOA reviewer says it "allows our small HOA to be run in a professional manner, even though our board has limited experience with accounting." [Capterra](https://capterra.com/p/237509/Easy-HOA/reviews/)

Recurring gaps: reserve-fund tracking, fee transparency, accounting depth, support responsiveness — not core dues collection.

### (d) Go-to-market a solo founder could use

- Arizona statute-change SEO ("SB1494 foreclosure threshold") with a free compliance calendar as lead magnet — law firms own this traffic today.
- CAI Central Arizona events and HOA law firms as referrers; direct mail to registered agents of small associations; r/HOA, Nextdoor, CPAs who do HOA books.

### (e) Biggest risk

Low ACV against an entrenched leader selling the same thing at $49/mo. At $600–$1,500/yr per community you need several hundred boards for a solo income, each won through a slow volunteer sales cycle with annual board turnover; the money-movement layer (ACH, KYC, trust accounting) is the part an agent can't carry liability for.

### (f) Scores

- **AI-build fit: 8/10.** Deterministic CRUD + ledger + documents + calendar on well-trodden stacks; payments are the only genuinely hard integration.
- **Value / sellability: 5/10.** Sticky, cash-flowing, and acquirable as a micro-SaaS, but the ceiling for a solo entrant behind PayHOA is modest and every dollar is sold one board at a time.

---

## Idea 2 — Short-term-rental compliance tool for hosts and small PMs

### (a) Competitor landscape

| Vendor | Price | Notes / source |
|---|---|---|
| Avalara MyLodgeTax | $27/mo per property (1–5 properties); Pro (6+) by quote; a third-party review adds a $299 one-time setup fee per property | "approximately 30 days to obtain your licenses"; 11,000+ US jurisdictions; does not handle marketplace-collected taxes. [Avalara Vrbo page](https://www.avalara.com/mylodgetax/en/resources/vrbo.html), [STR Specialist review, Feb 2026](https://strspecialist.com/reviews/avalara-mylodgetax-tax-compliance-review), [StrHub](https://strhub.com/product/avalara-mylodgetax-2/) |
| HostReady (hostready.ai) | Not published | "street-level, daily-updated compliance monitoring," "more than 850 U.S. markets," renewal reminders, an "Autopilot" service that files and renews licenses; integrations with Hospitable (June 12, 2026), Lodgify, Hostfully, Track. [Hospitable help](https://help.hospitable.com/en/articles/15484308-connect-hospitable-with-hostready-to-automate-short-term-rental-compliance), [Hostfully](https://www.hostfully.com/integrations/hostready/) |
| Hospitable | Bundled with PMS | Launched lodging-tax automation covering "state, county and municipal tax requirements." [ShortTermRentalz](https://shorttermrentalz.com/news/hospitable-automates-lodging-tax-compliance/) |
| Hostaway / Guesty / Lodgify / Hostfully | PMS pricing | No native compliance engine found; Hostaway's glossary says it helps "follow permit details and renewal dates"; others rely on HostReady. [Hostaway](https://www.hostaway.com/glossary/str-permit/) |
| PermitAndGo | From $19 | Hosts with 1–10 properties. [Capterra](https://www.capterra.in/software/1097629/PermitAndGo) |
| StayRule | n/a | NYC, LA, SF, Texas deadline tracking. [Product Hunt](https://www.producthunt.com/products/stayrule-2) |
| Government side: Granicus Host Compliance | Sold to cities | Acquired Nov 12, 2019; "more than 300 local governments" (Berkeley, Hollywood FL, Las Vegas, Denver, LA, Seattle). [GovTech](https://www.govtech.com/biz/Granicus-Buys-Short-Term-Rental-Compliance-Software-Company.html) |
| Government side: Deckard Rentalscape | Priced on STR count | "Trusted by 400+ jurisdictions," scans "over 15 million listings across 10,000+ rental websites daily." [Deckard](https://info.deckard.com/short-term-rental-software-government), [packages](https://deckard.com/rentalscape-product-packages) |

Note: a host-side, PMS-integrated version of exactly this idea (HostReady) shipped in 2026 inside Hospitable, Lodgify and Hostfully.

### (b) Demand signals

- **Enforcement is the demand engine.** 400+ (Deckard) and 300+ (Granicus) jurisdictions pay for listing surveillance; nine Texas cities and one county use Deckard, and a Fort Worth host group sued the city for paying Deckard to "surveil citizens." [CBS Texas](https://www.cbsnews.com/texas/news/how-are-cities-monitoring-short-term-rentals-many-are-turning-to-tech/). Every new enforcement contract creates hosts who suddenly need to be compliant.
- **Arizona has a statutory template that is genuinely fiddly.** A.R.S. § 9-500.39 (SB1168, 2022): cities may require a permit with a fee "not to exceed… $250"; owners must notify properties "adjacent to, directly and diagonally across the street" before first rental and file an attestation; keep an emergency contact able to respond "in a timely manner in person"; carry liability insurance "in the aggregate of at least $500,000" or use a marketplace that does; show the permit or TPT number on every ad; three verified violations in 12 months allow a suspension of up to 12 months; fines escalate $500 → $1,000 → $3,500 (or one/two/three nights' rent). [Laws 2022, Ch. 343](https://www.azleg.gov/legtext/55leg/2R/laws/0343.htm). ADOR adds that operators "may not offer for rent… without first obtaining a current TPT license" even when Airbnb/Vrbo remit, and must still file a return showing marketplace receipts with deduction code 775. [ADOR](https://azdor.gov/business/transaction-privilege-tax/online-lodging-marketplace). Sedona's permit is reported at ~$210, Gila County $250/unit, Scottsdale requires a city license under Ordinance 4566. [Lodgify Sedona guide](https://www.lodgify.com/guides/sedona-short-term-rental-regulations/), [Safely](https://safely.com/articles/what-to-know-about-arizona-short-term-rental-laws/), [Steadily Scottsdale](https://www.steadily.com/blog/airbnb-short-term-rental-laws-regulations-scottsdale)
- **Rules change constantly, locally.** 2025 alone: Houston adopted an ordinance 4/16/2025 (effective 1/1/2026), Bellaire TX on 11/17/2025, Brighton UT capped STRs at 15% of dwellings, Twin Cities suburbs are being sued over bans, North Carolina has a preemption bill. [Houston](https://houstontx.gov/ara/str.html), [Bellaire](https://bellairetx.gov/1988/Short-Term-Rentals), [Star Tribune](https://www.startribune.com/twin-cities-airbnb-bans-lawsuit/601456823), [UNC LRS](https://lrs.sog.unc.edu/node/558084). No national ordinance count surfaced; that Deckard and HostReady both advertise *daily* monitoring is itself evidence of the change rate.
- **Host pain in their own words.** BiggerPockets (2020): "the tax compliance is complex in my area and they make it easy" (re MyLodgeTax); a Pigeon Forge PM called mixed platform remittance "a mess" and said Avalara's ~$20/mo "is worth it"; the skeptic: "One more vendor to give your hard-earned money to!" [BiggerPockets](https://www.biggerpockets.com/forums/530/topics/850072-str-compliance-companies-popping-up). Airbnb's forum: a Sequim, WA host couldn't register because "the city will not issue one to us either because we are not in the city"; a Massachusetts host was asked to file for months Airbnb had already collected. [Airbnb Community](https://community.withairbnb.com/t5/Help-with-your-business/Help-I-can-t-get-AirBnB-to-understand-that-we-are-not-in-the/m-p/1999057), [MA thread](https://community.withairbnb.com/t5/Help/MA-Occupancy-Tax/m-p/1160559/highlight/true). Airbnb itself called San Francisco's process a "registration nightmare." [PR Daily](https://www.prdaily.com/airbnb-fights-back-after-san-francisco-registration-proposal/). r/Louisville (late 2025): "There's no way you'll have it approved in 2-3 weeks for derby." [snapshot](https://reddit.sentinel-team.org/posts/1smfte9/snapshots/2026-04-17T23%3A48%3A15.182923Z)

### (c) Incumbent complaints

- MyLodgeTax: "$299 setup fee per property adds up for large portfolios"; "Pricing beyond the Starter tier isn't fully transparent"; doesn't handle "marketplace collections (Airbnb/Vrbo)"; needs "clean PMS data." [STR Specialist](https://strspecialist.com/reviews/avalara-mylodgetax-tax-compliance-review). Avalara company-wide Trustpilot sits at 3.2 from 477 reviews, including a one-star about Avalara admitting a mistake "but declined to pay for the correction." [Trustpilot](https://dk.trustpilot.com/review/avalara.com)
- The gap: permit/renewal/neighbor-notice workflow per city with evidence packs — which HostReady now claims.

### (d) Go-to-market

- Arizona-first permit guides (Scottsdale/Sedona/Phoenix/Flagstaff) at $10–20/property/month; one statutory rules model covers every Arizona city with local parameters.
- PMS app marketplaces (Hospitable, OwnerRez, Hostaway) — where HostReady already sits; BiggerPockets, host Facebook groups, CPAs, white-label to 10–100-door PMs.

### (e) Biggest risk

The product is a legal-data maintenance treadmill with liability attached. Rules change city by city, weekly; one wrong renewal date or missed neighbor-notice attestation costs a host a $1,000–$3,500 fine, and filing taxes on a client's behalf is a fiduciary act. Incumbents with teams (Avalara, HostReady inside Hospitable) already do daily monitoring; a solo founder either narrows to Arizona (small TAM: a few thousand paying hosts at best) or inherits a nationwide research burden an agent cannot be trusted to carry unsupervised.

### (f) Scores

- **AI-build fit: 6/10.** The software is simple; the asset is continuously verified legal data, which needs human review per jurisdiction and carries error liability.
- **Value / sellability: 5/10.** Clear willingness to pay (~$27/property/mo is established), acquirers exist (Avalara, PMS vendors), but per-property pricing, host churn and 2026 bundling by PMSs cap the upside.

---

## Idea 3 — WCAG 2.1 AA scanning/monitoring for small public entities and SMBs

### (a) Competitor landscape and pricing

| Vendor | Published price | Source |
|---|---|---|
| accessiBe accessWidget (overlay) | Micro $59/mo ($490/yr, ≤5k visits), Growth $179 ($1,490), Scale $479 ($3,990), Enterprise custom; "Custom legal terms" $20k+ | [accessibe.com/pricing](https://accessibe.com/pricing). FTC final order April 22, 2025: $1M, barred from claiming its product "can make any website WCAG-compliant," 20-year reporting. [FTC](https://ftc.gov/news-events/news/press-releases/2025/04/ftc-approves-final-order-requiring-accessibe-pay-1-million) |
| UserWay (overlay; owned by Level Access) | Widget $490/yr (≤100k pageviews), $1,490/yr (≤1M), Pro Plus $1,190, Ultimate $2,490; monitoring add-on $990/yr (100 pages) to $10,990/yr (1,500 pages) | [userway.org/pricing](https://userway.org/pricing/) |
| AudioEye | Three tiers (Basic automated "address about 50% of issues," Self-Managed, Managed with "Assurance" legal protection); no prices on page; aggregators list from $49/mo | [audioeye.com/pricing](https://www.audioeye.com/pricing/), [GetApp](https://www.getapp.com/it-management-software/a/audioeye/pricing/) |
| Siteimprove / Monsido (Acquia Optimize) / Silktide | Quote only; a competitor estimates $500+/mo, $300+/mo and $400–600+/mo respectively | [A11yProof](https://a11yproof.pages.dev/compare/pricing), [AccessibilityChecker](https://www.accessibilitychecker.org/blog/siteimprove-alternatives/) |
| Level Access | Custom quotes after discovery call | [TestParty](https://testparty.ai/blog/level-access-pricing-2026) |
| Pope Tech (WAVE engine) | Free (25 pages), Team $25/mo annual, Business Plus $225/mo, Professional $400/mo | [pope.tech/pricing](https://pope.tech/pricing). Miami University replaced Level Access AMP with it in July 2026 because "the interface is easier to use." [Miami OH](https://miamioh.edu/it-services/news-media/2026/07/introducing-pope-tech-new-digital-accessibility-platform.html) |
| Equalize Digital Accessibility Checker (WordPress) | Free; Pro $190/yr; Small Business $750/yr (5 sites); Agency $2,250/yr (25 sites) | [equalizedigital.com](https://equalizedigital.com/accessibility-checker/pricing/) |
| axe DevTools/Deque, Allyant | Developer tooling / services; no list prices found | — |
| CMS vendors bundling accessibility | K-12 sysadmins pick Apptegy, Finalsite, Edlio, ParentSquare, SchoolStatus because "Site is wcag/ada compliant"; Edlio sells an "optional AI add-on that makes fixes automatically" | [r/k12sysadmin snapshot](https://reddit.sentinel-team.org/posts/1qi7jej/snapshots/2026-01-21T03%3A32%3A02.164998Z) |

### (b) Demand signals

- **The regulatory driver moved.** DOJ's interim final rule (issued April 17, published in the Federal Register April 20, 2026, doc. 2026-07663) pushed Title II deadlines to **April 26, 2027** (population ≥50,000) and **April 26, 2028** (<50,000 and special districts); WCAG 2.1 AA unchanged; comments to June 22, 2026; one source reports a court challenge, and DOJ "is considering… a separate, new rulemaking" for K-12. [League of MN Cities](https://www.lmc.org/news-publications/news/all/doj-extends-web-accessibility-compliance-deadlines-for-local-governments/), [Duane Morris](https://www.duanemorris.com/alerts/doj_extends_ada_title_ii_digital_accessibility_deadlines_one_year_0426.html), [EDUCAUSE](https://er.educause.edu/articles/2026/6/doj-and-hhs-extend-web-accessibility-deadlines-to-2027-2028). The target segment (small entities) now has 18 months of slack, and its lobby (LMC, NLC) is asking for "tailored exemptions for very small jurisdictions."
- **Private-sector litigation is the live driver.** UsableNet 2025: 5,114 suits (3,195 federal, 1,919 NY/CA state); 64% of defendants under $25M revenue; 70% eCommerce; **1,416 suits against sites running accessibility widgets**; New York ~2,742, Florida ~977. [UsableNet 2025 report](https://info.usablenet.com/2025-year-end-digital-accessibility-lawsuit-report-plain-text). Seyfarth counts 3,117 federal cases, +27% YoY. [ADA Compliance Pros](https://www.adacompliancepros.com/blog/who-gets-sued-web-accessibility-filing-data)
- **Are small governments buying, and from whom?** Evidence says: mostly through their CMS vendor, and mostly with labor rather than new software. EdTech Magazine (June 2026) describes small districts "fold[ing] accessibility into existing duties rather than hire," with a Nebraska director citing the challenge of "prioritizing the extensive requirements." [EdTech](https://edtechmagazine.com/k12/article/2026/06/ada-title-ii-web-accessibility-compliance-guide-small-k-12-districts-perfcon). The large-ticket precedent is litigation-driven: Seattle Public Schools' ~$287,000 AudioEye contract (2016) under a consent decree. [SPS board packet](https://www.seattleschools.org/wp-content/uploads/2021/07/I06_20161207_Action_Report_AudioEye_Contract_Packet.pdf)
- **Procurement reality (Arizona).** Formal competition starts at $100,000 (A.R.S. § 41-2535(A)); Mohave County needs one quote ≤$10,000 and three written quotes to $100,000; Mesa buys under $10,000 on a P-card, $10,000–$50,000 with three informal quotes, and uses State of Arizona, Omnia, Sourcewell and SAVE cooperatives; sole-source awards cannot be piggybacked. [Mohave](https://www.mohave.gov/departments/procurement/documents/procurement-operating-procedures-small-purchases/), [Mesa](https://www.mesaaz.gov/Business-Development/Procurement-Services/How-to-Do-Business-with-Mesa), [AZ SPO](https://spo.tscti.com/sites/default/files/2025-05/procurement-regulations_0.pdf). A $1–9k/yr scanner is a P-card purchase — easy to sell, easy to displace.

### (c) Incumbent complaints

- Overlay Fact Sheet (1,048 signatories): "any site which has deployed an overlay in the past year and a half has been less useable for both my wife and me" (blind user); "As a screen reader user, numerous sites have become less usable for me with this overlay"; a WebAIM survey found 67% of practitioners rate overlays "not at all or not very effective." [overlayfactsheet.com](https://overlayfactsheet.com/en/)
- Lawsuits despite overlays: *Murphy v. accessiBe* alleged the overlay "makes it impossible for some screen reader users to access" the site; Tribeca Skin Care was sued while running accessiBe. [Level Access](https://levelaccess.com/blog/overlay-fails-in-lawsuit-test-website-accessibility-settlement-speaks-volumes), [WebAbility](https://www.webability.io/blog/accessibility-overlay-lawsuit). The National Federation of the Blind said these tools "frequently introduced new barriers." [A11y Collective](https://a11y-collective.com/blog/accessibility-overlays/)
- Governance suites: buyers "who only need accessibility may pay for unused features" (Siteimprove/Monsido). [AccessibilityChecker](https://www.accessibilitychecker.org/blog/siteimprove-alternatives/)

### (d) Go-to-market

- Free public scan reports for every Arizona town, county and district mailed to the clerk/IT lead, priced under the $10k P-card line; Arizona League of Cities and Towns, AASBO, ADA-coordinator webinars; resell through gov web agencies (CivicPlus/Granicus/Revize shops) and WordPress agencies for the SMB demand-letter market.
- Never say "compliant" or "protection" — the FTC order shows what gets punished.

### (e) Biggest risk

Scanning is commoditized (WAVE, axe, Lighthouse, Pope Tech free tier) and the target's deadline just slipped to 2028 with exemptions on the table. The remaining value — PDF remediation, training, manual audits — is services; a scanner alone sells at $25–$225/mo to buyers whose CMS vendor already includes a checker.

### (f) Scores

- **AI-build fit: 7/10.** Crawl + axe-core + reporting + remediation guidance is straightforward; the hard parts are PDF/document remediation and avoiding claims the FTC has already policed.
- **Value / sellability: 6/10.** A real, dated regulatory driver, tens of thousands of entities, and purchases under procurement thresholds — but a crowded, commoditized field where the segment buys from incumbents it already pays.

---

## Idea 4 — Shopify apps in under-served niches

### (a) Store economics (confirmed)

- Revenue share: 0% on the first $1M of **lifetime** app revenue counted from Jan 1, 2025, then 15%; the annual reset was sunset effective June 16, 2025; "Earnings are aggregated at the partner level." Standard rate without registration is 20%; billing carries a 2.9% processing fee. [Shopify changelog, Apr 24, 2025](https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share), [Revenue share docs](https://shopify.dev/apps/store/revenue-share)
- Steam-style fee: none, but a Built for Shopify badge took one developer "about six months of free use and roughly 1,000 free users" to earn. [Barn2](https://barn2.com/?p=712947)

### (b) Competition and saturation proxies

- Store size: 21,509 live apps (AppstorePulse, May 2026), 22,546 (June 2026); apps per 1,000 shops rose from 5.5 (2023) to 8.8 (2026). Share of new apps with **zero reviews**: 52% (2024) → 68% (2025) → 77% (2026). [Taylor Sicard](https://taylorsicard.com/blog/shopify-app-store-statistics-2026), [RevenueHunt](https://revenuehunt.com/state-of-the-shopify-app-economy/), [Indie Hackers 21,509-app analysis](https://www.indiehackers.com/post/i-analyzed-21-509-shopify-apps-to-see-if-the-market-is-saturated-5428b05aa9)
- Revenue distribution (Barn2, Oct 19, 2026, 1,675 apps, "possibly off by a factor of two"): top 16 apps take 40.6% of revenue; the bottom half take 0.6% combined; **median app $2,500–$5,200/mo gross**; oldest fifth of apps ~$19,100/mo vs youngest fifth ~$190/mo; star rating "showed no correlation with success." [Barn2](https://barn2.com/?p=712947)
- Solo outcomes (self-reported): Wide Bundle "nearly 3,000 paying customers at $15/month" [first-mrr](https://first-mrr.com/tech/Shopify%20App%20API); a $100k+ MRR portfolio [Indie Hackers](https://www.indiehackers.com/post/qdReVAgLjz6EpW4OrJSI); $25k MRR in under a year (2021) [Indie Hackers](https://www.indiehackers.com/post/i-bootstrapped-a-shopify-app-to-25k-mrr-in-less-than-a-year-ama-89f3d4c471); $78k CAD MRR after 3 years [Indie Hackers](https://www.indiehackers.com/post/i-bootstrapped-a-shopify-app-to-78k-cad-mrr-in-3-years-ask-me-anything-5fbbc2b886). The long tail: an acquire.com listing for a portfolio at "~$500 MRR" [acquire.com](https://app.acquire.com/startup/wavy6rcufu-profitable-shopify-app-portfolio-with-500-mrr-and-low-support-overhead), and app-analytics vendor Mantle winding down [Shopify dev community](https://community.shopify.dev/t/mantle-shutdown/37796).

Category check (review counts as competition proxy, fetched Nov 2026):

| Niche | Incumbent | Rating / reviews | Price | Native Shopify move |
|---|---|---|---|---|
| B2B wholesale | B2B Wholesale Hub (ex-Wholesale Club, 2015, BFS badge) | 4.7 / 703 | $39–$99/mo | B2B features opened to non-Plus plans (reported April 2, 2026); community confirms "B2B available on the Basic plan" (May 1, 2026); native B2B checkout reported GA June 3, 2026 (single source). Wholesale Gorilla (~5,000 installs) "loses its core pricing and terms functionality." [Wholesale Hub](https://apps.shopify.com/wholesale-club), [Community](https://community.shopify.com/t/b2b-available-on-the-basic-plan/615519), [Planetsoft](https://theplanetsoft.com/shopify-b2b-features-non-plus-plans/), [Pulse](https://www.pulse.bot/ecommerce/news/shopify-launches-native-b2b-checkout-threatening-wholesale-apps-a6517d7e-e36e-4f88-bb04-8bdac4799e91/) |
| B2B wholesale | SparkLayer (2021) | 4.9 / 389 | Free–$299/mo | same |
| Pre-orders | PreOrder Now WOD (2017, BFS) | 4.5 / 1,027 | $19.95–$59.95/mo | none native |
| Local pickup/delivery | Zapiet Pickup + Delivery | 4.9 / ~1,895 (#533 overall) | n/a | Shopify has native local delivery/pickup settings; routing is third-party |
| Delivery routing | EasyRoutes (2020, BFS) | 4.9 / 289 | Free–$59/mo (+$1,125 enterprise) | none native |
| Bundles | Shopify Bundles (Shopify, July 2023) | **2.8 / 571** | Free | Native, but "fixed bundles and multipacks, but not mix-and-match"; reviewers hit variant limits and lost imagery |
| Sales-tax nexus helper | Shopify Tax | — | Free to $100k sales/yr then 0.35% (cap $0.99/order, $5,000/yr; new stores after May 13, 2026 get a lifetime $100k threshold with no annual cap listed) | **Native "Tax liability insights"** flags states at 80% of economic-nexus thresholds; native automated filing exists. [Pricing](https://help.shopify.com/en/manual/taxes/shopify-tax/pricing), [Liability insights](https://help.shopify.com/en/manual/taxes/us/us-tax-liability), [Automated filing](https://help.shopify.com/en/manual/taxes/shopify-tax/automated-filing) |

Sources for app listings: [SparkLayer](https://apps.shopify.com/sparklayer), [PreOrder Now](https://apps.shopify.com/preorder-now), [Zapiet via AppNavigator](https://appnavigator.io/app/click-and-collect/), [EasyRoutes](https://apps.shopify.com/easyroutes), [Shopify Bundles](https://apps.shopify.com/shopify-bundles), [Bundles changelog](https://changelog.shopify.com/posts/shopify-now-offers-native-support-for-product-bundles).

### (c) Where it is still open

- **Dead:** a US/Arizona sales-tax nexus helper (native since Shopify Tax); basic wholesale pricing/net terms (native 2026); fixed bundles (native 2023).
- **Open-ish:** mix-and-match/configurator bundles on the native component-inventory model (the free Shopify app's 2.8 stars are 571 merchants listing what's missing); "advanced B2B" (quoting, sales-rep workflows, EDI, net-terms dunning) — the pivot incumbents are being pushed into; multi-driver routing with proof-of-delivery (EasyRoutes has 289 reviews after six years: small market or under-served); pre-orders with partial payments. Discounts and bundles are the most crowded shelves (~1,100 and 472 apps).

### (d) Go-to-market

App Store search is the channel and it is pay-to-play on reviews: launch free, farm ~1,000 installs to reach the Built for Shopify badge, then price. Supplement with Shopify Community answers, vertical Partner agencies, YouTube tutorials; acquiring a stale app with 100+ reviews is a legitimate shortcut (Barn2's advice).

### (e) Biggest risk

Platform absorption plus discovery. Shopify has now nativized tax, bundles and B2B within three years; 77% of new apps get zero reviews; the median survivor grosses $2.5–5k/mo before support costs. A solo builder can ship fast, but the queue of identical apps means the AI advantage is small exactly where it matters (reviews, ranking, support).

### (f) Scores

- **AI-build fit: 8/10.** Mature APIs, Remix/Polaris templates, clear review guidelines; an agent can build and iterate several apps.
- **Value / sellability: 6/10.** Documented exits and $20k+ MRR solo outcomes exist and the 0%-to-$1M rev share is generous, but the distribution is a steep power law and the two niches you named with the biggest TAM (tax, B2B) were just closed by Shopify.

---

## Idea 5 — Original-IP idle/incremental game on Steam ($5–15)

### (a) Comparable sales and revenue (Steam only, third-party estimates)

Raijin.gg estimates as of Nov 1, 2026 (medium confidence unless noted); Gamalytic was rate-limited during this run. Gamalytic itself reports estimates "within 30% of true sales 77% of the time" and excludes bundles and off-Steam revenue. [Gamalytic about](https://gamalytic.com/about), [SteamPageAnalyzer](https://www.steampageanalyzer.com/blog/gamalytic-vs-vg-insights)

| Game | Price | Est. copies | Est. gross | Reviews | Source |
|---|---|---|---|---|---|
| Melvor Idle (Nov 2021, published by Jagex; also web + mobile, not counted) | $9.99 | 426.4K | $3.4M | 16,362 | [Raijin](https://raijin.gg/app/1267910/Melvor_Idle) |
| Rusty's Retirement (Apr 2024, solo dev) | $6.99 | 318.6K | $1.8M | 14,926 | [Raijin](https://raijin.gg/app/2666510/Rustys_Retirement); dev announced 200,000 copies in two weeks and 100,000 in five days [GamesRadar](https://www.gamesradar.com/games/management/amid-rave-reviews-and-200000-sales-idle-stardew-valley-style-farming-sim-dev-can-hardly-believe-their-success-im-only-now-realizing-how-life-changing-this-is); another tracker estimates ~1M copies/$4.4M [GameRevenueData](https://gamerevenuedata.pages.dev/games/rustys-retirement/) — the trackers disagree by 3×, which tells you how soft these numbers are |
| Nodebuster (Aug 2024) | $2.99 | 338.5K | $860K | 13.7K | [Raijin](https://raijin.gg/app/3107330/Nodebuster) |
| To The Core | — | 114.6K | $824K (high confidence) | — | [Raijin](https://raijin.gg/app/1988550/To_The_Core) |
| Magic Research 2 (2024) | $5.99 | 17.6K | $89.7K (Jul 2026) | 610 | [Raijin](https://raijin.gg/app/2864890/Magic_Research_2) |
| Increlution (EA Oct 2021) | $3.99 | 28.3K | $90.3K | 1,260 | [Raijin](https://raijin.gg/app/1593350/Increlution) |
| Magic Research (Mar 2023) | $4.99 | 9.5K | $37.7K | 420 | [Raijin](https://raijin.gg/app/2311680/Magic_Research) |
| Leaf Blower Revolution (Dec 2020) | Free + IAP/DLC | n/a | not estimated | 26,519 (94%) | [Raijin](https://raijin.gg/app/1468260/Leaf_Blower_Revolution_Idle_Game) |
| Idle Slayer (Dec 2020) | Free + IAP | n/a | not estimated | 10,989 (84%) | [Raijin](https://raijin.gg/app/1353300/Idle_Slayer) |
| Bitburner (Dec 2021) | Free, open source | n/a | $0 | 7,447 (95%) | [Raijin](https://raijin.gg/app/1812820/Bitburner) |

Reading it honestly: the deep-systems games most like the proposal (Magic Research 1 & 2, Increlution — well-reviewed, systems-rich) grossed $38k–$90k each on Steam; the breakouts (Melvor with a publisher, Rusty's and Nodebuster with a viral visual hook) are exceptions. After Steam's 30% cut (25% above $10M, 20% above $50M), refunds, regional pricing, discounts and VAT, net is roughly half of gross. [Impress.games method](https://impress.games/blog/calculate-estimate-steam-game-sales-revenue-profit). The Steam Direct fee is $100 per app, recoupable after $1,000 adjusted gross revenue. [Steamworks](https://partner.steamgames.com/doc/gettingstarted/appfee)

### (b) Demand and market structure

- **Base rates are brutal.** Over 5,000 games released in 2025 did not recoup the $100 fee; of ~41,000 games released over three years, 50% earned $500 or less. [WN Hub](https://wnhub.io/news/stores-and-publishing/item-42136), [Yahoo](https://tech.yahoo.com/gaming/articles/over-5-000-games-released-152604830.html)
- **Genre flooding.** PC Gamer: "54 idle games released on Steam last week, and many look eerily similar." [PC Gamer](https://pcgamer.com/gaming-industry/steam-week-in-review-54-idle-games-released-on-steam-last-week-and-many-look-eerily-similar). A later week: 300+ releases, 120 with AI disclosures. [Pulse](https://www.pulse.bot/gaming/news/steam-week-in-review-more-than-300-games-released-on-steam-last-week-and-120-of-them-had-ai-disclosu-709aa200-d1db-4cdf-bf51-2be084a14169/)
- **Wishlist math.** Median week-1 sales = 0.15× launch wishlists for games with >25k wishlists, 0.10× for games priced above $10 (GameDiscoverCo, Oct 17, 2025). A $9.99 game needs ~20k wishlists to sell ~2–3k copies in week one. [GameDiscoverCo](https://newsletter.gamediscover.co/p/the-state-of-steam-wishlist-conversions)
- **What the audience wants (Steam review synthesis).** Praise clusters on prestige loops that "feel well integrated and give satisfying progression"; complaints on "perceived lack of content, short length, and shallow progression, especially in the mid-to-late game," "prestige upgrades are useless and do not carry over," and games that are "not actually idle." [Vaporlens examples](https://vaporlens.app/app/4102010/idle_chapel), [Farmer Against Potatoes Idle](https://vaporlens.app/app/1535560/farmer_against_potatoes_idle)
- **The AI-content problem is specific to this plan.** Steam AI disclosures rose ~800% in 2025 [ScreenHub](https://www.screenhub.com.au/news/games/steam-generative-ai-games-2673234/); one study of ~10,000 games found disclosed titles earned fewer reviews [Digital Citizen](https://www.digitalcitizen.life/steam-games-that-disclose-ai-use-may-sell-far-less-new-study-finds/); players petitioned Valve for an AI filter and suggested hiding "specific categories of games, such as incremental ones" [WN Hub](https://wnhub.io/news/stores-and-publishing/item-52443). In r/incremental_games' thread "The incremental games community finally broke me" (662 points, 232 comments): "Disclosure of AI is essential"; "I worry that vibe coding will cause the genre to stagnate"; but also "Tag it AI, if people don't like it, then they can filter those games out" and "I don't especially care if a game is AI as long as it's good." [snapshot](https://reddit.sentinel-team.org/posts/1pdda40/snapshots/2025-12-05T13%3A31%3A24.529936Z). Player-visible agent-generated content must be disclosed under Valve's rule; backend balance simulation need not.

### (c) IP constraint

A WoW-themed game is unsellable. Blizzard shut Nostalrius (2016, "more than 800,000 registered users") and Felmyst (2017, offline five hours after launch via a cease-and-desist from Mitchell Silberberg & Knupp), ordered Everlook's EU servers closed Sept 22, 2025, forced Stormforge to close by May 2026, and after suing Turtle WoW in August 2025 obtained a settlement with a permanent injunction (April 2026) barring servers, "modified or 'remastered' clients," and "'successor' projects." [GameSpot](https://www.gamespot.com/articles/world-of-warcrafts-fan-made-legacy-server-shut-dow/1100-6451979/), [Shacknews](https://shacknews.com/article/100712/world-of-warcraft-felmyst-fan-server-shut-down-5-hours-after-launch), [TweakTown](https://www.tweaktown.com/news/111116/), [Icy Veins](https://wp-prod.icy-veins.com/turtle-wow-is-over-blizzard-settlement-forces-shutdown/), [PC Gamer](https://pcgamer.com/games/world-of-warcraft/turtle-wow-classic-server-announces-shutdown-after-blizzard-wins-injunction). Original IP is mandatory; the systems (skills, crafting, prestige) are not protectable and can be kept.

### (d) Where web idle games monetize now

Kongregate stopped accepting new games July 1, 2020 (128,000 Flash games). [Game Developer](https://www.gamedeveloper.com/game-platforms/kongregate-shuts-off-game-submissions-as-flash-s-final-days-approach). Today: itch.io lets the developer set the cut 0–100% (default ~90% to the dev) but brings no audience; ad portals share ~50–60% of ad revenue (CrazyGames, Poki ~50%, GameDistribution 50/50, GameMonetize 45%), CrazyGames IAP is "invite-only"; Patreon is supplementary. [Cinevva](https://app.cinevva.com/guides/web-game-monetization), [Phaser guide](https://generalistprogrammer.com/tutorials/phaser-game-monetization-guide). The proven path (Melvor, Magic Research, Increlution) is a free web build that grows a community, then a paid Steam release.

### (e) Biggest risk

Discoverability in a flooded, AI-sceptical genre. A solo original-IP idle game with agent-generated content is precisely the profile the community is learning to filter; without a visual hook or publisher the realistic comps are $40k–$90k lifetime gross, and the median idle release earns far less.

### (f) Scores

- **AI-build fit: 9/10.** Fully self-contained, no compliance surface, and simulation-based balance testing is a natural agent loop; content generation is doable but must be disclosed.
- **Value / sellability: 4/10.** Hit-driven with a low median; the realistic outcome is a five-figure gross, and the game is not an acquirable asset in the way a SaaS with MRR is.

---

## Comparison

| Idea | Top competitors | Typical price | Demand signal strength | Biggest risk | AI-build fit | Value |
|---|---|---|---|---|---|---|
| 1. Small self-managed HOA software | PayHOA, HOA Start, Condo Control, Buildium, AppFolio, HOALife | $39–$59/mo per community; ACH $2.35–2.45/txn | Medium: 112k–149k self-managed boards nationally, ~10,300 AZ associations, annual statute churn; but leader already has 574 reviews | Low ACV + slow volunteer sales vs. entrenched incumbent | 8 | 5 |
| 2. STR compliance for hosts/PMs | Avalara MyLodgeTax, HostReady (in Hospitable/Lodgify/Hostfully), PermitAndGo; gov-side Granicus, Deckard | $27/property/mo (+$299 setup reported) | Medium-high: 700+ jurisdictions paying for enforcement, AZ statute with fines up to $3,500; hosts call it "a mess" | Legal-data treadmill and liability; PMS bundling in 2026 | 6 | 5 |
| 3. WCAG scanning for small public entities | accessiBe, UserWay, AudioEye, Siteimprove, Monsido, Silktide, Pope Tech, Equalize Digital | $25–$479/mo; gov suites $300–600+/mo | Medium: 5,114 suits in 2025 (64% <$25M revenue), but Title II small-entity deadline slipped to Apr 26, 2028 | Commoditized scanning; buyers get it from CMS vendors; deadline/exemption drift | 7 | 6 |
| 4. Shopify apps (niches) | B2B Wholesale Hub, SparkLayer, PreOrder Now, Zapiet, EasyRoutes; Shopify native Tax/Bundles/B2B | $19–$99/mo per merchant; 0% rev share to $1M lifetime | Medium: real solo exits, but 77% of new apps have zero reviews; median app $2.5–5k/mo | Shopify nativizing categories + discovery | 8 | 6 |
| 5. Original-IP idle game on Steam | Melvor Idle, Rusty's Retirement, Nodebuster, Magic Research 2, Increlution | $3–$10 one-time; Steam 30% | Low-medium: large genre audience, but 54 idle releases/week and AI stigma | Invisibility in a flooded, AI-sceptical genre | 9 | 4 |

### Gaps (not filled with guesses)
No search-volume figures; Reddit blocked (forum evidence is BiggerPockets, Airbnb Community and third-party snapshots); Capterra returned 403; Gamalytic was rate-limited (Steam figures are Raijin.gg estimates); no national STR-ordinance count exists; Shopify's B2B checkout GA date rests on one secondary source.
