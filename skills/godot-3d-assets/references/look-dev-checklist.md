# Look-Dev Checklist

## Identity
- Can someone name the art style in one phrase ("papercraft diorama", "painterly medieval")?
- Does a 460x215 capsule-size crop of a screenshot still read?
- Is there one signature visual element (a material, a light, a building silhouette) repeated across the game?

## Lighting
- Tonemapper chosen deliberately and compared
- Ambient + SSAO tuned so shadows aren't muddy
- Glow subtle; re-tuned after any 4.5→4.6+ upgrade
- Time-of-day/season palettes defined and consistent

## Materials
- Shared library in `res://art/materials/`; no one-off materials without reason
- Roughness/metal values consistent per material family

## Composition at default zoom
- Buildings distinguishable by silhouette and color
- Roads/paths readable against terrain
- Selection/hover highlight visible on every surface color

## Motion and life
- Idle motion in the scene at all times (smoke, foliage, villagers)
- Placement "juice": snap, pop, dust, sound

## Performance
- Draw calls and frame time recorded before/after each change
- Low-end target (e.g. Steam Deck) checked
