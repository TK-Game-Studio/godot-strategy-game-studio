# Simulation Scaling Patterns (Godot 4)

## Fixed tick, decoupled from frames

```gdscript
# sim/SimClock.gd (owned by GameState)
extends Node

signal ticked(tick: int)

@export var ticks_per_second := 5.0
var speed := 1.0          # 0 = paused, 1, 2, 3
var _acc := 0.0
var tick := 0

func _process(delta: float) -> void:
	if speed <= 0.0:
		return
	_acc += delta * speed
	var step := 1.0 / ticks_per_second
	var guard := 0
	while _acc >= step and guard < 8:   # cap catch-up to avoid spiral of death
		_acc -= step
		tick += 1
		ticked.emit(tick)
		guard += 1
```

Systems connect to `ticked` and do their work; heavy systems run every N ticks (`if tick % 10 == 0`).

## Agents as data

- Store villagers/carts in arrays of small Resources or packed arrays (positions, states, job ids).
- A presentation layer spawns visual nodes only for agents near the camera, pooled and reused.
- Far agents can be drawn with a `MultiMeshInstance3D` (one instance per agent, transform updated each tick) or not drawn at all.

## Time-slicing

- Job assignment, pathfinding requests, and need recalculation process a fixed budget per tick (e.g. 20 agents per tick), round-robin.
- Cache path results between common endpoints (home ↔ workplace) and invalidate on road changes.

## Derived values

- Keep aggregates (total housing, food per minute, workforce by type) cached; update on `building_placed/removed/upgraded` signals instead of recomputing every tick.

## Profiling

- Use the Godot Profiler and Monitors; add custom monitors with `Performance.add_custom_monitor` for sim tick time and agent counts.
- Test with a stress save (debug cheat that spawns hundreds of buildings and agents) after every major system.
- Lower-allocation physics query result classes are arriving in 4.8; prefer them for frequent raycasts when available.
