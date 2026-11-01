# Dry River: game design document (Track B)

Status: design, 2026-11-01. Builds on `design/02-track-b-idle-game.md` (architecture, roadmap, content targets), `design/03-track-b-setting-options.md` (Option 1, Sonoran myth, decided by D-005), and `design/04-track-b-port-inventory.md` (the legacy content shape). Audience evidence: `research/04-validate-ideas-1-5.md` Idea 5; build evidence: `research/06-ai-reverse-engineering-games.md` Section 3. "Dry River" is a codename until the trademark search at M3. Every number in this document is a starting hypothesis for the simulator and the weekly hour of play, not a promise.

## 1. Vision, pillars, player, comps

**Vision.** You are a drifter who makes camp at a dead river under two moons and, over hundreds of hours, brings a settlement back: prospect the washes, cut mesquite, trap and fish, smelt and smith, build in adobe, distill tonics, and hold off the things that walk out of the heat. When the sky finally breaks, you call the monsoon. The land floods and resets, but the river remembers you: watermarks stay, and some places only exist after the rain.

**Design pillars.**

| Pillar | What it means in practice | What it rules out |
|---|---|---|
| Actually idle | Every system progresses while the tab is closed, up to 24 hours, computed by the same simulation that runs live. Combat retreats rather than dies offline. | Minigames, timing checks, anything that pauses progress when you are away |
| Everything feeds something | Every material has at least two sinks, every skill touches at least three others. The content linter enforces it. | Vendor-trash materials, dead-end skills |
| Prestige that compounds | The monsoon is the heart of the game, not an endgame afterthought. Watermarks are permanent, visible from hour one, and change how the next cycle plays. | Prestige that only multiplies numbers, prestige that wipes progress without a visible return |
| A deep middle | Hours 20 to 150 have their own loops (expeditions, dungeon sets, ledger chains, camp building, charm etching), not a longer grind of the first ten hours. | Padding levels 60 to 99 with the same actions at longer timers |
| Honest craft | Original setting, disclosed and human-reviewed text and art, moddable data from day one, no ads, no purchases inside the game. | Undisclosed generated content, dark patterns, timers that sell their own skip |

**Target player.** The Melvor Idle and Magic Research player: 25 to 45, plays on a second monitor or a phone during the day, values systems depth, spreadsheets optional, tolerant of plain icons, hostile to ads and to anything that smells like a mobile cash shop. Secondary: the Steam Deck "podcast game" player who wants a six-minute check-in that is always rewarding.

**Comps and the difference.**

| Comp | What it does well | What Dry River does differently |
|---|---|---|
| Melvor Idle | Breadth of skills, mastery, dungeons, mod ecosystem | One concurrent action slot grows to three through the camp; prestige is a core loop rather than a late expansion; a setting nobody else has |
| Magic Research 1 and 2 | Tight prestige cadence, research tree, readable formulas | Longer per-cycle arc with a persistent world; crafting economy with real material flows; combat with gear sets |
| Increlution | Elegant death-and-retry loop, strong narrative beats per run | Less punishing; progress carries through watermarks and the collection log; the narrative lives in the ledger, not the run timer |

The one-line pitch for the store page: "An idle RPG of the dry country. Prospect, build, fight, and call the rains."

## 2. World bible

### 2.1 Setting and tone

A high desert frontier a century after an unnamed mining company pulled out. Canyons, mesas, dry washes, a ruined mission, salt flats, a mine that goes down farther than anyone remembers digging. Two moons: the Elder, large and slow, and the Lantern, small and fast; their crossings set the night calendar. The dead are not quiet here, the dust gets up and walks, and a bird the size of a barn nests on the mesa and brings the storms.

Tone: mythic, dry, kind. Folklore rather than horror. No guns, no outlaws with hats; the "outlaw constructs" are wind-up watchmen the company left behind, still enforcing a claim nobody holds. Humor is deadpan and lives in item flavor text and the caravan master's ledger notes. Spanish loanwords appear only where they are ordinary English (adobe, arroyo, mesa, playa, cienega). No real Indigenous nation names, no sacred figures as monsters, no real towns.

### 2.2 The eight areas

| Area | Unlock | Named places | Character |
|---|---|---|---|
| Dry River Camp | Start | The Campfire; the Dead Riverbed; the Trading Post; the Cistern | Home. Every building you raise appears here. Low-level gathering, no combat |
| Agave Flats | Cartography 1 | Bluestem Field; Jackrabbit Warren; Shepherd's Tank; Coyote Hollow | First wilderness. Harvesting, trapping, beasts |
| Copper Wash | Cartography 10 | The Green Seam; the Tailings; the Sluice; Smelter's Ruin | Industry. Ores, first constructs, the first dungeon (Smelter's Ruin) |
| The Old Mission | Cartography 20 | The Bell Tower; the Cloister Garden; Padre's Cellar; the Ossuary | Faith and ghosts. Herbs, tonics, the Ossuary dungeon |
| Salt Lake Bed | Cartography 32 | The White Flat; Brine Pools; Pillar Field; the Saltwalker's Seat | Blinding and strange. Salt, brine fishing, salt things, the Saltwalker dungeon |
| The Deep Mine | Cartography 45 | Shaft Nine; the Timber Works; the Foreman's Office; the Drowned Gallery (flooded only) | Dark, deep, procedurally deeper. Mine dead, rich ores, the Foreman dungeon |
| Thunderbird Mesa | Cartography 60 | Crow's Stair; the Lightning Field; the Needle; the Rookery | Storm country. Skyiron, storm kin, the Rookery raid tier |
| Two Moons Pass | Cartography 78 | Moonglass Scree; the Watchfire; the Narrows; the Far Gate | The edge of the map. Moonglass, the hardest open fights, the Far Gate raid tier |

Flooded areas (Section 8) are the ninth through twelfth areas and only appear after a monsoon. The 1.0 target of 25 areas counts sub-areas inside each of these (for example the Deep Mine has five depth levels) and the flooded set.

### 2.3 Creature families

| Family | Members (low to high) | Signature | Material drop |
|---|---|---|---|
| Beasts | Jackrabbit, Quail, Packrat, Rattler, Javelina, Coyote, Gila Monster, Bobcat, Mountain Lion | Fast, fair, food | Hide, meat, feathers, venom |
| Dust Spirits | Heat Shimmer, Dust Wraith, Whirlwind, the Haboob (dungeon boss) | High evasion, weak to silver | Spirit dust, dry salt |
| Outlaw Constructs | Tin Watchman, Claim Jumper, Wind-up Marshal, the Paymaster (dungeon boss) | High armor, no food, drop parts | Gears, brass scrap, copper wire |
| Mine Dead | Lamplighter, Powder Monkey, Mine Ghost, the Foreman (dungeon boss) | Bleed immune, carry lanterns, flee light | Grave iron, lantern oil, old coin |
| Caliche Kin | Adobe Husk, Caliche Golem, Kiln Brute | Slow, heavy, weak to blunt | Caliche, fieldstone, lime |
| Salt Things | Brine Crawler, Salt Pillar, Brine Hound, the Saltwalker (dungeon boss) | Drain tonics, immune to venom | Rock salt, brine glass |
| Storm Kin | Thunder Calf, Lightning Hound, Storm Crow, the Thunderbird (raid tier) | Chain damage, fast | Storm feathers, skyiron shard |
| Cryptids | Chupacabra, Night Hare, Mesa Lurker, the Tall Walker (Far Gate raid tier) | Rare spawns, unique drops | Cryptid tufts, moonglass |

### 2.4 Glossary (world terms)

