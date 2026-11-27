# Godot Strategy Game Studio

A Claude plugin for building strategy, management, city-builder, and colony-sim games in Godot 4, from first idea to Steam release. Adapted from OpenAI's Game Studio plugin (originally for browser games with Phaser / Three.js) and expanded with genre research and 2026–2027 Steam market data.

## Install

Claude Code:

```
/plugin marketplace add tuges77/godot-strategy-game-studio
/plugin install godot-strategy-game-studio@godot-strategy-game-studio
```

Claude app (claude.ai / desktop): download this repository as a zip and add it as a plugin, or install it from the Claude directory once listed.

## What it does

This plugin does not run any code, send data anywhere, or connect to any server. It only contains instruction files (skills) and reference notes that Claude reads when you work on a Godot game.

## Skills

| Skill | What it does |
|---|---|
| godot-game-studio | Entry point: design workflow, hook, genre patterns, routing to the others |
| godot-foundations | Architecture: autoloads, data Resources, signals, input, simulation tick, save/load, performance, version upgrades |
| godot-game-ui | HUD, build menu, tooltips, menus, Theme, gamepad/Steam Deck, accessibility, localization |
| godot-playtest | Headless checks, GdUnit4 tests, smoke and balance runs, screenshot review, reports |
| godot-3d-assets | Code-built meshes and GLB imports, stylized lighting/materials, LOD and MultiMesh |
| godot-steam-release | Store page, demo, Next Fest, GodotSteam, export, Deck, AI disclosure |

## Requirements

- Godot 4.5 or newer recommended (written against 4.7 stable / 4.8 beta)
- GDScript projects; C# works but examples are GDScript
- Optional: a Godot MCP server for live editor access, GdUnit4 for tests, GodotSteam for Steamworks

## Version

0.2.0 (November 2026). Market figures in `godot-steam-release/references/steam-market-2027.md` are dated; re-check before relying on them.

## License

Based on openai/plugins `game-studio` (MIT License, © OpenAI). This adaptation is also MIT.
