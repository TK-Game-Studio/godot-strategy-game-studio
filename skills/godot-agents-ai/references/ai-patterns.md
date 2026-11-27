# AI Patterns (Godot 4, GDScript)

## Utility scoring

```gdscript
# sim/ai/UtilityPicker.gd
class_name UtilityPicker extends RefCounted

static func pick(options: Array[Dictionary]) -> Dictionary:
	# each option: {"id": StringName, "score": float}
	var best := {}
	var best_score := -INF
	for o in options:
		if o.score > best_score:
			best_score = o.score
			best = o
	return best

static func curve(x: float, exponent := 2.0) -> float:
	# 0..1 input -> 0..1 urgency; exponent > 1 makes urgency rise late
	return pow(clampf(x, 0.0, 1.0), exponent)
```

Score example: `eat = curve(hunger) * (1.0 if food_available else 0.0) * distance_penalty(dist)`.

Add small random jitter (seeded RNG) to break ties so agents don't move in lockstep.

## Grid pathfinding with AStarGrid2D

```gdscript
var astar := AStarGrid2D.new()
func setup(size: Vector2i) -> void:
	astar.region = Rect2i(Vector2i.ZERO, size)
	astar.cell_size = Vector2.ONE
	astar.diagonal_mode = AStarGrid2D.DIAGONAL_MODE_ONLY_IF_NO_OBSTACLES
	astar.update()

func set_blocked(cell: Vector2i, blocked: bool) -> void:
	astar.set_point_solid(cell, blocked)

func set_road(cell: Vector2i) -> void:
	astar.set_point_weight_scale(cell, 0.5)  # roads are cheaper

func path(from: Vector2i, to: Vector2i) -> Array[Vector2i]:
	return astar.get_id_path(from, to)
```

## Flow field (many units, one target)

1. BFS/Dijkstra from the target cell over the grid → integration field (cost to target).
2. For each cell, direction = toward the lowest-cost neighbor.
3. Units read the direction of their cell each tick. Recompute only when the target or obstacles change.

## Job board

```gdscript
# data row
{"id": 42, "type": &"haul", "from": Vector2i(3,4), "to": Vector2i(9,2), "item": &"wood", "claimed_by": -1, "priority": 2}
```

Each tick, idle agents (budgeted) take the highest `priority / distance` unclaimed job they can do.

## Storyteller / director hook

Agents and colony stats feed a director (see `godot-events-narrative`) that schedules threats and windfalls based on wealth, mood, and time since the last major event.