| Term | Meaning |
|---|---|
| Camp | Your settlement at Dry River; the home screen |
| Monsoon | The prestige. Filling the Monsoon bar lets you call the rains |
| Watermark | A permanent upgrade bought with Silt after a monsoon |
| Silt | Prestige currency left behind by the flood |
| Keepsake | One item per cycle carried through a monsoon |
| Caravan | The traders who arrive every 20 hours with rotating stock and ledger requests |
| Scrip | Caravan credit, earned from ledger requests, spent at the Trading Post |
| Ledger | The light quest system: requests recorded in the caravan master's book |
| Wash | A dry streambed; where ores and gems collect |
| Arroyo | A steep wash |
| Tank | A stock pond that holds water after rain; fishing spot |
| Cienega | A desert marsh; appears only after a monsoon |
| Playa | A dry lakebed; the Salt Lake Bed fills into Playa Lake after a monsoon |
| Caliche | Hardpan calcium crust; building stone and a creature family |
| Adobe | Mud brick; the camp's building material |
| Mesquite | The common hardwood; also pods for flour |
| Ironwood | Dense slow-burning wood for tools and charcoal |
| Palo verde, Ocotillo, Cholla, Saguaro, Agave, Prickly pear, Jojoba, Creosote | Plants of the Flats and Mesa; each is an input |
| Skyiron | Meteoric iron found on the Lightning Field; the second-highest metal |
| Moonglass | Volcanic glass from Two Moons Pass that holds moonlight; the top-tier material |
| Moonsteel | Skyiron alloyed with moonglass; the top tool and weapon metal |
| Turquoise, Fire agate, Chrysocolla, Peridot, Amethyst, Obsidian | The six gem tiers |
| Concho | A silver disc ornament; the jewelry line's signature |
| Bandolier | The ammunition slot item for Marksmanship |
| Tonic | A Distilling consumable that buffs for a timed duration |
| Spirit | A Distilling drink that gives a long passive buff; one active at a time |
| Mash | The intermediate fermented input for spirits |
| Still | The Distilling tool |
| Etching | A charm effect engraved by Silversmithing onto gear (the legacy enchant system) |
| Hired Hand | A second concurrent action slot, unlocked by building the Bunkhouse |
| Drifter | You, in dialogue |
| Padre | The absent priest of the Old Mission; his ghost keeps the garden |
| Mission Bell | The bell whose ring opens the Ossuary |
| Dust Spirit | The dead heat given shape |
| Thunderbird | The storm-bringer nesting on the Mesa; the first raid tier |
| Saltwalker | The thing that walks the White Flat at noon; a dungeon boss |
| The Elder and the Lantern | The two moons |
| Haboob | A wall of dust; a weather event and a boss |
| Virga | Rain that evaporates before landing; a weather modifier that teases the Monsoon bar |
| Dry Spell | A challenge monsoon run with modifiers |
| Landmark | A surveyed point that gives a permanent small bonus |
| Collection Log | The list of every item, monster, and landmark seen |
| Mastery | Per-action proficiency, 1 to 99 |

### 2.5 Naming conventions

- Ids: `snake_case` with a type prefix (`item_copper_ingot`, `mon_dust_wraith`, `area_copper_wash`, `act_fish_catfish`, `wm_high_water`). Ids never change after release; display names can.
- Items: material plus form, two or three words ("Copper Ingot", "Rattler-hide Vest", "Saguaro-fruit Jam"). Gear sets take a possessive of a figure or place ("Padre's Vestments"). Tools take tier material plus tool ("Skyiron Pick").
- Monsters: two words at most, noun phrases, no apostrophes ("Dust Wraith", "Tin Watchman"). Bosses take "the" ("the Foreman").
- Places: "the" plus a landmark noun ("the Green Seam") or a plain compound ("Copper Wash").
- No invented fantasy phonology, no apostrophes in names, no "of the" chains longer than one.
- Flavor text: one sentence, present tense, dry. A joke every fifth item at most.

### 2.6 Forbidden-terms policy

The forbidden-terms lint runs on content, strings, code, commit messages, and the repo, with zero tolerance. Categories to screen, each with a list the lint maintains:

1. Blizzard and Warcraft proper nouns: continents, zones, cities, dungeons, raids, factions, races with distinctive spellings, named characters, deities, organizations.
2. Item, set, spell, ability, talent, and currency names from that franchise, including distinctive compounds and the legacy repo's strings counted in `design/04-track-b-port-inventory.md`.
3. Franchise-specific common words that read as that franchise in a game context (the hearth-and-stone compound, the legacy "heirloom" item category, the legacy zone id prefixes `the_`, `ench_`, `incubator_`, `ss_` and the rest).
4. Other protected franchises as a second list (tabletop monster names with trademark status, other MMO proper nouns), screened to "warn" rather than "fail."
5. Legacy flavor entities (the thief's ghost, the smith's ghost) are rewritten, not renamed, so the sentences do not survive either.

A human reads every rename-map entry before the public alpha, and the lint's near-miss mode (edit distance 2) is reviewed monthly.

## 3. Core loop and session design

### 3.1 The loop

Pick an action, watch it tick, spend what it makes on the next thing. At the scale of a session: gather, craft, equip, fight, build, unlock an area, repeat. At the scale of a cycle: fill the Monsoon bar, choose a keepsake, call the rains, spend Silt on watermarks, play the flooded areas before they drain, and start a faster cycle.

### 3.2 Idle versus active

- Action slots. One slot at the start. Building the Bunkhouse (Masonry 20) adds the Hired Hand slot for gathering or crafting only. The watermark "Second Shift" adds a third. Combat always takes the first slot; the camp keeps working while you fight.
- Nothing requires presence. There are no timing minigames, no "tap to collect," no decaying buffs that punish absence. Active play is rewarded by decisions, not reflexes: switching actions at the right level, chaining crafts, choosing dungeons, timing a tonic.
- Active bonus without a penalty: a "Watchfire" effect gives plus 10% action speed while the window is focused. It is a thank-you, small enough that idle players lose nothing meaningful.

### 3.3 Timers

Fixed timestep, ten ticks per second. Base action timers: gathering 2.0 to 6.0 seconds by tier; crafting 1.5 to 4.0 seconds; combat attack intervals 1.8 to 4.0 seconds; dungeon fights chain without travel. Speed modifiers stack additively to a floor of 40% of the base timer (tool tier, mastery, etchings, tonics, watermarks, Watchfire), so a maxed Woodcutting action is 0.8 seconds, never zero. Travel between areas costs real seconds (10 to 90) reduced by Cartography; it is the only waiting that is not production.

### 3.4 Mastery

Every action has a mastery level 1 to 99 with its own XP, earned alongside skill XP. Each skill has a mastery pool fed by a share of all its mastery XP; spending pool XP lets you push a favored action's mastery directly. Mastery checkpoints at 10, 25, 50, 75, 99 give action-specific effects (listed per skill in Section 4). The general rule: mastery makes an action cheaper, faster, or more generous, never strictly necessary for progression.

### 3.5 Offline progress

- Cap: 24 hours. Beyond that the queue stops and the Cistern (Masonry 10) banks up to 8 additional hours that you release with a click, so a weekend away is not wasted. The watermark "Deep Cistern" raises the bank to 24 hours.
- Computation: fast-forward of the real simulation at full fidelity for up to 2 hours of game time, then a batched equivalent (same expected output, variance collapsed) for the remainder. Both paths are deterministic from the seed and are checked against each other in CI.
- Inventory full stops the action and the summary says so. Inputs exhausted stops the action. Combat: auto-eat from the food slot; when food runs out, auto-retreat at the configured Vigor threshold; you never die offline.
- The offline summary lists XP per skill, items gained, items consumed, fights won, and anything rare, with "Keep" and "Sell all junk" buttons.

### 3.6 The first ten minutes, hour, and day

