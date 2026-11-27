---
name: godot-procedural-worlds
description: Builds maps and worlds for Godot 4 strategy and management games - square and hex grids, GridMap, heightmap terrain, FastNoiseLite generation, biomes, resource placement, rivers and coasts, seeds, scenario maps, map editors, and map size/scale planning. Use when the user asks for a new map, random or seeded generation, terrain, a grid system, buildable areas, or wants to plan map size before coding.
---

# Godot Procedural Worlds

## Overview

The map is the main stage of a strategy game. Decide scale, grid type, and generation approach before building systems on top, because placement, pathfinding, economy, and performance all depend on it.

## Decide First (write in DESIGN.md)

- Grid: square (city builders, colony sims), hex (4X, tactics), or gridless/organic with a hidden placement grid.
- Cell size in meters and map size in cells; estimate late-game object counts from it.
- Authored maps, seeded procedural maps, or hybrid (authored templates + procedural details).
- Vertical: flat with height for looks only, or real height that affects building (terraces, water flow).

## Logical Grid Vs Visuals

- Keep a logical grid in the simulation: arrays/`PackedInt32Array` for terrain type, height, fertility, resource, occupant id.
- Visuals derive from the grid: terrain mesh or `GridMap` tiles, `MultiMeshInstance3D` for trees/rocks/crops.
- Hex grids: axial coordinates (q, r) in data; convert to world position only for rendering; use a neighbor table for pathfinding and adjacency.

## Generation Pipeline (seeded)

1. Seed one `RandomNumberGenerator` and the `FastNoiseLite.seed` from the map seed; store the seed in the save.
2. Height: layered noise (FBM) + falloff toward edges for islands or a basin shape.
3. Moisture/temperature noise → biomes (lookup table).
4. Water: sea level threshold; rivers by following downhill from high points.
5. Resources: placement by biome with minimum spacing (Poisson-disk style) and fairness guarantees near the start (e.g. at least N food, wood, stone within radius R).
6. Validation pass: start area buildable, all key resources reachable, no isolated starting positions in multiplayer/4X. Regenerate with a new sub-seed if validation fails.
7. Decoration: trees, flowers, rocks as MultiMesh instances, with density from biome and distance to buildings.

## Performance

- Chunk large maps (e.g. 32×32 cells per chunk) for meshes, MultiMesh, and navigation regions; rebuild only changed chunks.
- Build terrain meshes with `SurfaceTool` off the main thread (`WorkerThreadPool`) and apply on the main thread.
- Use visibility ranges for small decorations; consider 4.8+ texture streaming for large worlds.

## Map Editor And Scenarios

- An in-game or editor tool to paint terrain/resources onto the logical grid; save maps as Resources or JSON.
- Scenarios = map + starting state + goals + modifiers (data Resources); reuse for tutorials and challenge maps.

## Anti-Patterns

- Generation logic mixed into rendering code
- Unseeded randomness (unreproducible bugs)
- Map size chosen without estimating object counts
- Rebuilding the whole terrain mesh on every building placement

## References

- Snippets: `references/worldgen-snippets.md`
