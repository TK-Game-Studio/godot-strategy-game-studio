# UI Recipes

## Brief to fill in before designing a HUD

```text
Game fantasy: <genre and world>
Camera: <top-down orbit, isometric, free RTS camera>
Primary verbs: <place, assign, upgrade, inspect, expand>
Always visible: <resource bar, time controls, build dock>
On hover: <tooltips for build items, resources, status icons>
On demand: <inspector, stats, goals, encyclopedia>
In-world indicators: <icons over buildings, queues, smoke, warnings>
Material/tone: <papercraft, wood, parchment, clean modern>
Motion: <calm; strong only for rewards and warnings>
Inputs: mouse+keyboard and gamepad (Steam Deck)
Resolutions: 1280x720, 1280x800, 1920x1080, 2560x1440
Avoid: dashboard look, default font, clutter in center, click-through to world
```

## Shell menus

- Title: continue, new game (mode/difficulty select), load, settings, quit
- Pause: resume, save, load, settings, main menu
- Settings: display (window mode, resolution, vsync, frame cap), graphics presets, UI scale, audio (master/music/SFX/UI/ambient), controls (remap keyboard + gamepad), accessibility (colorblind mode, reduced motion, text size), gameplay (autosave interval, edge scrolling), language
- Milestone/victory screens tied to the world, not plain dialogs

## Gamepad build flow (pattern)

1. Shoulder buttons cycle build categories; D-pad/stick selects item.
2. Placement cursor snaps to grid/road; stick moves cursor, A confirms, B cancels, bumper rotates.
3. Hold a button to open a radial for quick actions (demolish, upgrade, info).

## UI review checklist

- New player finds how to build something in under 10 seconds?
- Every buildable item has a tooltip with cost and effect?
- Center of screen clear during play?
- Any UI click ever places/selects in the world?
- Layout holds at 1280x720, 1280x800, 2560x1440?
- Text readable on a Steam Deck screen?
- Every action reachable by gamepad?
- Numbers readable over bright and dark world areas?
- Only one primary panel open at a time?
- UI sounds soft and consistent?
- All strings localizable?
