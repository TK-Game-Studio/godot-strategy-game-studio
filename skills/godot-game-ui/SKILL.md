---
name: godot-game-ui
description: Designs and implements game UI in Godot 4 with Control nodes and Themes. Use when the user asks for a HUD, build menu, hover tooltips, building inspector, resource bar, notifications, pause/settings/title menus, UI theme or fonts, controller or Steam Deck support for UI, accessibility, localization, or says the UI feels cluttered, plain, or confusing.
---

# Godot Game UI

## Overview

The goal is a readable, themed interface that supports play and protects the view of the world. In management and city-building games the world is the main screen; UI frames it.

Default: the world is a 3D scene; UI is `Control` nodes on a `CanvasLayer`, styled by one shared `Theme` resource.

## Visual Direction First

Settle and write down (in `DESIGN.md`):
- fantasy and material language (paper, wood, cloth, parchment, brass, clean modern)
- typography: one display font, one readable body font (set in the Theme)
- palette: 4–6 colors shared with the world's palette
- motion tone (calm for cozy; strong motion only for rewards and warnings)
- UI sound tone (soft, consistent, never startling)

## Implementation Standards

1. One `Theme` (`res://ui/theme/main_theme.tres`) on the root UI Control; style with `StyleBoxFlat`/`StyleBoxTexture` (9-slice), not per-node overrides.
2. Layout with containers and anchors, never absolute pixel positions.
3. Scaling: stretch mode `canvas_items`, aspect `expand`; test at 1280×720, 1280×800 (Steam Deck, 16:10), 1920×1080, 2560×1440. Offer a UI scale setting.
4. Input boundary: blocking UI uses `mouse_filter = STOP`; decorative containers `IGNORE`/`PASS`. Camera and placement read input in `_unhandled_input`.
5. When a modal is open, gate camera/placement input explicitly (pause with UI `process_mode = ALWAYS`, or a UI-open flag).
6. UI reacts to simulation signals; it never computes game rules.
7. Version-aware extras (check the project's version first):
   - 4.6+: mouse/touch focus is separate from keyboard/gamepad focus; review custom focus styles.
   - 4.7+: Control transform offset properties allow animating position/rotation/scale inside containers without fighting layout.
   - 4.8+: `Label`/`RichTextLabel` `auto_font_size` with min/max sizes for fitting text in fixed boxes.

## Management-Game UI Budget

Always visible:
- compact resource bar (top edge)
- time/speed cluster (corner)
- build dock (bottom edge, compact)

On hover:
- tooltip for every buildable item, resource, and status icon

On demand:
- building inspector when selected
- stats, needs/tiers, goals, encyclopedia behind buttons or hotkeys

Rules: center of the screen clear during play; persistent UI ≈20% of the screen or less; one large panel at a time; notifications are short toasts with a log for history; prefer in-world indicators (icons over buildings) over panels.

## Tooltips

- Build options show name, cost, inputs/outputs, requirements, and why locked.
- `tooltip_text` for simple text; override `_make_custom_tooltip(for_text)` for rich tooltips (icons, colored costs).
- Unaffordable costs in red with what is missing.
- Short tooltip delay (~0.3 s); allow pinning or nested tooltips for complex terms if the game is deep.

## Controller, Steam Deck, Accessibility

- Every action reachable by gamepad by default (required for Deck Verified): focus navigation (`focus_neighbor_*`, focus mode), radial or tabbed build menu, cursor-snapping for placement.
- Show matching button glyphs for the active input device; don't flicker between keyboard and controller prompts.
- Text readable at 1280×800 on a 7" screen: body text roughly 18 px or larger at that resolution; respect the UI scale setting.
- On-screen keyboard when text entry is required (e.g. naming a save) on Deck.
- Colorblind-safe status colors (pair color with icon/shape), reduced-motion toggle, remappable keys, subtitles/captions for voiced or important audio.
- Localization from day one: all strings through `tr()` and CSV/PO translations; leave room for longer text (German, Turkish).

## Motion

- `create_tween()`, UI durations 0.12–0.25 s.
- Strong motion only for state change, reward, danger, onboarding; respect reduced motion.

## Anti-Patterns

- Admin-dashboard look: flat gray boxes, default font, equal panels on every edge
- Many panels open at first load; walls of text during play
- Absolute positioning that breaks at other resolutions
- Click-through from UI to the world
- Tiny text that fails on Steam Deck
- Mouse-only interactions with no controller path
- Loud or sharp UI sounds in a cozy game
- UI for mechanics the user removed

## References

- Recipes and review checklist: `references/ui-recipes.md`
