---
name: godot-economy-balance
description: Designs and balances game economies in Godot 4 strategy and management games - resources, production chains, needs and population tiers, prices, upkeep, costs, unlock pacing, and difficulty curves. Use when the user asks to balance numbers, design production chains, tune difficulty modes, fix runaway or starving economies, build a balance spreadsheet, or run simulated balance tests.
---

# Godot Economy Balance

## Overview

Balance is a loop: model it on paper, encode it as data, simulate it headless, play it, adjust. Numbers live in data Resources (`res://data/`), never hard-coded, so balancing never needs code changes.

## Workflow

1. Map the economy as a graph: every resource, its producers, consumers, and conversion ratios. Check for dead ends, loops without sinks, and resources with no use. Export the graph as Mermaid in `docs/economy.md`.
2. Define rates per unit time (per in-game minute/day), not per tick, so speed changes don't break balance.
3. Build a spreadsheet (or CSV) generated from the data Resources: buildings × inputs/outputs/workers/cost/upkeep. A simple script can dump all `.tres` defs to CSV so the sheet always matches the game.
4. Compute the "ideal ratios" (e.g. 2 farms feed 1 bakery feeds 12 houses) and the payback time of each building (cost ÷ net value per minute).
5. Encode targets per phase: minute 0–10, 10–30, 30–60, 60+ — what should the player afford, how many people, which unlocks.
6. Simulate: headless balance runs with a fixed seed and a scripted "reasonable player" build order; log stock, population, happiness per day to CSV; compare across difficulty modes (see `godot-playtest`).
7. Playtest and adjust only a few values at a time; record changes in a balance changelog.

## Rules Of Thumb

- Every source needs a sink: upkeep, consumption, decay, construction, trade.
- Growth curves: costs grow faster than linear (e.g. ×1.15–1.5 per tier), income grows roughly linear per building — creates decisions about when to expand.
- Payback times should lengthen as the game progresses; early buildings pay back in minutes, late ones over longer horizons.
- Avoid one dominant strategy: if one building/route is best in all situations, add a constraint (space, workers, input scarcity, upkeep) or a counter.
- Make shortages recoverable on default difficulty; on hard, shortages cascade.
- Difficulty modes change rates and thresholds (consumption ×, threat frequency, starting stock), not rules.
- Show the math: tooltips with production/consumption per minute and the reason for each modifier.

## Implementation In Godot

```gdscript
# scripts/BuildingDef.gd
class_name BuildingDef extends Resource
@export var id: StringName
@export var cost: Dictionary[StringName, int] = {}
@export var upkeep_per_day: Dictionary[StringName, float] = {}
@export var inputs_per_day: Dictionary[StringName, float] = {}
@export var outputs_per_day: Dictionary[StringName, float] = {}
@export var workers: int = 0
@export var unlock_tier: int = 0
```

- Typed dictionaries need Godot 4.4+; on older versions use plain `Dictionary`.
- Difficulty presets as `DifficultyDef` Resources with multipliers applied in one place in the simulation.
- A `tools/export_balance_csv.gd` (run with `godot --headless --script`) that walks `res://data/` and writes CSV for the spreadsheet.

## Anti-Patterns

- Numbers scattered across scripts
- Balancing only by playing (no model, no simulation)
- Changing many values at once
- Hidden modifiers the player can't see
- Inflation without sinks; single-currency economies with nothing to spend on late

## References

- Formulas and checks: `references/balance-formulas.md`