- Ten minutes. The campfire panel, three skills visible (Harvesting, Woodcutting, Prospecting), a cheerful ledger note from the caravan master asking for twenty mesquite pods. You cook your first meal at minute six, your first level-ups land every forty seconds, the Monsoon bar shows 0.3% with a tooltip that promises the rain matters.
- One hour. Cartography has opened Agave Flats; you have trapped a jackrabbit, cooked it, and taken your first fight against quail with a copper knife. The Trading Post sells a Copper Pick. You have built the Drying Rack and seen a 2-piece set bonus on the Rattler-hide pieces you cannot yet make. You understand that smelting needs charcoal, which needs wood, which the Flats have.
- One day. Copper Wash is open, the Smelter's Ruin has been cleared once (hour 4 to 6 for an active player), and the Bunkhouse is on the Masonry list at 20. You have a reason to log back in: the caravan returns in 20 hours, the first ledger chain is one request from a recipe reward, and the first offline summary has shown that the game plays itself fairly.

### 3.7 Daily return hook

Three hooks, none punishing: the caravan every 20 hours (rotating stock, a scrip payout for queued ledger requests, two "wanted" prices that are 50% higher than base for one day); the weather, a 24-hour modifier drawn from a short table (Virga: plus 15% Monsoon gain; Haboob: dust spirits spawn in every area; Clear Night: double moonglass; Still Air: plus 10% crafting speed); and the offline summary itself. There is no streak, no daily chest, no loss for missing a day.

## 4. Skills

### 4.1 Count and rationale

Fifteen skills: the fourteen from the setting options plus Cartography as a fully leveled fifteenth. Cartography is leveled rather than a passive unlock because the map needs its own progression (area unlocks, travel reduction, landmarks), because a gathering-only player deserves a non-combat route into every area, and because surveys consume paper and ink, which gives Harvesting, Leatherwork, and Distilling another sink. The content target in `02` said "about 14"; fifteen fits the budget and the UI rail (three groups of five).

### 4.2 Legacy mapping

| Legacy skill | Dry River skill | Notes |
|---|---|---|
| Mining | Prospecting | Ores and gems from washes; same node model, renamed tiers |
| Fishing | Fishing | Tanks, brine pools, mine water, and moon pools replace the legacy spots |
| Herbalism | Harvesting | Agave, cactus fruit, fibers, flowers |
| Cooking | Cooking | Food heals in combat; same role |
| Blacksmithing | Smelting plus Silversmithing | Split: ingots and intermediates versus finished metal goods |
| Alchemy | Distilling | Tonics and spirits |
| Enchanting | Silversmithing (etching line) | The five legacy affixes become etched charms |
| Tailoring | Leatherwork (woven line) | Cloth armor becomes yucca and cotton weave under one skill |
| Leatherworking | Leatherwork | Hide armor, packs, quivers |
| Jewelcrafting | Silversmithing | Rings, conchos, pendants |
| Skinning | Trapping | Hides come from trapping and from combat drops; no separate skill |
| Woodcutting (likely) | Woodcutting | Carries over |
| New | Masonry, Marksmanship, Cartography | Camp buildings, the ranged style, the map |

### 4.3 Gathering skills

**Prospecting.** You work the washes with a pick and a pan, following the green stain of copper to the seam. Later you climb the Lightning Field for iron that fell from the sky and the scree where glass remembers the moons. Inputs: none (tool only). Outputs: ores, stone, gems, with gem actions dropping bonus "pan finds" of lower gems. Interactions: feeds Smelting (ores), Masonry (fieldstone, caliche), Silversmithing (gems), Cartography (landmark markers need fieldstone).

| Level | Action | Output | Base timer |
|---|---|---|---|
| 1 | Break caliche | Caliche Chunk | 2.0s |
| 1 | Pan copper | Copper Ore | 2.4s |
| 8 | Dig tin | Tin Ore | 2.8s |
| 12 | Quarry fieldstone | Fieldstone | 3.0s |
| 15 | Pick turquoise | Turquoise | 3.4s |
| 25 | Mine iron | Iron Ore | 3.6s |
| 30 | Cut salt block | Salt Block | 3.2s |
| 35 | Pry fire agate | Fire Agate | 4.0s |
| 40 | Mine silver | Silver Ore | 4.2s |
| 45 | Chip chrysocolla | Chrysocolla | 4.4s |
| 55 | Mine gold | Gold Ore | 4.8s |
| 60 | Sift peridot | Peridot | 4.8s |
| 65 | Knap obsidian | Obsidian | 5.0s |
| 75 | Gather skyiron shards | Skyiron Shard | 5.6s |
| 80 | Split amethyst | Amethyst | 5.6s |
| 90 | Collect moonglass | Moonglass | 6.0s |

Mastery: 10, plus 5% double ore; 25, nodes never deplete (removes the legacy node-depletion timer for that action); 50, 10% chance of a gem pan find; 75, minus 15% timer; 99, every tenth action yields an extra ingot's worth.

**Harvesting.** Everything that grows out here either feeds you, clothes you, or kills you, and the trick is knowing which season. You learn the Flats one plant at a time. Inputs: none. Outputs: foods, fibers, herbs, flowers, nuts. Interactions: Cooking, Distilling, Leatherwork (yucca, cotton), Cartography (paper from fiber), Masonry (ocotillo for fences).

| Level | Action | Output |
|---|---|---|
| 1 | Gather mesquite pods | Mesquite Pods |
| 5 | Pick prickly pear | Prickly Pear |
| 10 | Strip creosote | Creosote Leaf |
| 15 | Cut yucca | Yucca Fiber |
| 20 | Cut agave heart | Agave Heart |
| 25 | Pick cholla buds | Cholla Bud |
| 35 | Pole saguaro fruit | Saguaro Fruit |
| 40 | Pick wild cotton | Wild Cotton |
| 45 | Gather desert lavender | Desert Lavender |
| 50 | Shake jojoba | Jojoba Nut |
| 60 | Cut ocotillo | Ocotillo Cane |
| 70 | Pick night-bloom | Night-bloom |
| 80 | Harvest datura | Datura |
| 90 | Gather rain lily | Rain Lily (flooded areas double it) |

Mastery: 10, plus 5% double; 25, seeds: 2% chance of a second item of the next tier; 50, no "bad pick" (removes the 10% empty-handed roll); 75, minus 15% timer; 99, fibers come pre-spun (saves one Leatherwork step).

**Woodcutting.** Mesquite burns hot and ironwood burns forever, and the camp eats both. The lightning-struck trees on the Mesa are something else again. Inputs: none. Outputs: logs. Interactions: Smelting (charcoal), Masonry (beams), Marksmanship (arrow shafts), Fishing (rods), Cooking (fuel is abstracted away; wood is not consumed to cook).

| Level | Action | Output |
|---|---|---|
| 1 | Gather dead brush | Brushwood |
| 5 | Cut mesquite | Mesquite Log |
| 15 | Cut palo verde | Palo Verde Log |
| 25 | Cut cottonwood | Cottonwood Log |
| 40 | Cut ironwood | Ironwood Log |
| 55 | Cut juniper | Juniper Log |
| 65 | Cut pinyon | Pinyon Log (plus pine nuts) |
| 75 | Cut sycamore | Sycamore Log |
| 85 | Cut lightning-struck mesquite | Charged Mesquite |
| 95 | Split petrified wood | Stonewood |

Mastery: 10, plus 5% double; 25, bark: 15% chance of a Tannin Bark drop (Leatherwork input); 50, two logs per cut at 10%; 75, minus 15% timer; 99, kindling: every log also yields Charcoal at 25%.

**Fishing.** The river is dead but the water is not gone; it hides in tanks, brine pools, the flooded shaft, and the still pools of the pass where the moons look back at you. Inputs: bait optional (Trapping gives worms and grubs for plus 20% speed). Outputs: raw fish, shells, brine. Interactions: Cooking (every fish), Distilling (fish oil for lantern oil), Silversmithing (pearl-like "tank shells"), Salt Lake Bed brine feeds Smelting glass.

