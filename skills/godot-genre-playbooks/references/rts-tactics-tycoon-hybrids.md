# RTS / Tactics, Tycoon, Roguelite Builder, RPG Hybrids

## RTS / real-time tactics

Core loop: gather → build → produce units → scout → fight → expand.

Design levers: counters (rock-paper-scissors+), map control, macro vs micro balance, pause or slow-mo option for single-player.

Signature problems: APM walls; units clumping and pathing jank; AI that cheats or is passive. Answers: control groups and smart commands, single-player pause; formation movement + flow fields for large groups; AI built on build orders + utility scoring with difficulty via decision quality, not only resource bonuses.

Godot notes: many units → simulation-driven movement on a grid/flow field rather than one NavigationAgent3D with avoidance per unit (RVO avoidance has a significant cost with many agents); use MultiMesh for rendering crowds. See `godot-agents-ai`.

MVP slice: 2 factions, 6 units each, 1 resource, 3 maps, skirmish vs AI.

## Turn-based tactics

Core loop: position → act → react; information and terrain matter.

Levers: clear hit chances, telegraphed enemy intent, few but distinct abilities, undo for misclicks in planning.

## Tycoon / business

Core loop: build attractions/businesses → set prices → serve customers → reinvest; market conditions move.

Levers: customer needs and satisfaction, pricing elasticity, competitors, staff, research, financial tools (loans, upkeep).

Signature problem: solved economy (one optimal layout/price). Answers: shifting demand and trends, scenarios with goals and constraints, customer variety.

MVP slice: 1 venue, 8 build items, 3 customer types, pricing, 1 scenario with 3 goals.

Reference games: Planet Coaster/Zoo, Two Point series, RollerCoaster Tycoon, Game Dev Tycoon.

## Roguelite builder

Core loop: short run with random map/offers → pick from limited choices → survive escalating pressure → meta-progression between runs.

Levers: draft choices (buildings/blueprints offered, not all available), run modifiers, seeds, clear run length (30–90 min), unlocks that add variety rather than only power.

Signature problem: samey runs. Answers: modifiers that change the rules, biome variety, build-defining rare offers.

Reference games: Against the Storm, Dorfromantik, Islanders.

## RPG / strategy hybrids

Patterns: heroes/leaders with levels and gear leading armies; settlement management feeding an expedition layer; characters with relationships inside a management game.

Rule: one layer is primary, the other supports it. Decide which (e.g. colony funds expeditions). Make the layers exchange resources both ways so neither is a side-game.

Reference games: Heroes of Might and Magic, Mount & Blade, Battle Brothers, Dungeon Settlers (colony funds dungeon expeditions).
