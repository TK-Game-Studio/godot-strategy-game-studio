# Playtest Checklist (Godot, desktop + Steam Deck)

## Universal
- Boots into a useful first state without errors in the output log
- Main verbs obvious and responsive
- HUD readable over the world, center kept clear
- Pause, pressure, failure, and recovery states work
- Window resize, fullscreen toggle, alt-tab/focus loss behave sensibly

## Management depth
- Late-game save: still meaningful decisions every few minutes?
- Expansion serves a goal; sprawl isn't just maintenance
- Each difficulty mode feels different in pressure, not rules
- Production chains understandable from tooltips/previews

## 3D
- Camera stability and limits; camera ↔ menu input handoff
- Depth and silhouette readability
- Lighting/material stability across day/night, seasons, weather
- No z-fighting, missing textures, shadow acne
- Placement footprint and collision alignment
- Performance with late-game object counts

## Save/Load
- Save from every mode; load restores stock, buildings, levels, time, population, unlocks, RNG state
- Previous build's save still loads (migration)
- Autosave does not hitch noticeably

## Steam / Deck readiness
- Windows export runs on a clean machine without the editor
- Settings persist between launches
- No debug overlays/cheats in release export
- Full controller access with default config; on-screen keyboard for text entry
- Text readable at 1280x800
- Correct window title, icon, version string

## Reporting
- Screenshots for visual findings
- Severity order, reproduction steps
- Likely owner: simulation, visuals, UI, assets, save system