| Level | Action | Where | Output |
|---|---|---|---|
| 1 | Net mosquitofish | Shepherd's Tank | Mosquitofish |
| 5 | Catch minnows | Shepherd's Tank | Minnow |
| 10 | Catch pupfish | Shepherd's Tank | Pupfish |
| 15 | Catch chub | Coyote Hollow | Chub |
| 25 | Catch sucker | Coyote Hollow | Sucker |
| 30 | Gig bullfrog | Cloister Garden pond | Bullfrog |
| 35 | Trap crayfish | Cloister Garden pond | Crayfish |
| 40 | Skim brine shrimp | Brine Pools | Brine Shrimp plus Brine |
| 45 | Catch catfish | Padre's Cellar | Catfish |
| 55 | Catch bass | Playa Lake (flooded) or Cellar | Bass |
| 65 | Catch gar | Playa Lake or the Narrows | Gar |
| 70 | Catch blind cavefish | Drowned Gallery (flooded) | Blind Cavefish |
| 85 | Catch moonfish | The Watchfire pools | Moonfish |
| 90 | Land stormfish | Lightning Field after Haboob | Stormfish |

Mastery: 10, plus 5% double; 25, no lost bait; 50, "tank shell" byproduct 5%; 75, minus 15% timer; 99, every catch is also cooked at 10% (a lucky campfire).

**Trapping.** You do not hunt out here; you set a snare, walk away, and come back. Patience is the whole skill, which is why it suits a game you leave running. Inputs: bait (any food item, consumed 1 per 5 catches; optional). Outputs: meat, hides, feathers, eggs, bone, venom. Interactions: Cooking (meat, eggs), Leatherwork (hides), Marksmanship (feathers), Distilling (venom), Fishing (grubs as bait).

| Level | Action | Outputs |
|---|---|---|
| 1 | Snare quail | Quail, Quail Egg, Feathers |
| 5 | Snare jackrabbit | Jackrabbit Meat, Light Hide |
| 10 | Trap packrat | Packrat Meat, Odd Trinket (sellable) |
| 15 | Net dove | Dove, Feathers |
| 25 | Trap rattler | Rattler Meat, Rattler Hide, Venom |
| 35 | Pit-trap javelina | Javelina Meat, Tough Hide, Tusk |
| 45 | Trap coyote | Coyote Meat, Coyote Pelt |
| 55 | Trap gila monster | Gila Scales, Venom |
| 65 | Trap bobcat | Bobcat Pelt |
| 75 | Trap mule deer | Venison, Deer Hide, Antler |
| 85 | Trap pronghorn | Pronghorn Meat, Fine Hide, Horn |
| 95 | Snare dust hare | Dust Hare Tuft, Spirit Dust |

Mastery: 10, plus 5% double; 25, humane: no empty traps; 50, bait lasts twice as long; 75, minus 15% timer; 99, trophy: 2% chance of the family's rare material.

### 4.4 Crafting skills

**Smelting.** One kiln, one crucible, and the patience to keep the heat even. Everything the camp builds with metal or glass starts here. Inputs: ores, charcoal, salt, sand, brine. Outputs: ingots, charcoal, lime, glass, rivets, wire. Interactions: Prospecting and Woodcutting in; Silversmithing, Masonry, Leatherwork (rivets), Marksmanship (arrowheads via Silversmithing) out.

| Level | Recipe | Inputs | Output |
|---|---|---|---|
| 1 | Copper Ingot | 2 Copper Ore, 1 Charcoal | 1 ingot |
| 1 | Charcoal | 2 any log | 3 Charcoal |
| 5 | Lime | 3 Caliche Chunk | 2 Lime |
| 8 | Bronze Ingot | 1 Copper Ore, 1 Tin Ore, 1 Charcoal | 1 ingot |
| 20 | Rivets | 1 Bronze Ingot | 10 Rivets |
| 25 | Iron Ingot | 2 Iron Ore, 2 Charcoal | 1 ingot |
| 30 | Brine Glass | 2 Salt Block, 1 Brine | 1 glass |
| 35 | Steel Ingot | 2 Iron Ingot, 3 Charcoal | 1 ingot |
| 40 | Silver Ingot | 2 Silver Ore, 2 Charcoal | 1 ingot |
| 45 | Copper Wire | 1 Copper Ingot | 5 Wire |
| 55 | Gold Ingot | 3 Gold Ore, 2 Charcoal | 1 ingot |
| 75 | Skyiron Ingot | 3 Skyiron Shard, 4 Charcoal (ironwood only) | 1 ingot |
| 90 | Moonsteel Ingot | 1 Skyiron Ingot, 1 Moonglass, 4 Charcoal | 1 ingot |

Mastery: 10, 5% chance to save charcoal; 25, double ingot at 5%; 50, charcoal cost minus 1; 75, minus 15% timer; 99, ore cost minus 1 (floor 1).

**Silversmithing.** The camp's one anvil does everything from arrowheads to wedding silver, and in time it learns to etch charms into metal that the dust spirits respect. It is the skill that makes the fighter and the showpiece both. Inputs: ingots, gems, wire, rivets, hides for grips. Outputs: melee weapons, arrowheads, jewelry, conchos, etched charms, tool heads. Interactions: Smelting and Prospecting in; Attack, Marksmanship, Leatherwork (conchos, riveted sets) out; every tool upgrade needs a smithed head.

| Level | Recipe | Output and role |
|---|---|---|
| 1 | Copper Knife | First melee weapon |
| 3 | Copper Arrowheads x10 | Marksmanship ammunition part |
| 5 | Copper Ring | Ring slot, plus 1 Attack |
| 10 | Bronze Blade | Melee weapon |
| 15 | Silver Concho (silver ore at 15 is bought until 40) | Jewelry and set piece component |
| 25 | Iron Blade, Iron Arrowheads | Weapon, ammo |
| 30 | Riveted Plates | Component for Riveted armor |
| 40 | Silver Ring, Turquoise Pendant | Jewelry with Defense and Vigor |
| 50 | Etching: Swift | Charm: minus 8% action timer on one tool or weapon |
| 52 | Etching: Bountiful | Charm: 10% double output |
| 55 | Gold Ring, Fire Agate Pendant | Jewelry with Marksmanship |
| 58 | Etching: Springfed | Charm: 10% chance not to consume inputs |
| 62 | Etching: Prospector's | Charm: plus 15% coin from sales |
| 66 | Etching: Hallowed | Charm: plus 8% XP |
| 75 | Skyiron Blade, Skyiron Arrowheads | High weapon tier |
| 80 | Amethyst Pendant | Tonic duration plus 20% |
| 90 | Moonsteel Blade | Top weapon tier |
| 95 | Moonglass Lens | Cartography top tool part; Chapel window |

The five etchings are the legacy acceleration, massive, replenishing, covetous, and divine affixes under new names. One etching per item; re-etching overwrites. Mastery: 10, 5% save a gem; 25, jewelry gets plus 1 to its stat; 50, etchings cost one fewer ingot; 75, minus 15% timer; 99, a smithed item is "signed" with a 2% bonus to its primary stat.

**Leatherwork.** Hide, fiber, and cotton: the three things that stand between you and the sun. The same hands that sew a pack sew a vest, and a riveted coat is just a vest that met the anvil. Inputs: hides, yucca, cotton, tannin bark, rivets, conchos. Outputs: Hide and Woven armor, Riveted armor (with Silversmithing parts), packs (bank space), quivers and bandoliers, parchment. Interactions: Trapping and Harvesting in; Defense, Marksmanship, Cartography (parchment), the bank out.

