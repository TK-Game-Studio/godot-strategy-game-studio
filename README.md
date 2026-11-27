# Godot Strategy Game Studio

A Claude plugin for building strategy and management games in Godot 4 (city builders, colony sims, survival builders, 4X, grand strategy, RTS, tycoon, roguelite builders), from first idea to Steam release. Adapted from OpenAI's Game Studio plugin (originally for browser games with Phaser / Three.js) and expanded with genre research and 2026–2027 Steam market data.

## Install

Claude Code:

```
/plugin marketplace add TK-Game-Studio/godot-strategy-game-studio
/plugin install godot-strategy-game-studio@godot-strategy-game-studio
```

Claude app (claude.ai / desktop): download this repository as a zip and add it as a plugin, or install it from the Claude directory once listed.

## What it does

This plugin does not run any code, send data anywhere, or connect to any server. It only contains instruction files (skills) and reference notes that Claude reads when you work on a Godot game.

## Skills

| Skill | What it does |
|---|---|
| godot-game-studio | Entry point: design workflow, hook, routing to the others |
| godot-genre-playbooks | Playbooks for city builder, colony sim, survival, 4X, grand strategy, RTS, tactics, tycoon, roguelite builder, RPG hybrids |
| godot-foundations | Architecture: autoloads, data Resources, signals, input, simulation tick, save/load, performance, upgrades |
| godot-economy-balance | Production chains, costs, sinks, difficulty curves, spreadsheet and headless balance runs |
| godot-agents-ai | Villager needs, utility AI, job boards, AStarGrid2D/navmesh/flow fields, crowds, AI opponents |
| godot-procedural-worlds | Square/hex grids, seeded terrain and biomes, resource placement, chunks, scenarios, map editors |
| godot-events-narrative | Event director/storyteller, data-driven events, laws, goals, chronicles, endings |
| godot-game-ui | HUD, build menu, tooltips, menus, Theme, gamepad/Steam Deck, accessibility, localization |
| godot-3d-assets | Code-built meshes and GLB imports, stylized lighting/materials, LOD and MultiMesh |
| godot-audio | Adaptive music, reactive ambience, UI sounds, buses and settings |
| godot-modding | Data mods, PCK packs, mod manager, Steam Workshop, mod security |
| godot-playtest | Headless checks, GdUnit4 tests, smoke and balance runs, Steam Playtest, reports |
| godot-marketing | Hook, devlogs, short video, creators, community, announcement beats |
| godot-steam-release | Store page, demo, Next Fest, GodotSteam, export, Deck, AI disclosure |

## Requirements

- Godot 4.5 or newer recommended (written against 4.7 stable / 4.8 beta)
- GDScript projects; C# works but examples are GDScript
- Optional: a Godot MCP server for live editor access, GdUnit4 for tests, GodotSteam for Steamworks

## Version

0.3.0 (November 2026). Market figures in `godot-steam-release/references/steam-market-2027.md` are dated; re-check before relying on them.

## License

Based on openai/plugins `game-studio` (MIT License, © OpenAI). This adaptation is also MIT.
