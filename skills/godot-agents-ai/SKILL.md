---
name: godot-agents-ai
description: Builds AI for Godot 4 strategy and management games - villager/colonist behavior, needs, job and task assignment, utility AI, state machines and behavior trees, pathfinding with NavigationServer3D, AStarGrid2D or flow fields, crowds and RTS unit movement, and AI opponents for 4X/RTS/grand strategy. Use when agents get stuck, behave stupidly, cost too much CPU, or when the user wants rivals, raiders, or smarter citizens.
---

# Godot Agents AI

## Overview

Two different problems:
1. **Population agents** (villagers, colonists, carriers, customers): many, simple, must look alive and be cheap.
2. **Strategic opponents** (rival factions, raiders, AI empires): few, must make good decisions and feel fair.

Keep both inside the simulation layer (see `godot-foundations`): AI decides on data; visuals follow.

## Population Agents

### Decision model
- Needs (hunger, rest, social, faith…) decay over time; thresholds trigger tasks.
- Utility AI: score each candidate task = need urgency × task availability × distance penalty × personal trait modifiers; pick the highest. Easy to tune and explain in tooltips ("going to eat: hunger 82%").
- A small state machine per agent for task execution (Idle → MoveTo → Work → Carry → Return). Behavior trees only if tasks get complex; don't start with them.

### Job system
- Buildings post jobs/requests to a central job board (data). Agents claim jobs; one claim per job prevents swarming.
- Hauling: match supply and demand with a simple nearest-first assignment per tick budget.
- Priorities: let the player set building/job priorities instead of controlling agents directly.

### Movement and pathfinding
- Grid games: `AStarGrid2D` on the logical grid (fast, simple, deterministic); cache common paths (home ↔ work); invalidate on road/building changes.
- Freeform 3D terrain: `NavigationRegion3D` + `NavigationServer3D.map_get_path()`; bake navmesh with coarse cell size and simple source geometry; re-bake regions incrementally.
- Many agents with one goal (RTS armies, crowds to an event): flow field over the grid — one computation, many followers.
- RVO avoidance (`avoidance_enabled` on NavigationAgent3D) is expensive with many agents; enable it only where needed (near the camera, in tight spaces).
- Roads as faster edges (lower cost) give players a reason to build them.

### Scale
- Simulation LOD: far/off-screen agents update less often and skip animation; teleport along precomputed paths when invisible.
- Time-slice decisions (N agents per tick). Render crowds with `MultiMeshInstance3D`.
- Profile with custom monitors (agent count, decisions per tick, path requests per second).

## Strategic Opponents

- Utility AI at the strategic level: score actions (expand, build army, research, trade, attack target X) with weights from an AI personality Resource (aggressive, builder, trader).
- Build orders / opening scripts for the first minutes, then switch to utility scoring.
- Influence maps (threat, value, control per cell) to choose where to expand or attack.
- Difficulty: improve decision quality and reaction time first; resource bonuses only as a transparent, optional layer.
- Fairness: no hidden map knowledge on normal difficulty; let the player see AI intentions (diplomacy text, troop movements).
- Make the AI testable headless: AI vs AI matches with fixed seeds; log win rates and timings.

## Debug Tools

- Per-agent inspector: current need values, task scores, path.
- Overlays: job board, heatmaps (traffic, unhappiness), influence maps.
- Toggle via the debug overlay (F3) only in debug builds.

## Anti-Patterns

- One heavy node with `_physics_process` per villager at scale
- Avoidance on every agent
- Hard-coded if/else chains for decisions (use scoring)
- Cheating AI as the only difficulty lever
- AI that reads renderer state instead of simulation data

## References

- Patterns and snippets: `references/ai-patterns.md`