| Level | Recipe | Output |
|---|---|---|
| 1 | Tanned Hide | From Light Hide plus Tannin Bark or Lime |
| 5 | Hide Vest, Hide Boots | First Hide armor |
| 10 | Yucca Cord, Parchment | Cord for tools and rods; Cartography sheets |
| 15 | Woven Shirt | First Woven armor |
| 20 | Small Pack | Plus 6 bank slots (one per tier owned) |
| 25 | Rattler-hide Vest, Rattler-hide Boots | The first set (Section 5) |
| 30 | Quiver, Bandolier | Ammunition slot items |
| 35 | Javelina Coat | Hide armor |
| 40 | Cotton Duster | Woven armor |
| 45 | Coyote Cloak | Cloak slot |
| 50 | Riveted Vest | Riveted armor (plates plus tough hide) |
| 55 | Gila-scale Gloves | Hide gloves with venom resist |
| 60 | Large Pack | Plus 10 bank slots |
| 65 | Bobcat Hood | Hide hat |
| 75 | Deer-hide Armor set | Hide armor |
| 80 | Riveted Skyiron Coat | Riveted armor |
| 90 | Storm-feather Cloak | Top cloak, set piece |

Mastery: 10, 5% save a hide; 25, no tannin needed; 50, plus 2 armor on the piece; 75, minus 15% timer; 99, 5% chance to craft the piece pre-etched with Swift.

**Masonry.** Adobe is mud, straw, and time. Every building you raise at camp changes what the camp can do, and the camp is what survives the flood in outline. Inputs: caliche, fieldstone, lime, logs, bricks, brine glass, ingots for fittings. Outputs: bricks and mortar as intermediates; buildings as one-time constructions with upkeep-free passive effects; adobe "wards" (defensive consumables). Interactions: every skill, since buildings grant the camp its slots, storage, and bonuses.

| Level | Build | Effect |
|---|---|---|
| 1 | Adobe Brick, Caliche Mortar | Intermediates |
| 1 | Campfire Ring | Enables Cooking |
| 5 | Drying Rack | Cooking actions plus 5% double |
| 10 | Cistern | Offline bank of 8 hours (Section 3.5) |
| 15 | Smokehouse | Meat does not count against bank slots (stack as one) |
| 20 | Bunkhouse | Second action slot (Hired Hand) |
| 25 | Kiln | Smelting minus 10% timer |
| 30 | Tannery | Leatherwork minus 10% timer |
| 35 | Still-house | Distilling unlocks second tonic slot |
| 40 | Bell Tower repair | Opens the Ossuary dungeon; plus 1% Monsoon gain |
| 45 | Workshop | Tool upgrades cost minus 15% |
| 50 | Watchtower | Cartography travel minus 20% |
| 55 | Chapel | Offerings sink (Section 6); Hallowed etchings plus 2% |
| 60 | Granary | Food stacks infinitely; auto-eat picks the best |
| 65 | Assay Office | Sell prices plus 10% |
| 70 | Bathhouse | Vigor regen outside combat doubled |
| 80 | Mission Wall | Death penalty halved; wards craftable |
| 90 | Observatory | Reveals the next monsoon's flooded areas in advance |

Mastery applies to bricks and mortar only (double output at 10 and 50, timer at 25 and 75, 99 bricks cost no lime). Buildings reset at the monsoon but their foundations persist: rebuilding costs 50%, then 25% after the "Old Foundations" watermark.

**Distilling.** Agave in, spirit out; creosote in, salve out; venom in, something you should not drink out. The still-house is the camp's pharmacy and its bar. Inputs: agave, fruit, herbs, venom, jojoba, fish oil, brine glass bottles. Outputs: tonics (timed buffs, 10 to 30 minutes), spirits (one passive buff at a time, 2 hours), lantern oil, ink, salves (combat consumables). Interactions: Harvesting, Trapping, Fishing, Smelting (bottles) in; combat, every skill (tonics), Cartography (ink) out.

| Level | Recipe | Effect |
|---|---|---|
| 1 | Mash | Intermediate from any fruit or agave |
| 5 | Agave Spirit | Spirit: plus 5% gathering XP |
| 10 | Lantern Oil | Deep Mine entry consumable; sellable |
| 12 | Creosote Ink | Cartography input |
| 15 | Creosote Salve | Combat: heal 20% over 10 seconds |
| 20 | Tonic of Grit | Plus 10% Defense for 15 minutes |
| 25 | Mesquite Bitters | Plus 10% Cooking speed |
| 30 | Cholla Tonic | Plus 10% Prospecting double chance |
| 35 | Saguaro Wine | Spirit: plus 5% crafting XP |
| 40 | Lavender Tincture | Combat: cleanse venom and dust |
| 45 | Clarity Tonic | Plus 10% Marksmanship accuracy |
| 50 | Snake Oil | Weapon coating: venom damage over time |
| 60 | Night-bloom Elixir | Plus 10% Attack |
| 70 | Datura Smoke | Dust spirits lose evasion for the fight |
| 80 | Moon Wine | Spirit: plus 8% all XP |
| 90 | Storm Tonic | Plus 15% attack speed for 10 minutes |

Mastery: 10, 5% double; 25, bottles returned at 25%; 50, tonic duration plus 20%; 75, minus 15% timer; 99, a tonic also applies at half strength to the Hired Hand's action.

**Cooking.** The campfire is where the day ends. Food is how you survive a fight, and jam is how you survive a dry season with your spirits intact. Inputs: meat, fish, eggs, fruit, flour, nuts, salt. Outputs: healing food, rations (offline combat food), jams (long buffs to Vigor regen). Interactions: Harvesting, Trapping, Fishing in; combat, Trapping bait, ledger requests out.

| Level | Recipe | Heal |
|---|---|---|
| 1 | Roast Quail | 40 |
| 5 | Mesquite Flatbread | 60 |
| 10 | Prickly Pear Jam | Regen buff |
| 12 | Fried Minnows | 70 |
| 15 | Quail Egg Hash | 90 |
| 20 | Jackrabbit Stew | 120 |
| 30 | Saguaro-fruit Jam | Regen buff tier 2 |
| 35 | Catfish Fry | 180 |
| 40 | Javelina Roast | 240 |
| 45 | Cholla Bud Salad | 200, plus venom resist |
| 50 | Trail Rations | Offline food, 150, stacks tall |
| 60 | Venison Jerky | 320 |
| 65 | Bass in Agave Glaze | 400 |
| 80 | Pronghorn Feast | 560 |
| 90 | Stormfish Chowder | 720, plus attack speed |

Mastery: 10, 5% double; 25, no burning (removes the 15% burn roll); 50, plus 10% heal; 75, minus 15% timer; 99, perfect: 10% chance of a double-heal version.

### 4.5 Combat skills

**Attack, Defense, Marksmanship** are trained by fighting; the active style decides which gets XP (Attack or Marksmanship) and Defense trains in both at a third rate, or fully in a defensive stance that lowers damage 20%. Attack governs melee accuracy and damage with blades and clubs. Marksmanship governs ranged accuracy and damage with the bow, the sling, and thrown knives, and consumes ammunition from the Bandolier slot. Defense governs armor efficiency and evasion and gates armor tiers. Each has ten unlockable passive perks at 10, 20, 30, 40, 50, 60, 70, 80, 90, 99 (for example Attack 40: Follow-through, 10% chance of a second strike; Marksmanship 60: Recovery, 25% of ammunition returned; Defense 80: Set Stance, set bonuses count as one piece higher). Mastery for combat is per-monster: each kill adds monster mastery that raises damage against that monster up to plus 15% and reveals its loot table at 25.

### 4.6 Support skill

