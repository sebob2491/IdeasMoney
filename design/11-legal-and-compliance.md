# Legal and compliance design

Status: design, 2026-11-01. This is not legal advice. It is the list of what the studio will do, what needs a lawyer, and when. Evidence is in `research/03-tailwinds-2026.md` Section 6, `research/05-validate-ideas-6-10.md` Idea 7, and `research/06-ai-reverse-engineering-games.md` Section 5.

## 1. Studio

- **Entity.** Form an LLC before the first paying Track A customer or the first Steam dollar. Both products carry warranty and liability exposure that should not sit on you personally. Counsel question: single LLC for both or one per product.
- **Accounts** are owned by the entity's email, with you as the human owner; routines never hold ownership credentials.
- **Insurance.** Errors-and-omissions coverage for Track A once enterprise customers sign; not before.
- **Contracts.** Track A customer terms of service and a data processing addendum template by M3; Track B end-user license on Steam uses Valve's default plus a short custom EULA.
- **Records.** `DECISIONS.md`, the legal register, and the AI-generation log are kept indefinitely.

## 2. Track A: public-records data product

### 2.1 Posture

Build only on government public records: statutes and official compilations are not copyrightable under the government-edicts doctrine (*Georgia v. Public.Resource.Org*, 2020), facts are not copyrightable, and federal works are public domain. Avoid consumer platforms entirely: after *Reddit v. Anthropic* (N.D. Cal., Sept 2026) and *Reddit v. Perplexity* (S.D.N.Y., July 2026), terms-of-service contract and DMCA theories survive dismissal even for public data.

### 2.2 Legal register (gate for every source)

No source is enabled until its register entry is complete and approved. Fields: source name; jurisdiction; URL; data format; bulk or API availability; terms of use reviewed (date, by whom, notes); robots.txt policy observed; copyright or commercial-use posture of the state or agency; personal-data fields present; fields we store; fields we deliberately drop; rate limit set; contact address in our User-Agent; approval date and approver. The register is a protected file; only a human commit changes it.

### 2.3 State posture notes to verify with counsel

- **Arizona.** State works are asserted to be copyrighted and the public-records statute requires a statement of commercial purpose and allows agencies to charge for commercial use. Before enabling any Arizona state source, counsel reviews the specific agency's terms and whether a commercial-purpose request is required.
- **California, Florida, Texas, New York** publish bulk licensing data on open-data portals under permissive terms; verify each portal's license text and record it.
- **Vendor-hosted portals** (the systems many counties and cities run) have their own terms; prefer the agency's bulk file or a public-records request over scraping the vendor's UI.

### 2.4 FCRA scoping

Selling data about individuals for decisions on their eligibility for credit, insurance or employment can make a vendor a consumer reporting agency under the Fair Credit Reporting Act, with duties we cannot meet as a solo product. Measures: the product is framed and structured as business-entity data; sole-proprietor records carry the business fields published by the board; customer terms state that the data may not be used as a factor in determining an individual's eligibility for credit, insurance, employment or housing; enterprise contracts repeat the permissible-use clause; counsel confirms the framing before the first insurer or lender contract.

### 2.5 Privacy

Qualifier names and business addresses are personal information of individuals in some states' privacy laws. Measures: data minimization by field (the register lists what we drop); a public privacy policy; a correction and removal request process with a 10-business-day service level, reachable from every entity page and the API docs; `noindex` on entity pages about sole proprietors by default; retention of raw snapshots for provenance only, never republished.

### 2.6 Customer terms (outline)

Permissible use and the FCRA clause; no resale or redistribution of the dataset; attribution required on the free tier; rate limits and fair use; accuracy disclaimer (records are as published by the source as of the fetched date); no warranty of fitness; limitation of liability; indemnity for misuse; termination; governing law Arizona.

### 2.7 Scraping conduct

Identify ourselves in the User-Agent with a contact address; obey robots.txt; rate-limit per source; prefer bulk files and APIs; cache and use conditional requests; stop on request from an agency and record it in the register.

## 3. Track B: original-IP idle game

### 3.1 Original IP policy

No names, places, lore, item names, art, audio or text from Blizzard, Warcraft or any other publisher. A forbidden-terms list is enforced by a lint that fails CI on any hit in content, UI strings, docs and store copy. The legacy bundle is the only file allowed to contain such terms and it is excluded from the lint. Mechanics (rounds, skills, crafting, prestige) are not copyrightable and may be reused; look-and-feel is protected (*Tetris v. Xio*; *Spry Fox v. LOLApps*), so the UI is designed fresh, not traced.

### 3.2 Naming and trademark

Before the Steam page: USPTO search for the final name in the relevant classes (software and games), a Steam store search, a domain check, and a general web search for conflicting games. Record the results in design 12 and `DECISIONS.md`. Rename the repo so the Blizzard mark leaves the project URL.

### 3.3 AI disclosure

Steam's rule since January 2026 exempts AI coding tools but requires disclosure of any generated content players see or hear. Policy: all player-facing text is human-reviewed before release; generated art, if any, is logged and disclosed; the content pipeline keeps a generation log (what, when, which tool) that produces the disclosure text. The itch.io page carries the same disclosure. Draft disclosure text is written at M3 and recorded in `DECISIONS.md`.

### 3.4 Third-party assets

Every asset has a license entry: game-icons.net icons are CC BY 3.0 and require attribution in the game credits and store page; CC0 packs need no attribution but are recorded anyway; the commissioned icon set comes with a written transfer of rights; fonts are checked for embedding and distribution rights. An `ATTRIBUTIONS.md` is generated from the asset ledger and shipped with the game.

### 3.5 Code and content licensing

The game repository stays private and the code proprietary until you decide otherwise; the paid Steam build is the reason. Tooling that is useful to others (the content linter, the extractor pattern) can be open-sourced later under MIT as a separate repo if you want the goodwill. Content (names, text, data) is proprietary.

### 3.6 Players and platforms

Telemetry is opt-in, anonymous, documented in a plain privacy policy. Saves are the player's; export is always available. Mods: data packs are allowed for non-commercial use and may not include third-party IP; the mod policy is published with the alpha. Steam Cloud and achievements follow Valve's terms. No ads, no in-app purchases, so no consumer-protection exposure from loot boxes or children's advertising rules.

## 4. Cross-cutting: AI-generated content

- Generation log for both tracks: what was generated, when, by which tool, reviewed by whom.
- Public-facing copy for Track A (pages, docs) is reviewed for accuracy claims; the words "compliant," "guaranteed" and "official" are banned by the copy lint.
- Nothing scraped from other games or other companies' products enters either repo.

## 5. Checklist by milestone

| When | Track A | Track B |
|---|---|---|
| Setup week | Legal register schema and the protected-file hook | Forbidden-terms list and lint; asset ledger file |
| M1 | Register entries for the first three sources; privacy policy draft | Attribution file for CC BY icons; mod policy draft |
| M2 | Register entries for 15 states; correction and removal process live | Public alpha privacy note; telemetry opt-in text |
| M3 | Counsel review: Arizona sources, FCRA framing, customer terms, DPA template; LLC formed; Stripe | Trademark search; repo rename; disclosure text; commissioned-art rights transfer; Steam EULA |
| M4 | E&O insurance when the first enterprise contract signs | Launch checklist: credits, attributions, disclosure, age questionnaire |
