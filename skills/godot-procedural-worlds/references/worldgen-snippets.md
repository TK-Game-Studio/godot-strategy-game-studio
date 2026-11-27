# World Generation Snippets (Godot 4)

## Seeded height + moisture

```gdscript
class_name WorldGen extends RefCounted

var height_noise := FastNoiseLite.new()
var moist_noise := FastNoiseLite.new()

func _init(map_seed: int) -> void:
	height_noise.seed = map_seed
	height_noise.noise_type = FastNoiseLite.TYPE_SIMPLEX_SMOOTH
	height_noise.frequency = 0.02
	height_noise.fractal_type = FastNoiseLite.FRACTAL_FBM
	height_noise.fractal_octaves = 4
	moist_noise.seed = map_seed + 1013
	moist_noise.frequency = 0.035

func height_at(x: int, y: int, size: Vector2i) -> float:
	var h := (height_noise.get_noise_2d(x, y) + 1.0) * 0.5   # 0..1
	# radial falloff for an island/basin shape
	var c := Vector2(size) * 0.5
	var d := Vector2(x, y).distance_to(c) / c.length()
	return h * (1.0 - smoothstep(0.6, 1.0, d))

func biome(h: float, m: float) -> StringName:
	if h < 0.30: return &"water"
	if h < 0.34: return &"beach"
	if h > 0.80: return &"mountain"
	if m > 0.6: return &"forest"
	if m < 0.3: return &"dry_grass"
	return &"meadow"
```

## Hex axial helpers

```gdscript
const HEX_DIRS := [Vector2i(1,0), Vector2i(1,-1), Vector2i(0,-1), Vector2i(-1,0), Vector2i(-1,1), Vector2i(0,1)]

static func hex_to_world(q: int, r: int, size: float) -> Vector3:
	var x := size * (sqrt(3.0) * q + sqrt(3.0) / 2.0 * r)
	var z := size * (1.5 * r)
	return Vector3(x, 0.0, z)

static func hex_distance(a: Vector2i, b: Vector2i) -> int:
	var dq := a.x - b.x
	var dr := a.y - b.y
	return (absi(dq) + absi(dr) + absi(dq + dr)) / 2
```

## Spacing-based resource placement (simple)

Try random candidates (seeded) and accept a point only if no accepted point of the same type is within `min_dist`; stop after K failures. Good enough for most strategy maps.

## Fairness check near start

Count resource cells by type within radius R of the start; if any is below its minimum, place extra deposits or regenerate with `seed + attempt`.
