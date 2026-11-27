# City Builder, Colony Sim, Survival / Society Builder

## City builder

Core loop: place housing → workers appear → place production → supply needs → population tiers up → unlock new needs/buildings → expand.

Design levers:
- Layout as the puzzle: adjacency, coverage radii, road logistics, terrain (organic) or zoning (grid).
- Needs tiers: each tier adds goods and services; chains get longer.
- Visible logistics: carriers, carts, queues show bottlenecks in the world.

Signature problem: late-game sprawl. Answers: constraints (land, verticality, one settlement), moving conditions (seasons, prices, events), expansion with purpose (outposts, monuments), session structure.

MVP slice: 1 map, 3 needs tiers, ~15 buildings, 1 pressure source (winter or threats), save/load, 30–60 minutes of play.

Reference games: Manor Lords, Anno series, Banished, Kingdoms and Castles, Timberborn, Tiny Glade (cozy, no goals).

## Colony sim

Core loop: individuals with needs and skills → assign priorities → they act autonomously → events disturb → player adapts.

What makes it a colony sim: named, autonomous individuals with personality whom the player watches over time; the player directs priorities rather than issuing every action.

Design levers:
- Needs and mood thresholds that cause visible behavior (breaks, refusals, celebrations).
- Skills and traits that make people non-interchangeable.
- An event director ("storyteller") that paces threats and windfalls based on colony state (see `godot-events-narrative`).
- Relationships for emergent drama.

Signature problems: repetitive stories; micromanagement as colony grows; agent CPU cost. Answers: trait combinations and rare events; work priorities and zones; simulation LOD (see `godot-agents-ai`).

MVP slice: 3–8 colonists, 5 needs, 6 job types, 1 storyteller with 5 event types, one biome.

Reference games: RimWorld, Dwarf Fortress, Going Medieval, Oxygen Not Included.

## Survival / society builder

Core loop: a looming threat (cold, storm, siege) + scarce resources + society meters (e.g. hope and discontent) + laws/policies that trade comfort for efficiency.

Design levers:
- Two opposing society meters that can both be high or both low, driven by needs, events, and laws.
- Irreversible or costly decisions (laws, edicts) with delayed consequences.
- Escalating threat waves on a known schedule.
- Moral framing: the game comments on the path taken (endings, epilogues).

Signature problem: a cruel-but-optimal dominant strategy. Answers: make extreme paths carry real long-term costs; reward balanced paths with unique options; reflect choices in the ending.

MVP slice: 1 scenario of 1–2 hours, 2 society meters, 8–12 laws, 3 threat waves.

Reference games: Frostpunk, Frostpunk 2, Against the Storm (storm cycles), They Are Billions.
