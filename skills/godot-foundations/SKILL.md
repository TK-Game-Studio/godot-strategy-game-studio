---
name: godot-foundations
description: Sets and protects Godot 4 project architecture. Use when the task involves scene tree structure, autoloads/singletons, game state ownership, data Resources, signals, the input map, the simulation tick, save/load and save migration, debug overlays, performance budgets for large colonies/cities, or upgrading between Godot versions.
---

# Godot Foundations

## Overview

Godot projects decay fast when game rules live inside visual nodes, UI scripts poke into each other, and save data is scattered. Establish these boundaries early and protect them on every change.

Default rule: game state lives in plain data (Resources, typed arrays/dictionaries, or a simulation module); scenes only display and animate it; UI talks to the game through signals or a small manager API.

## Version Awareness

- Read the engine version first (`godot --version` / `config/features` in `project.godot`) and stay within its APIs.
- Notable defaults since 4.6: new projects use Jolt for 3D physics; Windows defaults to Direct3D 12; glow runs before tonemapping (old scenes may look different); mouse/touch focus is separate from keyboard/gamepad focus.
- Before upgrading minor versions: commit to git, read the official migration guide, compare before/after screenshots (lighting/glow), and re-run tests.

## Architecture Rules

1. Separate simulation from presentation.
   - Simulation owns: stock, population, buildings and levels, jobs, timers, calendar/seasons, threats, unlocks, RNG seed.
   - Presentation owns: meshes, animation, particles, camera, audio, UI widgets.
   - Visual nodes read data and react to signals; they never decide rules.
2. Few, clear autoloads: typically `GameState` (or `Sim`), `Events` (signal bus), `SaveManager`, `Settings`. Not every script becomes an autoload.
3. Content as data: building types, recipes, tiers, needs, events, difficulty presets are `Resource` classes (`class_name BuildingDef extends Resource`) saved as `.tres` under `res://data/`. Adding a building = adding a `.tres`.
4. Signals for change notification: `stock_changed`, `building_placed`, `day_passed`, `tier_reached`. UI connects; no per-frame polling.
5. Explicit input: actions in the Input Map (`camera_pan`, `rotate_building`, `cancel`, `pause`, `speed_up`, plus gamepad bindings). World/camera input in `_unhandled_input` so UI consumes events first.
6. Use typed GDScript (`var stock: Dictionary[StringName, int]`, typed function signatures) and enable the "untyped declaration" warning; it catches agent mistakes early.
7. Deterministic simulation where practical: one seeded `RandomNumberGenerator` owned by the simulation, so bugs and balance tests are reproducible.
8. Save serializable state only (see `references/save-system.md`): ids, type keys, grid positions, numbers. Rebuild visuals on load. Version every save and migrate.
9. Debug tools are toggleable: an F3 overlay (FPS, frame time, draw calls via `Performance.get_monitor`, node count, key sim numbers) and cheat keys gated by `OS.is_debug_build()`.

## Simulation At Scale (management games)

- Run the simulation on a fixed tick (e.g. 4–10 ticks/sec scaled by game speed), not in every node's `_process`.
- Keep many agents (villagers, carts) as data rows; only nearby/visible ones need full nodes and animation.
- Spread expensive work across ticks (pathfinding batches, job assignment rounds).
- Cache derived values (total housing, production rates) and update them on change signals.
- See `references/simulation-scaling.md` for patterns.

## Suggested Folder Layout

```
res://
  autoload/      GameState.gd, Events.gd, SaveManager.gd, Settings.gd
  data/          *.tres definitions (buildings, tiers, needs, events, difficulty)
  scripts/       Resource class definitions, shared helpers
  sim/           pure simulation modules (economy, population, jobs, threats)
  world/         map, grid/terrain, camera, building scenes
  ui/            HUD, menus, tooltips, theme
  art/           meshes, materials, textures, shaders
  audio/
  tests/         GdUnit4 tests and smoke scenes
  DESIGN.md      fantasy, verbs, systems, decided-against list
```

## Checklist Before Writing A New System

- What data does it add, and where does it live?
- What signals does it emit?
- What does it save, and how do old saves load without it?
- What does the player see always / on hover / on demand?
- Per-tick cost with a late-game save (e.g. 300+ buildings, 200+ agents)?
- How is it tested (unit test, debug cheat, playtest step)?

## Anti-Patterns

- Game rules in `_process` of visual nodes
- UI reaching into world nodes via long `get_node("../../..")` paths
- Saving node paths or object references
- Hardcoded stats instead of data Resources
- One giant script that does everything
- Unseeded randomness in simulation
- Reintroducing removed mechanics during refactors

## References

- Save system: `references/save-system.md`
- Simulation scaling: `references/simulation-scaling.md`
