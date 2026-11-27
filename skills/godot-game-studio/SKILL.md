---
name: godot-game-studio
description: Entry point for Godot 4 game work, especially strategy, management, city-builder, colony-sim, and cozy builder games. Use when the user says "help me build/design a game", plans a new feature or system, asks what to work on next, says the game "isn't fun yet" or "looks weak", or when a request spans design, code, UI, assets, testing, and release at once. Routes to the specialist godot-* skills.
---

# Godot Game Studio

## Overview

Keep design, architecture, UI, assets, testing, and release decisions in one coherent plan, then hand off to the most specific specialist skill. Target: Godot 4 with GDScript, desktop first, Steam as the main storefront.

Assume the user may not be a programmer. Explain decisions in plain language, make code changes directly in the project, and never ask the user to hand-edit scripts unless there is no alternative. Reply in the user's language.

## First Steps In Any Project

1. Check the Godot version: run `godot --version` (or read `config/features` in `project.godot`). Use only APIs that exist in that version. When features from a newer minor version would help, say so and point to the official migration guide before upgrading. As of late 2026, 4.7.x is the stable line and 4.8 is in beta; re-check for 2027.
2. Read any design notes, roadmap, or decisions file in the project (e.g. `DESIGN.md`, `docs/`). If none exists, offer to create a short `DESIGN.md` with: fantasy, verbs, core loop, systems, decided-against list.
3. Respect the decided-against list. Never reintroduce mechanics or UI the user removed.
4. Check whether the project uses git. If not, recommend it before large changes; agent edits must be easy to undo.

## Routing

| Request is about… | Go to |
|---|---|
| Scene tree, autoloads, data Resources, signals, input, save/load, simulation tick, performance | `godot-foundations` |
| HUD, build menu, tooltips, inspectors, notifications, menus, Theme, controller/Steam Deck UI, accessibility | `godot-game-ui` |
| Testing a build, bugs, screenshots, automated tests, balance checks | `godot-playtest` |
| Models, materials, lighting, imports, LOD, MultiMesh, "make it look better" | `godot-3d-assets` |
| Steam page, demo, Next Fest, wishlists, Steamworks, export, Deck, AI disclosure | `godot-steam-release` |

Keep one plan across skills: every new mechanic must name its data, its UI surface, its visual representation, how it is tested, and how it shows up in a trailer/screenshot.

## Default Workflow For A New Game Or Major Feature

1. Lock the player fantasy and 3–5 primary verbs (e.g. place, assign, upgrade, inspect, expand).
2. Define the core loop, pressure/fail states, progression, and target session length.
3. Find the hook: one thing a stranger understands from a 5-second GIF. If there is none, design one before adding systems.
4. Check the design against the genre patterns in `references/genre-design-patterns.md`, especially the late-game sprawl problem.
5. Define data first (see `godot-foundations`), then the UI surface (see `godot-game-ui`), then visuals (see `godot-3d-assets`).
6. Build vertical slices: each slice playable end-to-end before the next starts.
7. Close every slice with a playtest pass (see `godot-playtest`).
8. Plan the demo early: the first 30–60 minutes are the product that earns wishlists (see `godot-steam-release`).

## Design Guardrails (management / strategy)

- Every system must create a decision, not just a number going up.
- Make cause and effect visible in the world before showing it in a panel.
- Prefer few, deep, interlocking systems over many shallow ones. Cut systems that need heavy UI to explain themselves.
- Give expansion a purpose beyond itself; plan the late game from day one.
- Difficulty modes tune pressure (rates, thresholds, threat frequency), not rules.
- Unlock systems gradually; the first minutes must be readable without a tutorial wall.
- Scope against comparable shipped games, and keep a contingency buffer (~25%).

## Working As An AI Agent On A Godot Project

- Make small, reviewable changes; run the project headless after each change to catch script errors (see `godot-playtest`).
- Prefer editing `.gd`, `.tres`, and `.tscn` text files carefully; keep `uid://` references intact and never hand-renumber them.
- If a Godot MCP server is connected, use it to inspect the live scene tree, run scenes, and capture screenshots; otherwise use the CLI.
- After each session, update `DESIGN.md` / a changelog with what changed and what the user should try in-game.

## Output Expectations

- Planning: short plan covering fantasy/verbs, loop, hook, data, UI surface, visuals, test approach, and demo impact, written for a non-programmer.
- Implementation: obvious folder structure, small working steps, verified by running the game or a headless check.
- Always end with what the user should try in-game to see the change.

## References

- Genre design patterns and lessons from recent hits: `references/genre-design-patterns.md`
- Market realities for 2026–2027 Steam releases: `../godot-steam-release/references/steam-market-2027.md`

## Examples

- "Add a housing-tier system to my village game."
- "The game is fun but looks weak, what should we fix first?"
- "Plan the next month of work toward a Next Fest demo."
- "My city builder gets boring after hour three."
