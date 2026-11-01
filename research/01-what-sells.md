# What solo builders and tiny teams actually earn from software (2025–2026)

Research date: 2026-11-01. Scope: products a human-directed AI coding agent could plausibly build over a few months, that then run mostly hands-off and produce recurring revenue or a sellable asset.

## How to read the numbers

Every figure below carries one of these tags:

- **[verified]** – revenue pulled from a payment provider (Stripe, RevenueCat, App Store) by a third party (TrustMRR) or published by the platform itself (Shopify, Apple, Apify, OpenAI).
- **[self-reported]** – the founder's own public number (dashboard, post, interview). Survivorship bias is severe: people post wins, not failures.
- **[estimate]** – algorithmic third-party estimates (Gamalytic, AppMagic, Alinea). Gamalytic's own stated margin of error is up to ~30% (see [Steam 2025 coverage](https://gamedevreport.beehiiv.com/p/19-606-steam-games-made-16b-in-2025-most-made-0-but-that-s-not-a-problem)).
- **[vendor]** – published by a company selling into the category (brokerages, billing platforms, monetization SDKs). Directionally useful, numerically suspect.
- **[unverified]** – surfaced in search snippets or paywalled/blocked pages I could not read in full; URL given so you can check.

Limitations of this run: the web-search budget ran out partway through; the second half relied on fetching URLs already found. Indie Hackers, Product Hunt, and bigideasdb pages mostly returned 403/429. Category 10 (vertical software) and category 4 (WordPress) are thinner than I wanted; see the gaps section.

---

## The short version

