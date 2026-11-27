# 4X and Grand Strategy

## 4X (explore, expand, exploit, exterminate)

Core loop: explore map → settle/claim → develop economy and tech → contest rivals → reach a victory condition.

Pacing arc: exciting early expansion → deepening mid-game competition → late game that must be a climax, not a slog.

Signature problems:
- Snowball: an early lead (player or AI) becomes unstoppable, so the outcome is decided long before the game ends.
- Late-game drag: empire size multiplies micromanagement; the interface built for the early game collapses under late-game numbers.

Answers used by the genre:
- Late-game scenarios/crises that challenge the leader (e.g. Stellaris-style crises, fallen empires).
- Era/age transitions that reshuffle the board.
- Victory conditions woven into the whole game, not only the final era.
- Automation and delegation (governors, sectors, auto-explore) as the empire grows.
- Diminishing returns on size (upkeep, unrest, admin capacity).
- Fewer, more meaningful units/cities instead of hundreds of tiles to tweak.
- Catch-up mechanics for trailing AIs/players, transparently explained.

MVP slice: small map (6–8 regions per player), 2 AI rivals, 1 victory condition, 3 resources, 1 tech tree of ~15 nodes, turn-based.

Godot notes: hex or square grid via `godot-procedural-worlds`; turn processing as pure simulation (easy to test headless); AI rivals via `godot-agents-ai` (utility scoring).

Reference games: Civilization series, Old World, Humankind, Stellaris, Endless Legend.

## Grand strategy

Core loop: steer a realm over decades/centuries; manage characters, diplomacy, economy, war; history emerges.

Design levers:
- Emergent narrative from systems + autonomous characters with personalities + a light layer of scripted events (Crusader Kings approach).
- Transparent systems: opinion modifiers, tooltips that break down every number.
- Pausable real time with speed controls.
- Map as the main UI (map modes for political, economic, religious views).

Signature problems: opacity (players don't know why), blobbing (map painting), overwhelming onboarding. Answers: breakdown tooltips everywhere; coalitions/overextension; guided start, focused first goals.

MVP slice: one region, 1 playable realm + 4 AI realms, characters with 4–5 traits, 20 events, diplomacy (alliances, war, peace), 50 years of play.

Reference games: Crusader Kings II/III, Europa Universalis, Victoria, Imperator.
