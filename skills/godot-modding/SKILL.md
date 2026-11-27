---
name: godot-modding
description: Adds mod support and user-generated content to Godot 4 strategy and management games - data-driven mods (JSON/CSV/Resources), resource packs (PCK/ZIP) via ProjectSettings.load_resource_pack, mod loading order and conflicts, mod manifests, Steam Workshop through GodotSteam UGC, custom maps and scenarios, and mod security. Use when the user asks about modding, Workshop, user maps, DLC/patch packs, or making the game easier to extend.
---

# Godot Modding

## Overview

Strategy and management games live longer with mods (RimWorld and Frostpunk 2 on PC are examples of games where the Workshop is a major part of the community). The cheapest path is designing the game data-driven from the start; mod support then means loading more data.

## Levels Of Mod Support (choose deliberately)

1. **Data mods** (safest, recommended first): new/changed buildings, resources, events, maps as JSON/CSV or `.tres` files loaded from a mods folder. No code execution.
2. **Asset mods**: textures, models, audio, translations.
3. **Resource packs** (PCK/ZIP) via `ProjectSettings.load_resource_pack()`: can add scenes and scripts; powerful but can run arbitrary code.
4. **Script mods**: full code access. Highest risk; only with clear warnings.

## Architecture

- All content registries (`BuildingDB`, `EventDB`, `MapDB`) load base content, then mods in order, by stable ids.
- Mod manifest (`mod.json`): id, name, version, game version range, dependencies, load order hints, description.
- Conflict policy: later mods override by id; log conflicts; show them in a mod manager screen.
- Validate mod data at load; skip broken entries with clear errors instead of crashing.
- Saves record active mods and versions; warn on load when they differ.

## Resource Packs

- Load packs as early as possible: from an autoload's `_init()`, not `_ready()`, so preloaded scenes see the overrides.
- Packs replace files with the same path by default; pass `false` as the second argument to `load_resource_pack()` to avoid overriding existing files, or keep mods in their own `res://mods/<id>/` folder.
- Modders need a matching Godot version to export packs; publish a mod SDK project (base folder structure, example mod, docs).

## Steam Workshop

- Use GodotSteam's UGC functions (create item, set content/preview/title, submit update; query subscribed items and their install folders). Check the GodotSteam docs for exact function names in your version.
- On startup, read subscribed items' install folders and pass them to the same mod loader as local mods.
- Workshop requires enabling UGC in Steamworks and accepting the Workshop legal agreement flow for uploaders.

## Security

- Data mods: parse with strict schemas; never `eval`/execute strings from mods.
- PCK/script mods can contain malicious code; tell players clearly, let them disable code mods, and keep code-mod loading opt-in.
- Never load mods from paths outside the user's mods folder or Workshop install folders.

## Checklist

- [ ] Content loaded through registries by id
- [ ] Mod manifest + mod manager UI (enable/disable/order)
- [ ] Load errors shown in-game, not crashes
- [ ] Saves store mod list
- [ ] Example mod + modding docs shipped
- [ ] Workshop upload/download tested in an exported build
