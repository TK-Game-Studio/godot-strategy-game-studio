# Save System Pattern (Godot 4)

## Principles

- Save simulation data, not nodes.
- Every saved object has a stable id and a type key that points to a data Resource (e.g. `"house_t1"`).
- Visuals are rebuilt from data after loading.
- Every save has `save_version`; loading runs migrations in order.

## Minimal shape

```gdscript
# autoload/SaveManager.gd
extends Node

const SAVE_VERSION := 1
const SLOT_PATH := "user://save_%d.json"

func save_game(slot: int) -> bool:
	var data := {
		"save_version": SAVE_VERSION,
		"state": GameState.to_dict(),
	}
	var f := FileAccess.open(SLOT_PATH % slot, FileAccess.WRITE)
	if f == null:
		push_error("Save failed: %s" % FileAccess.get_open_error())
		return false
	f.store_string(JSON.stringify(data, "\t"))
	return true

func load_game(slot: int) -> bool:
	if not FileAccess.file_exists(SLOT_PATH % slot):
		return false
	var parsed = JSON.parse_string(FileAccess.get_file_as_string(SLOT_PATH % slot))
	if typeof(parsed) != TYPE_DICTIONARY:
		return false
	parsed = _migrate(parsed)
	GameState.from_dict(parsed["state"])
	Events.game_loaded.emit()
	return true

func _migrate(data: Dictionary) -> Dictionary:
	var v: int = data.get("save_version", 0)
	# if v < 1: add defaults for fields introduced in version 1
	data["save_version"] = SAVE_VERSION
	return data
```

## Notes

- JSON turns ints into floats; cast on load (`int(d["x"])`).
- Vector3/Color are not JSON-native; store as arrays.
- Write to a temp file and rename for crash safety if saves get large.
- Autosave on a timer and before quitting (`NOTIFICATION_WM_CLOSE_REQUEST`).
