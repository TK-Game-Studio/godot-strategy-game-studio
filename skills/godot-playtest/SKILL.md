---
name: godot-playtest
description: Runs structured playtests, automated tests, and QA on Godot 4 games. Use when the user asks to test the game, check a build, find bugs, write or run unit tests, review gameplay screenshots, run smoke or balance tests, verify a feature works, or reports what they noticed while playing.
---

# Godot Playtest

## Overview

Test the game the way a player experiences it: boot, first minutes, main verbs, UI readability, pressure states, save/load, long sessions, and performance. Combine automated checks with structured review of what the user reports.

## Preferred Workflow

1. Import + static check (fast, every change):
   - First run or after asset changes: `godot --headless --path <project> --import`
   - Then: `godot --headless --path <project> --quit-after 300` and scan output for `SCRIPT ERROR`, `Parse Error`, `ERROR:`.
2. Unit tests for simulation logic: use GdUnit4 (v6 requires Godot 4.5+; it has a CLI and a GitHub Action for CI). Test pure sim modules: production rates, needs, tier thresholds, save round-trip, difficulty presets. Install it into `addons/` only with the user's agreement.
3. Smoke scenes: `res://tests/smoke_*.tscn` that place buildings, advance time N ticks, save, load, and assert on state; exit with `get_tree().quit(1)` on failure. Run headless.
4. Balance runs: headless scripted runs with a fixed RNG seed that advance many in-game days and log key curves (stock, population, happiness) to CSV for review. Compare across difficulty modes.
5. Screenshot review: capture representative states (start, busy mid-game, late game, menu open, night/threat) via a debug key using `get_viewport().get_texture().get_image().save_png("user://shots/…png")`, or via a Godot MCP server / screen access if available. Judge readability, UI weight, and visual identity from the images.
6. External playtests: Steam Playtest (free) or a small closed beta; add an in-game feedback button (main menu + pause) that opens a form or Discord, and attach version + seed to reports. Watch players without helping when possible; note where they hesitate.
7. Guided manual pass: when automation can't cover it, give the user a short numbered script (what to do, what they should see) and ask for notes.
8. Report findings in severity order.

## What To Check

### Every build
- boots to the first playable screen without errors
- new game in each difficulty/mode starts correctly
- main verbs: place, rotate, cancel, select, inspect, upgrade, demolish
- pause really stops simulation; speeds work
- save → quit → load restores the same state; an old save from the previous build still loads

### Management systems
- no unintended negative stock; numbers match tooltip claims
- tier/upgrade requirements visible and reachable
- pressure/threat frequency matches difficulty
- no soft-locks on default difficulty
- late game: is there still a meaningful decision every few minutes?

### 3D world and camera
- camera limits, no clipping under terrain, camera input stops under menus
- placement preview matches final position/footprint
- silhouettes readable at default zoom; no z-fighting or missing materials

### UI
- center clear, tooltips everywhere, no click-through
- holds at 1280×720, 1280×800, 2560×1440; readable on Steam Deck
- full gamepad path

### Performance
- FPS and sim tick time with a stress save (hundreds of buildings/agents)
- no frame-time growth over a 30–60 minute session (leaks)
- draw calls and node count on the debug overlay
- minimum-spec / Steam Deck check before demo release

### Feel
- first 5 minutes: always an obvious next goal?
- anything startling instead of informative (sounds, flashes)?
- moments of waiting with nothing to do?

## Reporting Standard

Lead with findings. For each: what the player sees, how to reproduce, severity (blocker / major / minor / polish), likely owner (simulation, world/visuals, UI, assets, save system). Then list what passed and the single most important fix next.

## References

- Full checklist: `references/playtest-checklist.md`
