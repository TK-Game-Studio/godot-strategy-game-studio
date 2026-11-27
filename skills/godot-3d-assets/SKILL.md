---
name: godot-3d-assets
description: Creates, imports, and optimizes 3D assets and the visual look for Godot 4 games. Use when the task involves building models in code, importing GLB/glTF from Blender, materials and shaders for a stylized look, lighting and WorldEnvironment, import settings, LOD, MultiMesh, visual identity for Steam screenshots, or when the game "looks weak".
---

# Godot 3D Assets

## Overview

Consistent, identifiable visuals beat detailed ones at indie scale. Lock scale, pivots, materials, palette, and lighting first; add detail where the camera actually looks; make sure screenshots and 5-second clips read instantly.

Two supported sources:
- **Code-built meshes**: assembled from primitives (`BoxMesh`, `CylinderMesh`, `PrismMesh`) or generated with `SurfaceTool`/`ArrayMesh`. Safe for agent-driven projects, easy to vary procedurally.
- **Imported GLB/glTF 2.0** from Blender or other tools, for shapes too complex for code. Don't ship FBX/OBJ as the long-term format.

Respect the user's chosen source. If they decided to build models in code, don't push imported assets.

## Shared Conventions (decide once, write in DESIGN.md)

- 1 unit = 1 meter; one grid/cell size; buildings sized to whole cells.
- Origin at bottom-center of the footprint; -Z is the building's front/door.
- Naming: `building_<type>_t<tier>`, `prop_<name>`, `mat_<name>`.
- One scene per building type; visuals as a child node swappable by tier/state.

## Stylized Look Recipe (diorama / papercraft / cozy / painterly)

1. Lighting first (largest gain, lowest risk):
   - `WorldEnvironment`: tonemap (AgX or Filmic/ACES — compare), ambient light, SSAO, subtle glow, gentle fog for depth. Since 4.6 glow runs before tonemapping; re-tune old scenes.
   - One `DirectionalLight3D` with soft shadows; time-of-day color grading via a gradient.
   - 4.7+: `AreaLight3D` for soft rectangular light (windows, signs, lanterns-as-panels).
   - 4.8+: multi-bounce AO reduces over-darkening; screen-space contact shadows ground small props.
2. Materials: a small shared library (8–15 materials or one shader with color/pattern params). Flat or gently textured albedo, high roughness, no metal for paper/wood/cloth.
3. Handmade feel: small random rotation/scale offsets, trim pieces, slightly irregular edges, procedural detail (flowers, fences, clutter) that appears as the settlement grows.
4. Miniature feel: subtle tilt-shift/DOF via camera attributes; keep it light so UI and gameplay stay readable.
5. Palette: same family as the UI so world and interface feel like one game.
6. Life last: smoke, birds, swaying trees, villagers walking, seasonal changes — these sell screenshots and trailers.

## Import Rules (GLB)

- Apply transforms and set origin in Blender before export.
- Import dock: LOD generation on, shadow meshes on, collision only where needed (simple shapes).
- Override imported materials with the shared library to keep the style consistent.

## Performance Rules

- `MultiMeshInstance3D` for repeated props (trees, rocks, fences, crops, far villagers).
- `visibility_range_end` on small details; LOD on dense meshes.
- Merge static decoration; reuse materials to keep draw calls low.
- Texture size proportional to on-screen size (256–1024 for most props). 4.8+ mip-level texture streaming reduces VRAM in large worlds.
- Check the debug overlay (draw calls, frame time) after every visual step; target Steam Deck as the low end if Deck support matters.

## Workflow For "Make It Look Better"

1. Capture screenshots at default zoom: start, busy mid-game, evening/night.
2. Fix lighting/environment → unify materials/palette → add silhouette detail to the 3–5 most-seen buildings → add ambient life.
3. Re-capture the same shots and compare side by side after each step.
4. Re-check performance after each step.

## Common Failure Modes

- Mixed scales; off-center pivots causing placement offsets
- Dozens of near-identical materials
- Detail nobody sees at default zoom
- Fixing asset problems with runtime offsets instead of fixing the asset
- Visual changes that silently alter collision/footprint
- Generic realism with no identity; screenshots that don't read at thumbnail size

## References

- Look-dev checklist: `references/look-dev-checklist.md`