**Cartography.** Nobody mapped this country because nobody came back, and the maps the company left are wrong on purpose. You survey one landmark at a time until the land stops surprising you. Inputs: Parchment, Creosote Ink, Fieldstone (markers). Outputs: area unlocks, travel time reduction (minus 1% per level, floor 40%), landmarks (permanent small bonuses: "Shortcut to the Seam" minus 20% Copper Wash travel; "Padre's Spring" plus 5% Harvesting in the Mission; "Vein Chart" plus 2% Prospecting double), and Old Maps that persist through monsoons as a discount on re-surveying. Actions: twenty surveys, three per area at levels 1, 5, 10 (Flats), 10, 15, 20 (Wash), 20, 26, 32 (Mission), 32, 38, 45 (Salt), 45, 52, 60 (Mine), 60, 68, 78 (Mesa), 78, 88, 99 (Pass), each a long timer (20 to 60 seconds) that must be performed in the area. Interactions: Leatherwork and Distilling in; everything out.

### 4.7 Tools and the six tiers

Every gathering, crafting, and support skill has one tool with six tiers, bought at the Trading Post with coin plus a smithed head from Silversmithing (tier 1 is given free). Tiers: minus 0, 5, 10, 15, 20, 25% action timer, plus a tier-specific bonus at tier 6.

| Skill | Tier 1 | Tier 2 | Tier 3 | Tier 4 | Tier 5 | Tier 6 (bonus) |
|---|---|---|---|---|---|---|
| Prospecting | Rock Hammer | Copper Pick | Iron Pick | Silvered Pick | Skyiron Pick | Moonsteel Pick (gems plus 5%) |
| Harvesting | Flint Flake | Copper Sickle | Iron Hook-knife | Silvered Pruner | Skyiron Machete | Moonsteel Harvest Knife (seeds plus 5%) |
| Woodcutting | Hand Axe | Copper Hatchet | Iron Felling Axe | Silvered Axe | Skyiron Axe | Moonsteel Axe (charcoal plus 25%) |
| Fishing | Ocotillo Cane | Corded Mesquite Rod | Iron-reel Rod | Silvered Rod | Skyiron Rod | Moonlit Rod (moonfish anywhere) |
| Trapping | Snare Loop | Copper Snare | Iron Jaw Trap | Silvered Trap | Skyiron Trap | Moon-wire Trap (trophy plus 2%) |
| Smelting | Clay Crucible | Copper-banded Crucible | Iron Crucible | Silvered Crucible | Skyiron Crucible | Moonsteel Crucible (charcoal minus 1) |
| Silversmithing | Stone Hammer | Copper Hammer | Iron Hammer | Silver Chasing Hammer | Skyiron Hammer | Moonsteel Hammer (etchings plus 2%) |
| Leatherwork | Bone Awl | Copper Awl | Iron Awl | Silvered Awl | Skyiron Awl | Moonsteel Awl (plus 2 armor) |
| Masonry | Flat Stone | Copper Trowel | Iron Trowel | Silvered Trowel | Skyiron Trowel | Moonsteel Trowel (bricks double at 10%) |
| Distilling | Clay Pot Still | Copper Still | Iron-banded Still | Silver Still | Skyiron Still | Moonglass Still (duration plus 10%) |
| Cooking | Campfire Spit | Copper Pot | Iron Skillet | Silvered Pan | Skyiron Oven | Moonsteel Range (heal plus 5%) |
| Cartography | Shadow Stick | Copper Compass | Iron Sextant | Silvered Compass | Skyiron Transit | Moonglass Lens (travel floor 30%) |

Tool tiers persist through the monsoon as one of the first watermark families (Section 8), because losing a Moonsteel Pick is the single most hated kind of reset in the comps.

## 5. Combat

**Stats.** Vigor (hit points, from Defense level and gear), Attack rating and Marksmanship rating (accuracy), Strength bonus and Draw bonus (damage multipliers for melee and ranged), Armor (damage reduction), Evasion (hit avoidance), Speed (attack interval), and two resistances (Venom, Dust). Enemies carry the same sheet plus a family tag.

**Gear slots (11).** Weapon, Off-hand (shield, quiver-mate, or lantern), Hat, Coat, Gloves, Boots, Belt, Cloak, Bandolier (ammunition), Ring, Pendant. Tools are not gear; the active tool is implied by the action.

**Armor classes.** Woven (light; plus Marksmanship accuracy, low armor, high evasion), Hide (medium; balanced; set-bonus friendly), Riveted (heavy; high armor, minus 15% Marksmanship accuracy, minus 10% evasion). Defense level gates each piece; Smelting and Leatherwork gate crafting them.

**Formula shapes (constants live in `formulas.ts` and are tuned by the simulator).**

- Hit chance: ratio form, `acc / (acc + eva)` with both ratings scaled by level and gear, clamped to 5% to 95%. The ratio shape keeps gear upgrades meaningful without a hard wall.
- Damage: a uniform roll between a weapon's floor and ceiling, multiplied by `1 + strength_bonus / k`, then reduced by armor multiplicatively with diminishing returns, `dmg × (1 − armor / (armor + c))`, so armor never reaches immunity and late enemies do not need absurd numbers.
- Family modifiers: flat multipliers (silver against Dust Spirits 1.5x; blunt against Caliche Kin 1.3x; venom immunity for Salt Things), applied last so they are legible in the tooltip.
- Vigor regen: percentage per 10 seconds outside combat; inside combat only food, salves, and set effects heal.

**Enemy archetypes.** Swarm (fast, weak, numerous, rewards area damage from the Thunderbird set), Brute (slow, heavy, punishes low armor), Skirmisher (ranged, high evasion, punishes Riveted), Shade (dust spirits; evasion, weak to silver and Datura Smoke), Husk (caliche; armor, weak to blunt), Construct (armor, no food drops, drops parts for the Workshop), Boss (phases, a telegraphed mechanic you prepare for with a tonic rather than react to).

**Structure.** Areas hold open fights (choose a monster, fight until retreat; infinite). Monsters have levels 1 to 120. Dungeons are fixed sequences of 6 to 12 fights ending in a boss, with an entry cost (Lantern Oil for the Mine, a Mission Bell ring for the Ossuary), a first-clear reward, and a repeatable loot table. Raid tiers (the Rookery, the Far Gate) are long multi-phase sequences of 20 to 30 fights with two bosses, gated by a Cartography survey and a full 4-piece set, with their own tables. The ten dungeons at 1.0: Smelter's Ruin, Coyote Hollow Den, the Ossuary, Padre's Cellar, the Saltwalker's Seat, the Foreman's Office, Shaft Nine Bottom, the Lightning Field Walk, the Narrows, and the Drowned Gallery (flooded only). Raid tiers: the Rookery (Thunderbird) and the Far Gate (the Tall Walker).

**Loot philosophy.** Every monster always drops its family material and a coin range. Each family has one rare at 1 in 200 to 1 in 500. Dungeon uniques use a visible pity counter (guaranteed by the Nth clear; the number is shown on the dungeon panel). No drop is worth less than the time to get it, which the linter checks as coin-per-hour against the sell table. Boss first clears grant a recipe or a landmark, never just an item.

**Food, tonics, and consumables.** One food slot (auto-eat at a threshold you set), one tonic slot plus a second from the Still-house, one spirit, one salve stack, one ward (adobe ward: absorbs one boss mechanic). Food heals flat amounts; tonics buff percentages; wards are the "bring the right thing" layer for bosses.

**Death.** You are knocked down, wake at the Campfire, lose 10% of carried coin (capped at one hour of average income, shown before you enter), keep every item, and combat stops. The Mission Wall halves it. Offline you never die (Section 3.5).

**Auto-combat rules.** Auto-eat threshold, auto-retreat threshold, auto-tonic (re-apply when expired), auto-loot with a junk filter, repeat dungeon toggle, "stop on rare drop" toggle, and a target-list queue (fight monster A until mastery 25, then B).

**Four armor sets (Hide unless noted).**