1. **The distribution is brutal and well-measured.** On mobile, the median subscription app makes **under $50/month** a year after launch and only ~17% ever reach $1K/month ([RevenueCat SOSA 2025, 75K apps, $10B](https://www.revenuecat.com/state-of-subscription-apps-2025/); [RevenueCat employee summary, Mar 2025](https://hackernoon.com/nobody-wants-to-pay-for-apps-anymoreexcept-when-ai-is-involved)). On Steam, the **median 2025 release earned ~$318** and only 23.4% cleared $5K ([Gamalytic via Game Dev Report, Dec 2025](https://gamedevreport.beehiiv.com/p/19-606-steam-games-made-16b-in-2025-most-made-0-but-that-s-not-a-problem)). Community survey estimates put **~50% of active indie hackers at $0–$1K MRR** and $10K MRR at roughly the top 15% ([Oct 2026 roundup, low-quality source](https://blog.mean.ceo/?p=11175)).
2. **But the right tail is real and verifiable.** TrustMRR, which reads revenue directly from Stripe/RevenueCat/App Store, shows dozens of small products at $100K–$400K MRR, including an API business (1Lookup, **$272,886 MRR**, 749 subscriptions) and a vertical EHR for disability-services providers (Kibu, **$234,319 MRR**) ([TrustMRR leaderboard](https://trustmrr.com/), [1Lookup](https://trustmrr.com/startup/1lookup), [Kibu](https://trustmrr.com/startup/kibu)). Team sizes are not shown, so "solo" is not established for those.
3. **Boilerplates and "AI wrapper" products are the clearest casualties.** ShipFast went from a reported ~$133K/month peak in 2024 to **$1,395 in the last 30 days** on Marc Lou's own dashboard (Nov 2026) ([marclou.com](https://marclou.com/), [Product Hunt thread](https://sharemeow.producthunt.com/discussions/saas-boilerplates-101-how-marc-lou-makes-133k-mo-selling-javascript)). SimpleClosure's 2025 shutdown report singles out "AI wrappers and application-layer tools built quickly on commoditized models" as facing the sharpest correction ([Dec 2025 summary](https://www.01net.it/the-2025-startup-shutdown-more-capital-later-stages-and-the-first-ai-reckoning/)).
4. **Exits for small software trade at roughly 2.5–4x annual profit.** Flippa's 2025 marketplace data: SaaS averaged **2.7x profit** (top quartile 5.8x), apps 2.4x ([Flippa, Dec 2025](https://flippa.com/blog/2025-online-business-ma-insights-from-flippa/)); BizBuySell 2021–2025 small software deals clustered at **2.4–4x SDE** (midpoint 3.12x) ([iconic.co summary, Aug 2026](https://iconic.co/blog/saas-arr-multiples-and-software-valuation/)).
5. **MCP servers and ChatGPT/Claude apps are not yet a direct revenue channel for independents.** OpenAI's Apps SDK rules prohibit selling digital goods, subscriptions or credits and even linking to an upgrade page ([OpenAI submission guidelines](https://developers.openai.com/apps-sdk/app-submission-guidelines)); Claude Code's plugin marketplaces have no payment mechanism ([Anthropic, Oct 2025](https://claude.com/blog/claude-code-plugins)); the only large payout stream is Apify's Actor marketplace, **$1.7M/month to 5,600 developers** [verified, platform-reported] ([Apify](https://apify.com/partners/actor-developers)).

---

## 1. Micro-SaaS (B2B niche tools)

**Verified upper tail.** TrustMRR's leaderboard (hourly Stripe/RevenueCat reads) shows, as of Nov 1, 2026: Postiz (social scheduling, Nevo David) **$350,207 MRR**; 1Lookup (phone/email/IP validation API) **$272,886 MRR**, $5.69M all-time, founded Jan 2022; Rezi (AI resume builder, Jacob Jacquet) **$241,203 MRR** on 10,018 subscriptions at $29/mo or $149 lifetime, founded 2015, $10.47M all-time; Kibu (compliance/EHR for I/DD providers) **$234,319 MRR**, 375 subscriptions, "contact sales" pricing, founded 2022 ([TrustMRR](https://trustmrr.com/), [Rezi](https://trustmrr.com/startup/rezi), [1Lookup](https://trustmrr.com/startup/1lookup), [Kibu](https://trustmrr.com/startup/kibu)). Caveat: TrustMRR does not display headcount, and these are the top of a self-selected leaderboard.

**Named solo/tiny-team founders [self-reported]:**

- **Pieter Levels** – Photo AI reported at **$105K/month revenue, $80K/month profit** from a single 40,870-line index.php (Levels' own post, Mar 2026, via [summary](https://lilys.ai/en/notes/solopreneur-20260210/solopreneur-website-3m-earnings)); earlier reports of $132K/month ([ppc.land](https://ppc.land/how-one-photo-ai-app-generates-132k-monthly-after-70-failed-startups/)) and ~$155K/month late 2025 ([profile](https://yespress.io/pieter-levels)). His open-stats page was offline when checked ([nomads.com/open](https://nomads.com/open)).
- **Tony Dinh / TypingMind** – a chat UI for LLM APIs sold mostly as one-time licenses from $39 plus team subscriptions; **~$137K/month** per a 2025 tweet and tracker, $1M trailing revenue per his Nov 2024 newsletter ([Starter Story breakdown, Oct 2024](https://starterstory.com/typingmind-breakdown)). "Solo" is contested: Latka lists three people.
- **Marc Lou** – public dashboard shows **$1,257,216 in the last 12 months** across ~43 products; today's earners are TrustMRR ($37,714/mo), DataFast analytics ($30,902/mo), a "Ship or Die" community ($16,796/mo). Also disclosed: MakeLanding sold for $35K, GameWidget for $4K, VirallyBot shut down after $80K lifetime ([marclou.com, Nov 2026](https://marclou.com/); [Feb 2026 breakdown](https://indieai.pages.dev/blog/marc-lou-81683-february-2026-income-breakdown)).
- **Plausible Analytics** (two founders, open source) – $10K MRR Jan 2021, $500K ARR Oct 2021, **$1M ARR June 2022**, still 100% subscriber-funded in Jan 2026; took 324 days to reach the first $400 MRR ([Plausible blog](https://plausible.io/blog/open-source-saas), [Jan 2026 post](https://plausible.io/blog/customers-not-investors)). Older but the best-documented tiny-team trajectory I found.

**Pattern:** the durable ones are either (a) utilities with a measurable output (analytics, validation, screenshots) sold on usage, or (b) tools that sit on someone else's paid behavior (resumes, social posting). Hands-off is relative: Rezi has 10,018 subscriptions to support.

## 2. Chrome / browser extensions

- **Superpower ChatGPT** – ~260–270K installs, ~150K weekly actives, free for the first nine months, then paid features: **$20–30K MRR** (founder interview, Aug 2024; [summary](https://readray.ai/en/notes/ai-monetization-20251028/how-i-built-it-20k-chrome-extension)). Founder's stated lesson: should have charged earlier.
- **Rick Blyth (Merch Wizard, KDP Wizard)** – vertical extensions for Amazon Merch sellers; **~$10K MRR at 70–80% margin**, lifetime deals $249–$699, over **$500K total revenue**, and an exit for a "multi six-figure" sum at **57x monthly profit** ([dev.to, Jan 2023](https://dev.to/rickblyth/how-much-money-i-made-developing-chrome-extensions-2ecn)). Older, but the only documented extension exit with a multiple.
- **Easy Folders** (ChatGPT folder organizer) – **$3,700 MRR and $42,000 total** six months after launch ([Indie Hackers post, fetch blocked](https://www.indiehackers.com/product/easy-folders/6-months-post-launch-my-chrome-extension-has-hit-3-700-in-mrr-and-42-000-in-total-revenue--O3qs28VAnAkcJw0j--M)) [unverified].
- **The typical case** – an AI prompt-assistant extension listed for sale in Apr 2026: 5,000+ installs, 28 paying subscribers, **~$135 MRR, $1,440 TTM revenue, asking $10,000** ([Microns listing](https://microns.io/startup-listings/ai-prompt-assistant-extension-for-llms)). ExtensionFast, a $149 starter kit for building extensions, has **$618 all-time verified revenue** since Mar 2025 ([TrustMRR](https://trustmrr.com/startup/extensionfast)).

**Economics.** Chrome Web Store has had no native payments since 2020, so you run Stripe/licensing yourself ([vendor guide](https://fungies.io/monetize-chrome-extension-2026)). **Platform risk is the defining feature:** Manifest V2 extensions were disabled by default in March 2025 and uBlock Origin was removed from the store, with final MV2 removal in Chrome 150/151 by mid-2026 ([PCWorld, Jun 2026](https://www.pcworld.com/article/3160794/the-last-lifeline-for-ublock-origin-in-chrome-is-almost-gone-for-good.html); [timeline](https://www.cloudwards.net/ublock-origin-no-longer-supported/)). A database of **9,656 expired extensions** marketed to indie hackers describes many as "Manifest V3 casualties" ([Chrome Goldmine, Mar 2026](https://peerpush.com/p/chrome-goldmine)). After the Honey affiliate scandal (Honey lost 3M of 20M users in two weeks, 4M+ by May 2025), Google changed Web Store policy in March 2025 to ban claiming affiliate commissions without providing a discount ([Wikipedia summary with citations](https://en.wikipedia.org/wiki/Honey_(company))) – an entire extension monetization model was rewritten overnight.

## 3. Shopify apps

**Fee rules [verified].** From June 16, 2025, developers keep 100% of the **first $1M in lifetime app revenue (counted from Jan 1, 2025)** and Shopify takes **15% above that**; the previous annual reset was sunset, and earnings aggregate across associated partner accounts ([Shopify changelog, Apr 24, 2025](https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share)). Developers with $20M+ app revenue or $100M+ company revenue pay 15% on everything ([Sicard benchmarks, Jul 2026](https://taylorsicard.com/blog/shopify-partner-earnings-benchmarks-2026)). Note the lifetime-vs-annual switch itself: a unilateral terms change that reduced lifetime take-home for every successful developer.

**What developers earn.** A July 2026 industry write-up citing Week One Labs' 2026 benchmarks: **the median listed app earns under $1,000/month**; top-decile apps in a category plausibly $5K–$50K MRR; top 1% above $100K; solo and two-person apps concentrate in the **$2K–$20K MRR** band; healthy apps have sold for about **3–5x annual revenue**; Mantle (app-billing tooling) shut down ([AdsX](https://adsx.com/blog/how-much-shopify-app-developers-make)) [vendor]. A broker summary of 29 Shopify app sales reported **median trailing revenue $85,000, median asking $250,000, 6 of 29 solo-run** ([bigideasdb](https://bigideasdb.com/how-to-sell-a-shopify-app)) [unverified – page rate-limited].

**Named example [self-reported].** Erikas Malisauskas (Kaching Appz), ex-freelancer: first app scaled to **$6.5K MRR and sold for $250K** (~3.2x ARR); Kaching Bundles Quantity Breaks now "6-figure MRR", portfolio **$100K+/month** (Mar 2025 post, [repost](https://www.goodreads.com/author_blog_posts/25605328-former-freelancer-earning-100k-month-with-this-app); [original](https://www.indiehackers.com/post/qdReVAgLjz6EpW4OrJSI)). At the other end, a profitable Shopify app portfolio at **~$500 MRR** was listed on Acquire.com ([listing](https://app.acquire.com/startup/wavy6rcufu-profitable-shopify-app-portfolio-with-500-mrr-and-low-support-overhead)).

**Under-served niches.** No source gave a credible list. The closest: "utilities that connect workflows across surfaces, or serve niches too small for Shopify to prioritize, tend to survive" ([AdsX](https://adsx.com/blog/how-much-shopify-app-developers-make)). Merchants pay for bundles, product options, and operational glue; generic reviews/upsell apps are crowded.

## 4. WordPress / WooCommerce plugins (freemium)

This category produced the least 2025–2026 hard data.

- **Barn2** (plugin company since 2016): WooCommerce Product Options reached **$500K lifetime sales** between Sept 2022 and Oct 2026, with 82 customer-requested features added ([Barn2, Oct 2026](https://barn2.com/blog/woocommerce-product-options-founder-story/)) [self-reported].
- **A 50-plugin WooCommerce portfolio on Flippa**: **$6,741 net revenue in 2025**, 81 active subscribers, 98% margin, asking **$30,000** (~4.4x revenue) ([Flippa listing](https://flippa.com/12279112-profitable-wordpress-woocommerce-plugin-portfolio-50-plugins-8k-annualized-revenue-23-yoy-growth-zero-marketing-spend-massive-untapped-potential)). This is probably closer to the median outcome than Barn2.
- **Conversion benchmark**: Freemius states most freemium products convert **1–2%** of free users to paid ([Freemius](https://freemius.com/blog/increase-freemium-upgrades-wordpress-plugin-theme/)) [vendor; page would not render in this session].
- An Indie Hackers story titled "How Building My Own WordPress Solution Evolved into $24K MRR" exists but is paywalled and undated ([link](https://www.indiehackers.com/post/oWv1ysO0F7ObEuxirYET)) [unverified].

Takeaway: WordPress.org is still free distribution to a huge install base, but 1–2% conversion means you need tens of thousands of active installs for a meaningful business, and support load (PHP versions, theme conflicts, WooCommerce updates) is the opposite of hands-off.

## 5. MCP servers, Claude/ChatGPT apps, agent tools

Be skeptical; almost everything written about "MCP monetization" is written by someone selling a monetization SDK.

- **"95% of MCP servers make zero revenue"** – no dataset, no methodology; the article exists to sell the MnemoPay SDK ([dev.to, Jun 2026](https://dev.to/t49qnsx7qtkpanks/95-of-mcp-servers-make-zero-revenue-the-protocol-gap-thats-causing-it-7m2)). Treat as marketing.
- **Actual disclosed numbers are tiny.** The operator of an ad network for MCP servers reports its first commission: **$0.21** on a $0.30 test click, Mar 2, 2026 ([dev.to](https://dev.to/nicolas_fainstein/how-i-monetized-my-mcp-server-and-what-the-021-first-commission-means-3o1k)). A founder with 61 agent-payment products reports an MCP server with **11 installs and $0 revenue**, and a companion post titled "61 Products, 92 Weekly Downloads, $7 Revenue" ([dev.to, Jun 2026](https://dev.to/rumblingb/how-i-monetize-mcp-servers-in-2026-real-numbers-no-hustle-bros-4ekf)). A $19/month crypto-data MCP server publishes a pricing rationale but no revenue, noting "the vast majority" of ~318 servers in a Claude Code marketplace are $0 ([dev.to, Apr 2026](https://dev.to/whoffagents/pricing-an-mcp-server-in-2026-why-we-charge-19mo-when-the-market-average-is-0-nig)). Marketplaces quote take rates of 18% (Agent Bazaar) and 30% (agentic-ads) ([dev.to, Feb 2026](https://dev.to/nicolas_fainstein/the-state-of-mcp-monetization-in-2026-28i4)) [vendor].
- **Directories are consolidating, not paying.** Smithery announced its MCP Proxy and managed identity "shut down on October 31" (2026) and now routes to Arcade; no payout features exist ([smithery.ai](https://smithery.ai/)).
- **ChatGPT Apps SDK: you cannot charge.** Commerce is limited to physical goods; selling "digital products or services, including subscriptions, digital content, tokens, or credits" is prohibited, as is linking to any checkout or upgrade page; Instant Checkout is a beta for select retail partners ([OpenAI guidelines](https://developers.openai.com/apps-sdk/app-submission-guidelines); [forum confirmation, Jan 2026](https://community.openai.com/t/chatgpt-app-monetization-apps-sdk/1372343)).
- **Claude Code plugins**: public beta Oct 9, 2025; anyone can host a marketplace from a git repo; nothing about payment ([Anthropic](https://claude.com/blog/claude-code-plugins)).
- **The one real payout stream is Apify.** Its Actor marketplace (scrapers/automations exposed "via API and MCP, usable inside Claude, Cursor, Windsurf") reports **$1.7M paid out monthly to 5,600 community developers**, "many developers earn over $3k", developers set per-run pricing ([Apify partner page](https://apify.com/partners/actor-developers)) [platform-reported; take rate not stated]. Average is ~$300/developer/month, so the distribution is presumably as skewed as everywhere else.
- **MCP as distribution, not product.** Established paid APIs now ship MCP servers as a channel (e.g., Bannerbear's Sept–Oct 2026 tutorials for Claude Code, ChatGPT and Codex, [blog](https://www.bannerbear.com/blog/)). RevenueCat reports AI-assistant referrals rose from under 5% of new sign-ups in Q2 2024 to **over 35% in Q2 2025** ([TechCrunch, Sep 2025](https://techcrunch.com/2025/09/23/dedicated-mobile-apps-for-vibe-coding-have-so-far-failed-to-gain-traction/)).

Verdict: build the paid API or SaaS first; an MCP server is a free front door to it. No evidence of standalone MCP servers being acquired or sustaining an independent at meaningful revenue.

## 6. Mobile apps (iOS/Android) built with AI agents

**The base rates [verified, large-N].**
- SOSA 2025 (75,000 apps, $10B): median app **<$50/month** at 12 months; **~17%** ever reach $1K/month; **~3.5%** reach $10K/month; top 5% make **$8,880/month** vs **≤$19** for the bottom quartile; ~30% of annual subscriptions cancel in month one; year-1 retention 44% (annual plans), 17% (monthly), 3.4% (weekly) ([RevenueCat](https://www.revenuecat.com/state-of-subscription-apps-2025/); [summary](https://hackernoon.com/nobody-wants-to-pay-for-apps-anymoreexcept-when-ai-is-involved)).
- SOSA 2026 (115,000 apps, $16B): average monthly revenue after one year **$72**, middle 50% **$16–$429**, top 10% **$2,500+**; share of new apps reaching $1K MRR within two years **fell from 19% to 17%**; top-quartile apps grew MRR 80% YoY while the bottom quartile fell 33%; RevenueCat attributes the flood of new apps partly to "vibecoding and lower barriers" ([RevenueCat SOSA 2026](https://www.revenuecat.com/sosa-26-insights/)). **AI apps earn 41% more per payer ($30.16 vs $21.37) but retain worse (21.1% vs 30.7% annual)** ([ppc.land, Mar 2026](https://ppc.land/ai-apps-earn-41-more-per-user-but-churn-30-faster-revenuecat-finds/)).
- Apple takes **15%** under the Small Business Program for developers under $1M in proceeds, reverting to the standard rate once you exceed it ([Apple](https://developer.apple.com/app-store/small-business-program/)).

**Named AI-built examples [self-reported].**
- **STOPPR** (David Attias, ex-quant, no app background): cloned a competitor's design in Figma, built in Cursor in ~2.5 weeks, MVP in a month; **$5K first month, $12K/month at five months**, ~60K downloads, ~900 payers, ~35% margin after TikTok ads; tools cost ~$400/month including Cursor Ultra at $200 ([Starter Story, Oct 2025](https://rosetta.to/u/starterstory/i-vibe-coded-a-12k-month-mobile-app-stoppr-breakdown)).
- **Creator Buddy** (Alex Finn, Claude Code + Next.js/Supabase): **$300K ARR at 90% margins** (web, not mobile); **Vibed Agents** (Paulius): $9K MRR, $100K+ total across 10+ apps; **Modest Mitkus**: $23K MRR SaaS with no coding background ([Jan 2026 roundup](https://iamjohnellison.substack.com/p/the-vibe-coding-wave-is-here-5-builders)).
- Counter-evidence: dedicated vibe-coding mobile apps themselves flopped — the largest had ~16,000 downloads and ~$1,000 consumer spend ([TechCrunch/Appfigures, Sep 2025](https://techcrunch.com/2025/09/23/dedicated-mobile-apps-for-vibe-coding-have-so-far-failed-to-gain-traction/)). RevenueCat powers purchases for **over 50% of AI-built iOS apps**, so the SOSA numbers above already include the vibe-coded cohort.

Hands-off reality: mobile needs continuous paid acquisition (STOPPR runs TikTok Spark Ads), weekly-plan churn is ~97% in year one, and review/compliance churn is constant.

## 7. APIs and data products

- **ScreenshotOne** (Dmytro Krasun, solo, launched ~2022): **$269K ARR** in Jan 2025 ([analysis](https://superframeworks.com/blog/screenshot-one)); ~$19.8K MRR in June 2025 per the founder's LinkedIn (via search); started at $1K MRR in May 2023 ([Indie Hackers product page](https://www.indiehackers.com/product/screenshotone-the-best-screenshot-api)). Competes on cold-start speed, ad/cookie-banner blocking and support, not on novelty.
- **1Lookup** (phone/email/IP validation): **$272,886 MRR, 749 subscriptions, $5.69M all-time**, pay-as-you-go credits, founded Jan 2022 ([TrustMRR, verified](https://trustmrr.com/startup/1lookup)). Team size unknown.
- **Unnamed PDF API** priced at $0.01/document instead of $29/month plans: $180 in month one, **$4.2K MRR at month six** ([PeerPush, Nov 2025](https://peerpush.com/blog/side-project-to-5k-mrr-monetization)) [unverifiable anecdote].
- **Marketplace economics**: RapidAPI takes roughly **20–25%** of subscriptions; a seller describes $49/$99/$199 tiers and no disclosed revenue ([dev.to, May 2026](https://dev.to/donnyautomation/the-honest-economics-of-selling-apis-on-rapidapi-from-a-live-portfolio-13k0)). Apify is the alternative channel (per-run billing, $1.7M/month total payouts, see §5).

APIs are the most hands-off category on this list once stable: usage billing, no UI to support, and the buyers are developers who self-serve. The moat is reliability and edge-case handling, which is exactly what months of agent-driven iteration can buy.

## 8. Boilerplates, templates, starter kits

- **ShipFast** (Marc Lou): $199–$299 one-time, 8,445 buyers claimed, founder blurb still says "$45,000 a month" ([shipfa.st](https://shipfa.st/)). Reported peak **~$133K/month in 2024** ([Product Hunt thread](https://sharemeow.producthunt.com/discussions/saas-boilerplates-101-how-marc-lou-makes-133k-mo-selling-javascript)); $528K in its first four months ([Starter Story](https://starterstory.herokuapp.com/marc-lou-shipfast)); **$8.8K in Feb 2026** ([breakdown](https://indieai.pages.dev/blog/marc-lou-81683-february-2026-income-breakdown)); **$1,395 in the last 30 days** as of Nov 2026 ([marclou.com](https://marclou.com/)). That is a ~99% decline from peak, in a market where AI agents now generate the scaffolding a boilerplate used to sell.
- **ExtensionFast** (Chrome-extension starter kit, $149): **$618 all-time** since Mar 2025 ([TrustMRR](https://trustmrr.com/startup/extensionfast)).
- **Notion templates**: Thomas Frank's **$1,000,508 in 2022** (Ultimate Brain $760K; $49–$179 price points; 50K-subscriber email list; "verified by Business Insider" per the source) is the canonical success; the same article's cautionary example is a Reddit creator with **$901 over 14 months from 14 templates** ([CartMango, May 2026](https://cartmango.com/how-to-sell-notion-templates/)) [vendor]. No 2025–2026 Frank figures surfaced.
- Renewal-model anecdote: a Next.js boilerplate at $199 + $79/year got **$31K new + $18K renewals in year two** ([PeerPush](https://peerpush.com/blog/side-project-to-5k-mrr-monetization)) [unverifiable].

Verdict: saturating fast and structurally exposed to the very tool you would build with. The only template businesses still growing are audience-first (Frank's YouTube) rather than product-first.

## 9. Indie games (Steam, itch.io), idle/incremental and systems-heavy

**Market shape [estimate].** 19,606 Steam releases in 2025; **median revenue $318**; 23.4% over $5K ([Gamalytic via Game Dev Report](https://gamedevreport.beehiiv.com/p/19-606-steam-games-made-16b-in-2025-most-made-0-but-that-s-not-a-problem)). Steam grossed ~$17.7B in 2025 (+15%), indies ~25% (~$4.5B) ([Alinea via Notebookcheck, Dec 2025](https://www.notebookcheck.net/Indie-games-accounted-for-25-of-Steam-s-revenue-in-2025.1189429.0.html)).

**Solo outliers (not a plan, but proof the ceiling is unbounded):**
- **Schedule I** (Tyler, TVGS, solo): **$151M gross in 2025** per Alinea, ~$130M per Gamalytic; 1.8–2M copies by end of March 2025 ([80.lv](https://80.lv/articles/walter-white-simulator-schedule-i-becomes-steam-s-most-popular-indie-game-of-2025/)).
- **Megabonk** (Vedinad, solo, $9.99): **1M copies in two weeks** after Sept 18, 2025 ([GamesRadar](https://www.gamesradar.com/games/roguelike/out-of-nowhere-roguelike-hit-megabonk-has-sold-1-million-copies-in-2-weeks-solo-creator-says-ill-be-eating-spaghetti-with-extra-sauce-tonight/); [Wikipedia](https://en.wikipedia.org/wiki/Megabonk)); ~$27M 2025 estimate ([Gamalytic via Game Dev Report](https://gamedevreport.beehiiv.com/p/19-606-steam-games-made-16b-in-2025-most-made-0-but-that-s-not-a-problem)).

**Idle/incremental specifically.**
- Genre leaders 2025 [AppMagic estimates]: **IdleOn $22M+**, **Cast n Chill $6.8M**, **The Farmer Was Replaced $6.8M**, Cookie Clicker 720K+ copies, Bongo Cat $670K ([NextBigGames, May 2026](https://nextbiggames.com/2026/05/09/idle-games-on-steam-revenue-trends-2026/)). Team sizes not stated there.
- **(the) Gnorp Apologue** (Myco, solo, $6.99): **120K+ copies in its first month** (Dec 2023), 3.5 years of part-time development, only 1.7K wishlists at launch; discovery came from YouTube/streams because the systems were fun to watch ([GameDiscoverCo, Jan 2024](https://newsletter.gamediscover.co/p/how-this-solo-dev-incremental-game)).
- **Tiny Terraces** (VuVuu, solo, idle farming, Early Access July 2025): **$26,799 gross / $17,363 net in month one**, 5,000+ Steam copies; the mobile ports made **$346** ([Gamepressure, Nov 2025](https://www.gamepressure.com/newsroom/how-much-can-you-earn-as-an-indie-dev-tiny-terraces-creator-share/zb889b)).
- **Idle Pixel Fantasy** (Max Ritters, solo, F2P + cosmetic DLC, 1.5 months of work): **$4.8K gross in five days**, 23K players, 4.4K DLC sold, propelled by Steam's "Popular Free Games" shelf ([80.lv, Oct 2025](https://80.lv/articles/no-marketing-no-microtransactions-usd4-8k-and-23k-players-in-5-days-one-indie-story)).
- Saturation signal: PC Gamer counted **54 idle games released on Steam in a single week**, "many look eerily similar" ([PC Gamer](https://www.pcgamer.com/gaming-industry/steam-week-in-review-54-idle-games-released-on-steam-last-week-and-many-look-eerily-similar/)).

Idle/incremental is unusually AI-agent-friendly (systems, balance tables, little art) and genuinely hands-off after launch (no servers, no churn). But a $300 median and a $25K–$30K realistic first-month ceiling for a competent solo entry at $5–$10 pricing is the honest expectation. The game is a lottery ticket with positive expected learning; it is not recurring revenue.

## 10. Niche vertical software (HOA, trades, dental, legal, agriculture, churches)

I could not retrieve verified solo-founder examples in the named verticals before the search budget ran out. What the evidence does show:

- Vertical compliance/EHR for a narrow regulated niche can reach **$234K MRR with 375 customers** (Kibu, I/DD providers, [TrustMRR](https://trustmrr.com/startup/kibu)) — i.e., ~$625/customer/month, enterprise-style pricing with "contact sales".
- "Vertical" also works as a vertical-on-a-platform: Rick Blyth's Amazon-Merch extensions (~$10K MRR, exit), Kaching's Shopify bundle app (6-figure MRR), Barn2's WooCommerce options plugin ($500K lifetime). The platform supplies distribution; the niche supplies pricing power.
- Marketplace-level observation: Shopify utilities "serving niches too small for Shopify to prioritize" survive ([AdsX](https://adsx.com/blog/how-much-shopify-app-developers-make)).

Treat this as a research gap, not a negative finding. Follow-up queries worth running: Starter Story and Indie Hackers interviews filtered by "church", "HOA", "field service", "dental lab", "farm", plus Acquire.com listings tagged vertical SaaS.

---

## The acquisition market for small software

| Source | What it measures | Multiple | Notes |
|---|---|---|---|
| [Flippa 2025 insights (Dec 2025)](https://flippa.com/blog/2025-online-business-ma-insights-from-flippa/) | Flippa closed deals, Jan–Dec 2025 | SaaS **2.7x profit** avg (top quartile 5.8x); Apps 2.4x (5.4x); Content 2.6x (5.5x); E-com 1.4x | By price band: $10K–$100K deals 1.8x avg; $100K–$250K 2.1x; $250K–$1M 2.3x; $1M+ 2.9x. SaaS volume +73.5%, content −33.5%, YouTube +155%. ~90 days to close $100K–$500K deals with clean books. 37% of buyers bought more than once. |
| [BizBuySell via iconic.co (Aug 2026)](https://iconic.co/blog/saas-arr-multiples-and-software-valuation/) | Small software/SaaS sold 2021–2025 | Middle half **2.4x–4x SDE**, midpoint 3.12x; 1.51x revenue | Median sale price $820K in 2025, up 34% from $612.5K. |
| [Acquire.com via bigideasdb](https://bigideasdb.com/state-of-saas-valuations-2026) [unverified – page rate-limited] | Acquire closed deals <$10M EV | Median **3.9x profit** in both 2024 and 2025 | Asking prices on 615 live listings averaged 2.6x TTM revenue / 10.7x TTM profit — asks run far above closes ([listing analysis](https://bigideasdb.com/saas-valuation-multiples-2026)). |
| [Empire Flippers (Jul 2025)](https://empireflippers.com/you-can-sell-your-business-for-life-changing-money-in-2025-heres-the-secret/) | Content/affiliate sites, 145 deals | **30x monthly profit (2023) → 27x (2024) → 24x (2025)** | Sites sold fell ~70 → ~50 → ~30 (YTD May 2025); sale/ask ratio 90% → 82%; days on market 55 → 85. |
| Shopify apps | Observed ranges | **3–5x annual revenue** ([AdsX](https://adsx.com/blog/how-much-shopify-app-developers-make)); 3–6x ARR ([Sicard](https://taylorsicard.com/blog/shopify-partner-earnings-benchmarks-2026)) | Kaching: $6.5K MRR → $250K ≈ 3.2x ARR. |
| Chrome extension exit | One founder | **57x monthly profit** (~4.75x annual) ([Blyth, 2023](https://dev.to/rickblyth/how-much-money-i-made-developing-chrome-extensions-2ecn)) | Multi-six-figure lump sum. |

**What buyers want (Flippa, Empire Flippers):** recurring revenue, "Rule of 40" style growth-plus-margin, diversified acquisition channels, clean institutional-quality financials, documented operations, honest risk disclosure. Professional buyers (PE, search funds, family offices) have moved down into the $500K–$5M band.

**What kills or discounts a deal:** incomplete financials and an unclear narrative; single-channel or single-customer concentration; algorithm dependency (Google/YouTube); dependence on affiliate/ads rather than own customers; sellers anchoring to asking prices (a 15–20% negotiation discount is normal even with professional valuation); and for content, "AI reducing informational search queries" ([Empire Flippers](https://empireflippers.com/you-can-sell-your-business-for-life-changing-money-in-2025-heres-the-secret/); [Flippa](https://flippa.com/blog/2025-online-business-ma-insights-from-flippa/)).

Practical implication: a $5K/month-profit asset realistically sells for **$150K–$240K** (2.5–4x), not the 10x+ seen in asking prices. If the goal is a sellable asset, recurring B2B revenue with Stripe-verifiable history is what moves you toward the top quartile.

## What is getting commoditized or dying

1. **AI wrappers.** SimpleClosure's 2025 shutdown report: AI companies were 15.9% of closures; "AI wrappers and application-layer tools built quickly on commoditized models" face the sharpest correction, while infra/dev-tool failures had raised about twice the capital ([summary, Dec 2025](https://www.01net.it/the-2025-startup-shutdown-more-capital-later-stages-and-the-first-ai-reckoning/); [press release](https://www.businesswire.com/news/home/20251223595831/en/)). TechRadar reported that 81% of US AI startups that wound down in 2025 were "wrappers and apps" ([article](https://www.techradar.com/pro/when-the-model-takes-away-the-moat)) [unverified – body truncated on fetch]. Mechanism: the model vendor ships your feature natively (memory, prototyping, coding agents).
2. **Generic horizontal SaaS, even at scale.** The "SaaSpocalypse": Claude Cowork launched Jan 12, 2026; 11 industry plugins (legal, finance, sales, marketing, support…) on Jan 30; ~**$285B** wiped from software/financial stocks in one session, the S&P North American Software Index ~15% down in January; ~$1T lost since Jan 28 and the S&P software & services index ~25% below its Oct 2025 peak by Feb 10; Atlassian ~70% down over 12 months ([Outlook Business, Feb 4 2026](https://www.outlookbusiness.com/explainers/saaspocalypse-explained-decoding-claude-cowork-plugins-that-triggered-havoc-in-it); [ACS Information Age, Feb 10 2026](https://ia.acs.org.au/article/2026/new-ai-tools-tank-software-stocks.html); [Motley Fool, Mar 2026](https://www.fool.com/investing/2026/03/09/why-are-software-stocks-down/)). Whatever the public-market overreaction, buyers of small SaaS now price "could an agent plugin replace this?" into the multiple.
3. **SEO content sites.** Flippa content-site transactions −33.5% in 2025; Empire Flippers multiples down from 30x to 24x monthly profit with a third the volume, explicitly blamed on Google dependence and AI answers ([Flippa](https://flippa.com/blog/2025-online-business-ma-insights-from-flippa/); [Empire Flippers](https://empireflippers.com/you-can-sell-your-business-for-life-changing-money-in-2025-heres-the-secret/)).
4. **Boilerplates and starter kits.** ShipFast's ~99% decline from peak (above). The thing AI agents are best at is scaffolding.
5. **Low-effort app-store volume plays.** RevenueCat: more apps submitted (vibecoding), fewer reaching $1K/month (19% → 17%) ([SOSA 2026](https://www.revenuecat.com/sosa-26-insights/)). Steam: 54 look-alike idle games in a week.
6. **MCP directories as businesses.** Smithery shut its proxy/identity service Oct 31, 2026 ([smithery.ai](https://smithery.ai/)).

## Platform risk, 2025–2026 (cite these when choosing a dependency)

- **Model vendor cuts you off.** Anthropic reduced Windsurf's first-party access to Claude 3.x models with **less than five days' notice** in June 2025, citing "capacity for sustainable partnerships"; Windsurf never got direct Claude 4 access and users had to bring their own API keys ([TechCrunch, Jun 3 2025](https://techcrunch.com/2025/06/03/windsurf-says-anthropic-is-limiting-its-direct-access-to-claude-ai-models/)). If your product's margin is someone else's inference, your margin is theirs to change.
- **Pricing changes upstream.** Cursor switched its $20 Pro plan to $20 of API-rate usage on June 16, 2025; users exhausted it "after just a few prompts" with Claude and some were billed unexpectedly; CEO apologized and refunded ([TechCrunch, Jul 7 2025](https://techcrunch.com/2025/07/07/cursor-apologizes-for-unclear-pricing-changes-that-upset-users/)).
- **Browser policy.** Manifest V3 removed uBlock Origin and thousands of MV2 extensions (Mar 2025 default-off, final removal mid-2026) ([PCWorld](https://www.pcworld.com/article/3160794/the-last-lifeline-for-ublock-origin-in-chrome-is-almost-gone-for-good.html)); Google's March 2025 affiliate policy change after Honey ([Wikipedia](https://en.wikipedia.org/wiki/Honey_(company))).
- **Store terms.** Shopify converted its $1M zero-commission exemption from annual to lifetime, effective June 16, 2025 ([Shopify](https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share)). Apple's 15% rate ends the year you pass $1M ([Apple](https://developer.apple.com/app-store/small-business-program/)). OpenAI's Apps SDK bans selling digital goods outright ([OpenAI](https://developers.openai.com/apps-sdk/app-submission-guidelines)).

## Data on AI-built software being sold

- **Stripe's 2025 annual letter (Feb 24, 2026):** twice as many startups reached $10M ARR within three months as in 2024; the 2025 Stripe cohort grew 50% faster than 2024's; Atlas formations +41%; 20% of new Atlas startups charged their first customer within 30 days (8% in 2020); 57% of new Stripe businesses were outside the US; $1.9T processed (1.6% of global GDP) ([TechCrunch](https://techcrunch.com/2026/02/24/more-startups-are-hitting-10m-arr-in-3-months-than-ever-before/); [Stripe](https://stripe.com/annual-updates/2025)). No solo-founder breakdown was published.
- **RevenueCat:** powers in-app purchases for **over 50% of AI-built iOS apps**; AI-assistant referrals >35% of new sign-ups by Q2 2025 ([TechCrunch, Sep 2025](https://techcrunch.com/2025/09/23/dedicated-mobile-apps-for-vibe-coding-have-so-far-failed-to-gain-traction/)); AI apps monetize 41% better per payer and retain ~30% worse ([SOSA 2026](https://ppc.land/ai-apps-earn-41-more-per-user-but-churn-30-faster-revenuecat-finds/)).
- **TrustMRR** is the best open dataset of Stripe-verified small-product revenue; its top-50 sums to roughly $9.3M MRR (my arithmetic from the page), heavily skewed by one creator platform at $3.57M ([TrustMRR](https://trustmrr.com/)).
- **Self-reported AI-built wins** cluster at $9K–$25K MRR (STOPPR, Vibed Agents, Mitkus) with one $300K ARR outlier (Creator Buddy) ([Starter Story](https://rosetta.to/u/starterstory/i-vibe-coded-a-12k-month-mobile-app-stoppr-breakdown); [roundup](https://iamjohnellison.substack.com/p/the-vibe-coding-wave-is-here-5-builders)).
- Not found in this run: a16z, Lenny's Newsletter or Indie Hackers survey data specific to agent-built products. Flag for follow-up.

## What this implies for a months-long, mostly hands-off, agent-built project

- The categories with (a) verified six-figure-MRR small products, (b) usage pricing that scales without support, and (c) tolerable platform risk are **developer-facing APIs/data utilities** and **narrow B2B tools with a measurable output** (validation, screenshots, analytics, compliance documents). Exits in those categories clear 2.5–4x profit on Flippa/BizBuySell and higher on Acquire.
- **Vertical-on-a-platform** (Shopify, WooCommerce, Amazon sellers) buys distribution you cannot build; the cost is a 15% cut above $1M and exposure to terms changes. Solo apps there realistically land at $2K–$20K MRR.
- **Idle/systems games** are the most agent-buildable and the most hands-off, but a median of $318 and a realistic solo ceiling in the tens of thousands means they are an asymmetric bet, not income.
- **Avoid**: boilerplates, generic AI wrappers, SEO content, and anything whose only distribution is an MCP/ChatGPT directory.

---

## Summary table

| Name | Category | Revenue or price | Year | Tag | Source |
|---|---|---|---|---|---|
| 1Lookup | API / data | $272,886 MRR; $5.69M all-time | 2026 | verified | https://trustmrr.com/startup/1lookup |
| Rezi | Micro-SaaS (resume) | $241,203 MRR; 10,018 subs; $29/mo or $149 lifetime | 2026 | verified | https://trustmrr.com/startup/rezi |
| Kibu | Vertical (I/DD EHR) | $234,319 MRR; 375 subs | 2026 (Apr) | verified | https://trustmrr.com/startup/kibu |
| Postiz | Micro-SaaS | $350,207 MRR | 2026 | verified | https://trustmrr.com/ |
| Marc Lou portfolio | Micro-SaaS / templates | $1,257,216 trailing 12 mo; ShipFast $1,395/30d | 2026 | self-reported | https://marclou.com/ |
| ShipFast peak | Boilerplate | ~$133K/mo | 2024 | self-reported | https://sharemeow.producthunt.com/discussions/saas-boilerplates-101-how-marc-lou-makes-133k-mo-selling-javascript |
| Photo AI (Levels) | Micro-SaaS (AI) | ~$105K/mo rev, ~$80K/mo profit | 2026 (Mar) | self-reported | https://lilys.ai/en/notes/solopreneur-20260210/solopreneur-website-3m-earnings |
| TypingMind (Dinh) | Micro-SaaS | ~$137K/mo; $1M trailing (Nov 2024) | 2024–25 | self-reported | https://starterstory.com/typingmind-breakdown |
| Plausible Analytics | Micro-SaaS (2 founders) | $1M ARR | 2022 | self-reported | https://plausible.io/blog/open-source-saas |
| ScreenshotOne | API (solo) | $269K ARR | 2025 (Jan) | self-reported | https://superframeworks.com/blog/screenshot-one |
| Unnamed PDF API | API | $4.2K MRR at month 6, $0.01/doc | 2025 | unverified | https://peerpush.com/blog/side-project-to-5k-mrr-monetization |
| RapidAPI sellers | API marketplace | 20–25% marketplace cut | 2026 | vendor/anecdote | https://dev.to/donnyautomation/the-honest-economics-of-selling-apis-on-rapidapi-from-a-live-portfolio-13k0 |
| Apify Actor developers | Agent tools / MCP | $1.7M paid out per month to 5,600 devs | 2026 | platform-reported | https://apify.com/partners/actor-developers |
| agentic-ads MCP ad network | MCP | $0.21 first commission | 2026 (Mar) | self-reported | https://dev.to/nicolas_fainstein/how-i-monetized-my-mcp-server-and-what-the-021-first-commission-means-3o1k |
| email-verify-mcp | MCP | 11 installs, $0 revenue | 2026 (Jun) | self-reported | https://dev.to/rumblingb/how-i-monetize-mcp-servers-in-2026-real-numbers-no-hustle-bros-4ekf |
| ChatGPT Apps SDK | Apps platform | Selling digital goods prohibited | 2026 | verified (policy) | https://developers.openai.com/apps-sdk/app-submission-guidelines |
| Superpower ChatGPT | Chrome extension | $20–30K MRR; ~150K WAU | 2024 (Aug) | self-reported | https://readray.ai/en/notes/ai-monetization-20251028/how-i-built-it-20k-chrome-extension |
| Merch Wizard / KDP Wizard (Blyth) | Chrome extension (vertical) | ~$10K MRR; $500K+ total; exit at 57x monthly profit | 2023 | self-reported | https://dev.to/rickblyth/how-much-money-i-made-developing-chrome-extensions-2ecn |
| Easy Folders | Chrome extension | $3,700 MRR; $42K total at 6 months | ~2024–25 | unverified | https://www.indiehackers.com/product/easy-folders/6-months-post-launch-my-chrome-extension-has-hit-3-700-in-mrr-and-42-000-in-total-revenue--O3qs28VAnAkcJw0j--M |
| AI prompt extension (for sale) | Chrome extension | ~$135 MRR; $1,440 TTM; asking $10,000 | 2026 (Apr) | listing | https://microns.io/startup-listings/ai-prompt-assistant-extension-for-llms |
| ExtensionFast | Starter kit | $618 all-time; $149 price | 2025–26 | verified | https://trustmrr.com/startup/extensionfast |
| Kaching Appz (Malisauskas) | Shopify apps | First app $6.5K MRR sold for $250K; portfolio $100K+/mo | 2025 (Mar) | self-reported | https://www.goodreads.com/author_blog_posts/25605328-former-freelancer-earning-100k-month-with-this-app |
| Shopify App Store median | Shopify apps | Median listed app <$1K/mo; solo apps $2K–$20K MRR | 2026 | vendor | https://adsx.com/blog/how-much-shopify-app-developers-make |
| Shopify revenue share | Shopify platform | 0% to $1M lifetime, 15% above (from Jun 16, 2025) | 2025 | verified | https://shopify.dev/changelog/update-to-shopifys-app-developer-revenue-share |
| Shopify app sales (29 deals) | Shopify exits | Median trailing rev $85K; median ask $250K | 2025–26 | unverified | https://bigideasdb.com/how-to-sell-a-shopify-app |
| Barn2 WooCommerce Product Options | WordPress plugin | $500K lifetime sales (Sep 2022–Oct 2026) | 2026 | self-reported | https://barn2.com/blog/woocommerce-product-options-founder-story/ |
| 50-plugin WooCommerce portfolio | WordPress plugins | $6,741 net 2025; 81 subs; asking $30K | 2025–26 | listing | https://flippa.com/12279112-profitable-wordpress-woocommerce-plugin-portfolio-50-plugins-8k-annualized-revenue-23-yoy-growth-zero-marketing-spend-massive-untapped-potential |
| Freemium plugin conversion | WordPress | 1–2% free-to-paid | n/d | vendor | https://freemius.com/blog/increase-freemium-upgrades-wordpress-plugin-theme/ |
| STOPPR (Attias) | Mobile (Cursor-built) | $12K/mo at month 5; ~900 payers; ~35% margin | 2025 (Oct) | self-reported | https://rosetta.to/u/starterstory/i-vibe-coded-a-12k-month-mobile-app-stoppr-breakdown |
| Creator Buddy (Finn) | Web SaaS (Claude Code) | $300K ARR, 90% margin | 2025–26 | self-reported | https://iamjohnellison.substack.com/p/the-vibe-coding-wave-is-here-5-builders |
| Vibed Agents (Paulius) | Apps (Claude Code) | $9K MRR; $100K+ total; 10+ apps | 2025 | self-reported | https://iamjohnellison.substack.com/p/the-vibe-coding-wave-is-here-5-builders |
| Median subscription app | Mobile | <$50/mo at 12 months; 17% ever reach $1K/mo | 2025 | verified (75K apps) | https://www.revenuecat.com/state-of-subscription-apps-2025/ |
| Apps after year one | Mobile | Avg $72/mo; middle 50% $16–$429; top 10% $2,500+ | 2026 | verified (115K apps) | https://www.revenuecat.com/sosa-26-insights/ |
| Apple Small Business Program | Mobile platform | 15% commission under $1M proceeds | 2026 | verified | https://developer.apple.com/app-store/small-business-program/ |
| Thomas Frank Notion templates | Templates | $1,000,508 in 2022; $49–$179 | 2022 | secondary/vendor | https://cartmango.com/how-to-sell-notion-templates/ |
| Schedule I (TVGS, solo) | Indie game | $151M gross 2025 (Alinea) / ~$130M (Gamalytic) | 2025 | estimate | https://www.notebookcheck.net/Indie-games-accounted-for-25-of-Steam-s-revenue-in-2025.1189429.0.html |
| Megabonk (Vedinad, solo) | Indie game, $9.99 | 1M copies in 2 weeks; ~$27M 2025 est. | 2025 | self-reported + estimate | https://en.wikipedia.org/wiki/Megabonk |
| IdleOn | Idle game | $22M+ in 2025 | 2025 | estimate (AppMagic) | https://nextbiggames.com/2026/05/09/idle-games-on-steam-revenue-trends-2026/ |
| The Farmer Was Replaced | Idle/automation game | $6.8M | 2025 | estimate (AppMagic) | https://nextbiggames.com/2026/05/09/idle-games-on-steam-revenue-trends-2026/ |
| (the) Gnorp Apologue (Myco, solo) | Incremental, $6.99 | 120K+ copies in first month | 2023–24 | self-reported | https://newsletter.gamediscover.co/p/how-this-solo-dev-incremental-game |
| Tiny Terraces (VuVuu, solo) | Idle farming | $26,799 gross / $17,363 net month one; 5,000+ copies; mobile $346 | 2025 | self-reported | https://www.gamepressure.com/newsroom/how-much-can-you-earn-as-an-indie-dev-tiny-terraces-creator-share/zb889b |
| Idle Pixel Fantasy (Ritters, solo) | F2P clicker + DLC | $4.8K gross in 5 days; 4.4K DLC | 2025 | self-reported | https://80.lv/articles/no-marketing-no-microtransactions-usd4-8k-and-23k-players-in-5-days-one-indie-story |
| Steam median release | Indie games | $318 median; 23.4% > $5K; 19,606 releases | 2025 | estimate (Gamalytic) | https://gamedevreport.beehiiv.com/p/19-606-steam-games-made-16b-in-2025-most-made-0-but-that-s-not-a-problem |
| Flippa SaaS deals | Exits | 2.7x profit avg; 5.8x top quartile | 2025 | marketplace data | https://flippa.com/blog/2025-online-business-ma-insights-from-flippa/ |
| BizBuySell small software | Exits | 2.4–4x SDE; median price $820K | 2021–25 | marketplace data | https://iconic.co/blog/saas-arr-multiples-and-software-valuation/ |
| Acquire.com closed deals | Exits | Median 3.9x profit | 2024–25 | unverified | https://bigideasdb.com/state-of-saas-valuations-2026 |
| Empire Flippers content sites | Exits | 24x monthly profit (down from 30x); volume −57% | 2025 | brokerage data | https://empireflippers.com/you-can-sell-your-business-for-life-changing-money-in-2025-heres-the-secret/ |
| SimpleClosure shutdown report | Commoditization | AI = 15.9% of closures; wrappers hit hardest | 2025 | vendor report | https://www.01net.it/the-2025-startup-shutdown-more-capital-later-stages-and-the-first-ai-reckoning/ |
| SaaSpocalypse | Commoditization | ~$285B one-session loss; ~$1T since Jan 28, 2026 | 2026 | press | https://ia.acs.org.au/article/2026/new-ai-tools-tank-software-stocks.html |
| Windsurf / Anthropic | Platform risk | Claude 3.x access cut with <5 days' notice | 2025 (Jun) | press | https://techcrunch.com/2025/06/03/windsurf-says-anthropic-is-limiting-its-direct-access-to-claude-ai-models/ |
| Cursor pricing change | Platform risk | $20 plan → $20 API-rate usage; refunds | 2025 (Jul) | press | https://techcrunch.com/2025/07/07/cursor-apologizes-for-unclear-pricing-changes-that-upset-users/ |
| Chrome MV3 / uBlock Origin | Platform risk | MV2 off by default Mar 2025; final mid-2026 | 2025–26 | press | https://www.pcworld.com/article/3160794/the-last-lifeline-for-ublock-origin-in-chrome-is-almost-gone-for-good.html |
| Stripe 2025 letter | AI-built software | 2x as many startups to $10M ARR in 3 months; 20% of Atlas startups charge within 30 days | 2025 | platform-reported | https://techcrunch.com/2026/02/24/more-startups-are-hitting-10m-arr-in-3-months-than-ever-before/ |

## Gaps and verification notes

- Indie Hackers, Product Hunt and bigideasdb blocked fetches (403/429); figures from those sites are marked unverified and taken from search snippets or page titles.
- No verified solo-founder examples retrieved for HOA, dental, church, trades, legal or agriculture software; the vertical evidence above is platform-vertical (Shopify/Woo/Amazon) plus one verified regulated-niche SaaS of unknown team size.
- No 2025–2026 primary data from Acquire.com or Quiet Light was retrievable; Flippa, BizBuySell and Empire Flippers primary pages were.
- Gamalytic/AppMagic/Alinea game figures disagree with each other by 10–20% (Schedule I: $130M vs $151M); treat all as estimates.
- The TechRadar "81% wrappers" statistic and the MicroConf/Baremetrics solo-SaaS medians that appear in search results could not be read in full and are either flagged or omitted.
- A follow-up search pass should target: Acquire.com's 2025/2026 multiples report, Week One Labs' 2026 Shopify benchmarks, Freemius' current conversion data, Starter Story vertical-SaaS interviews, and Gamalytic genre pages for idle/incremental.