| Set | Pieces | 2-piece | 4-piece | Flavor |
|---|---|---|---|---|
| Rattler-hide (Leatherwork 25) | Vest, Boots, Gloves, Hood | Plus 6% evasion | Hits apply venom: 5% of damage over 6 seconds | Still warm. Still angry. |
| Padre's Vestments (Woven; the Ossuary) | Stole, Robe, Sandals, Cord | Tonics last 20% longer | Once per fight, survive a killing blow at 10% Vigor | He left in a hurry and the garden kept growing. |
| Brine-crusted Plate (Riveted; the Saltwalker's Seat) | Helm, Coat, Greaves, Gauntlets | Plus 12% armor | Reflect 10% of damage taken; immune to tonic drain | Every step leaves a white print that will not wash out. |
| Storm-feather (the Rookery) | Cloak, Hood, Gloves, Boots | Plus 8% attack speed | Every third hit chains to a second enemy in the sequence for 40% | It is not that the feathers are light. It is that they want to be up there. |

## 6. Economy

**Currencies.** Coin (earned by selling, found in fights; the main currency). Scrip (earned only from ledger requests; spent on caravan exclusives: recipes, cosmetic titles, tool heads above tier 4). Silt (earned only at a monsoon; spent on watermarks). No currency is purchasable.

**The shop (the Trading Post).** Tool tiers, bank slots, basic materials at a 4x markup for anyone who refuses to gather (a convenience, never cheaper than gathering), ammunition, lantern oil, seeds. Prices use a rolling markup: each purchase of a stackable raises its price 2%, decaying back over 20 hours, so the shop cannot replace a skill. Caravan stock rotates every 20 hours with two "wanted" items at 1.5x sell price.

**Pricing philosophy.** Sell price is proportional to the labor seconds in the item: a gathered item sells for its base timer in coin times a tier factor from 1.0 to 1.5; a crafted item sells for the sum of its inputs plus 20% for the craft time. Buy price is 4x sell. This keeps every production chain profitable and no step strictly dominant.

**Sinks per material family.**

| Family | Sinks |
|---|---|
| Ores and ingots | Smelting, tool heads, weapons and rivets, Masonry fittings, Chapel offerings |
| Stone, caliche, lime | Bricks and mortar, every building, landmark markers, rebuilds after each monsoon |
| Logs | Charcoal (the largest), beams, arrow shafts, rods, Smokehouse fuel at build |
| Fibers and cotton | Cord, Woven armor, parchment, bandoliers |
| Hides | Armor, packs, tool grips, parchment |
| Meat, fish, eggs, fruit | Cooking, bait, ledger requests, mash |
| Herbs and flowers | Tonics, ink, salves, ledger requests |
| Gems | Jewelry, etchings, landmark "sightings," offerings (high value) |
| Feathers, venom, parts | Ammunition, snake oil, Workshop upgrades |

The Chapel accepts any material as an offering for ledger reputation at diminishing returns per day, which gives every item a floor sink without making it the best use of anything.

**Inflation controls.** Tool and bank costs scale geometrically; shop markup decays rather than resets; coin drops from monsters grow with level more slowly than sell values grow with tier; the Mission Wall and the Observatory are late coin sinks in the hundreds of thousands; the monsoon resets coin entirely, which is the hard ceiling on inflation.

**Bank space.** 40 slots at the start; up to 200 purchasable at geometric cost; plus 6, 10, and 16 from the three pack tiers; the Smokehouse and Granary collapse food into single stacks; two watermarks add 20 each. Equipped gear and tools do not take slots.

**Material flows in words.** Woodcutting feeds charcoal into Smelting, which turns Prospecting's ores into ingots; Silversmithing turns ingots and gems into weapons, tool heads, jewelry, and etchings, while Smelting's rivets go to Leatherwork. Trapping's hides and Harvesting's fibers go to Leatherwork for armor, packs, and parchment; parchment and Distilling's ink go to Cartography, which opens the areas where the higher tiers of every gatherer live. Trapping's meat, Fishing's catch, and Harvesting's fruit go to Cooking for combat food and to Distilling for mash, tonics, and bait. Masonry eats stone, lime, logs, and glass and gives back slots, speed, and sinks. Combat consumes food, tonics, ammunition, and wards, and returns family materials, coin, and the set pieces no crafter can make. The monsoon turns everything left on the table into Silt.

## 7. Progression and pacing

**XP curve.** Classic exponential: XP to the next level grows about 10% per level, doubling roughly every seven levels; level 50 at about 100K cumulative XP, level 99 at about 13M. Action XP grows faster than timers, so higher tiers are always better XP per hour, with the mid tiers best in XP per material.

**Soft caps.** Tool tiers gate by Smelting and coin, not level; areas gate by Cartography; Riveted armor gates by Defense; monsoon cadence gates by the bar. No skill is required above 75 for any unlock until Two Moons Pass, so 75 to 99 is optional depth rewarded by mastery and the collection log, not a wall.

| Milestone | Active player | Idle-heavy player (two check-ins per day) |
|---|---|---|
| First dungeon clear (Smelter's Ruin) | 4 to 6 hours | 2 to 3 days |
| All skills 30 | 10 to 14 hours | 5 to 7 days |
| First monsoon | 25 to 35 hours | 12 to 18 days |
| All skills 50 | 40 to 50 hours | 3 to 4 weeks |
| All skills 99, second cycle or later | 150 to 200 hours | 3 to 5 months |
| 1.0 completion (collection log full, ten monsoons, both raid tiers) | 200 to 300 hours | 5 to 8 months |

**Mid-game loops (hours 20 to 100).** Dungeon sets (farm a set, unlock the next dungeon's mechanic), expeditions (Cartography surveys that need a crafted kit and reward landmarks), ledger chains (recipes locked behind caravan stories), camp building (every building changes the loop), charm etching (the first min-max layer), and monsoon planning (what to carry as a keepsake, what to overfill the bar for).

**Late-game loops (hours 100 and on).** The two raid tiers, Deep Mine depth levels (procedurally deeper floors with rising rewards, reset each monsoon), Dry Spell challenge runs, the collection log's rare page, watermark tier caps that lift at monsoon 3, 6, and 10, and the flooded areas as a fresh map every cycle. The answer to "shallow mid-to-late game" is structural: the monsoon makes the whole game replay faster with new rules, and the late game adds places and modes rather than longer timers.

## 8. Prestige: the Monsoon

**Trigger.** The Monsoon bar fills from "cloud" points: skill levels (1 point each, 5 at every tenth), first dungeon clears (25), buildings (10), landmarks (5), ledger chains (15), weather. 100% is reachable at about 25 to 35 active hours the first time. You may call the rains at 100% or keep playing to overfill up to 300%; Silt scales linearly with the fill, so waiting is a choice, not a trap. A confirmation screen shows exactly what resets, what persists, and the Silt you will receive.

**Resets.** Skill levels (to the watermark floor), mastery (except the retained share), inventory, coin, scrip, equipped gear (except keepsakes), buildings (foundations persist), area unlocks (Old Maps make re-surveying one third the time), dungeon pity counters.

**Persists.** Watermarks and Silt, tool tiers (watermark family 1), keepsakes, achievements, collection log, monster mastery, ledger story progress, settings, statistics, and the Observatory's forecast.

**Watermark upgrades (36 at 1.0, 40 with the two raid-tier families).** Costs in Silt rise by tier; families cap at tier 3 until monsoon 3, tier 4 until monsoon 6, tier 5 at monsoon 10.

| Family | Watermarks |
|---|---|
| Riverbed (XP) | High Water: all XP plus 5% per tier; Floodline: start each cycle at skill level 5/10/15/20/25; Memory of Rain: keep 10/20/30/40/50% of mastery; Old Maps: Cartography surveys cost less per tier; Quick Study: mastery XP plus 10% per tier |
| Floodplain (gathering) | Rich Silt: gathering double chance plus 3% per tier; Full Nets: Fishing and Trapping bait lasts longer; Seed Bank: Harvesting starts with tier-2 plants unlocked; Open Seam: Prospecting nodes never deplete from cycle start; Standing Timber: Woodcutting yields charcoal at 10% |
| Kiln (crafting) | Hot Kiln: crafting speed plus 4% per tier; Spare Rivets: 10% chance to save inputs; Old Foundations: rebuilds cost 50/35/25/15/10%; Signed Work: etchings 1% stronger per tier; Deep Cellar: tonic duration plus 10% per tier |
| Armory (combat) | Thick Skin: Vigor plus 5% per tier; Keen Edge: damage plus 3% per tier; Second Wind: regen in combat 1% per 10 seconds per tier; Pity's Sake: dungeon pity counters shortened 10% per tier; Sure Shot: ammunition returned 10% per tier |
| Trading Post (economy) | Fair Dealer: sell price plus 5% per tier; Long Ledger: ledger scrip plus 10% per tier; Deep Pockets: plus 20 bank slots per tier; Early Caravan: caravan interval minus 2 hours per tier; Prospector's Luck: coin drops plus 10% per tier |
| Cistern (idle) | Deep Cistern: offline bank 8/12/16/20/24 hours; Second Shift: third action slot (one tier); Night Watch: offline speed plus 5% per tier; Steady Hands: Watchfire bonus applies offline at half (one tier); Long Fuse: auto-retreat recovers and resumes after 10 minutes (one tier) |
| Keepsake (retention) | Pack Mule: plus 1 keepsake per tier (max 4); Old Tools: tool tiers persist (tier 1 is the first purchase anyone makes); Heirs: one building survives intact per tier; Weathered: start with the Campfire, Rack, and Cistern built (one tier) |
| Rookery and Far Gate (raid, 4 total) | Storm Sense: raid-tier phases telegraphed longer; Nest Egg: raid uniques pity minus 25%; Moon Road: Two Moons Pass opens at Cartography 70; Far Sight: the Far Gate stays open across one monsoon |

**Flooded areas.** After the rains the map changes for the first 20 active hours of the cycle (the Observatory shows which):

| Flooded area | Replaces | What is there |
|---|---|---|
| The Running River | The Dead Riverbed | Fishing at camp with triple double-chance; river stones for Masonry |
| The Cienega | Agave Flats edge | Rain Lily, bullfrogs, a marsh beast family variant with wetland hides |
| Playa Lake | The White Flat | Bass and gar fishing; the Saltwalker is replaced by the Drowned Pillar, a water boss with its own table |
| The Drowned Gallery | Deep Mine level 3 | Blind cavefish, sunken ore (double skyiron), the Drowned Gallery dungeon |

When the flood recedes it leaves Silt Flats: bonus nodes in each area that pay out double for the next 10 hours. Monsoon 3 adds the Cienega's Heron Rookery, monsoon 6 adds the Flooded Cellars under the Mission, monsoon 10 the Hundred-year Flood, which floods every area at once for 40 hours.

**Cadence targets.** First monsoon 25 to 35 hours; second 12 to 18; third to fifth 8 to 12; sixth to ninth 5 to 8; tenth and later 3 to 5 for an active player. Idle-heavy players take roughly four times as long in calendar days with the same number of check-ins.

**The tenth monsoon.** It stays meaningful because (1) watermark tier caps lift at 3, 6, and 10, so the strongest effects arrive late; (2) the flooded set grows at the same beats; (3) Dry Spell modifiers (Section 9) unlock at monsoon 5 and give each later run a rule; (4) the collection log has a Monsoon page (every flooded item, every drowned boss) that cannot be finished before 10; (5) the Hundred-year Flood is the 1.0 finale, and its first clear is the game's last achievement.

## 9. Achievements, challenges, collections, ledger

**Achievements (150 at 1.0).** Six groups: Skills (level 50 and 99 per skill, 30), Combat (first and tenth clears, raid tiers, 35), Camp (every building, 20), Monsoon (first, third, sixth, tenth, overfills, 20), Collection (pages complete, 25), Oddities (fish in the Mine, roast a dust hare, survive a Haboob with no armor, 20). Steam achievements mirror 50 of them. Rewards are titles and nothing else.

**Dry Spells (challenge runs, unlocked at monsoon 5).** A modifier chosen at the call: No Trading Post; Night Only (the Lantern never sets; dust spirits everywhere); Thin Water (offline cap 8 hours, Silt plus 50%); Lean Years (every timer plus 25%, every drop plus 25%); Iron Rations (no Cooking; food from the caravan only). Each completed Dry Spell adds a permanent 2% to Silt gain and a title.

**Collection log.** Every item, monster, landmark, building, and flooded-only entry, with a per-area completion percentage on the map. Rare entries show their source once seen. The log is the long-tail goal that outlives the watermark list.

**Ledger quests (60 at 1.0).** The caravan master's book records requests: deliver 40 Rattler Hides; clear the Ossuary once; survey the Needle. Chains of three to five requests tell short stories (the company's last payroll, the Padre's letters, who built the Watchfire). Rewards: scrip, recipes that exist nowhere else (Moon Wine, the Large Pack), landmarks, titles. No combat is required for a ledger chain until the Mesa; every chain is finishable idle. Ledger text is generated to a brief, human-edited, and disclosed.

## 10. UI and UX

**Information architecture.** Left rail: skills in three groups (Gathering, Crafting, Combat and Support), with the active action's progress ring on its icon. Top bar: Monsoon bar, coin, scrip, offline indicator, weather, settings. Main panel: the selected screen. Right drawer: bank and equipment, collapsible. Bottom: notification tray.

Screens: Camp (buildings, campfire, hired hand), one Skill screen per skill (action cards with timer, XP, mastery, inputs and outputs, a "best XP" and "best coin" highlight), Combat (area, monster list, your sheet, auto rules, dungeon list with pity counters), Map (areas as a flat illustrated map, surveys, travel), Bank, Trading Post, Ledger, Achievements and Collection Log, Monsoon (bar, watermarks, keepsake picker, forecast), Statistics, Settings.

**Navigation.** Everything reachable in two clicks; keyboard: 1 to 9 for the rail, Tab order follows reading order, Escape closes drawers, slash opens a command palette that searches items, actions, and screens.

**Notifications.** Batched toasts (level-ups coalesce into "Woodcutting 14 to 17"), rare drops pinned until dismissed, offline summary as a modal once. Numbers: thousands separators to 999,999, then 1.25M, 1.25B, 1.25T, with an option for full digits or scientific. Timers show seconds to one decimal under 10 seconds, whole seconds above.

**Settings.** Auto-eat and auto-retreat defaults, notification density, number format, theme (ochre day, dusk, high contrast), font size 90 to 160%, reduced motion, colorblind palette (deuteranopia, protanopia, tritanopia modes change accents and add shape markers), screen reader verbosity, cloud save and export to file, telemetry opt-in, mod folder.

**Accessibility.** Full keyboard operation; every icon has a text label and an `aria-label`; live regions announce level-ups at a chosen rate; reduced motion stops the progress rings and replaces them with a percentage; focus rings are always visible; colors never carry meaning alone (rarity uses an icon frame shape as well); minimum text 14 px at 100%; axe checks in CI.

**Narrow window and Steam Deck.** Below 900 px the rail collapses to icons and the drawer becomes a tab; below 600 px screens stack vertically and the bottom bar becomes a five-tab nav. Steam Deck (1280 by 800) uses the collapsed rail, 125% text, gamepad mapping (bumpers switch skills, triggers switch screens, face buttons act on the focused card), and a 32 px minimum hit target. The legacy single-column design informs the stacked layout.
