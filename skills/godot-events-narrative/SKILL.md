---
name: godot-events-narrative
description: Builds event systems, emergent narrative, and decision layers for Godot 4 strategy and management games - random and triggered events, storylets, an event director/storyteller that paces threats, laws and policies, advisors, quests and goals, characters with traits, chronicles, scenarios, and endings. Use when the user wants events, story, choices with consequences, laws/edicts, a threat director, goals/quests, or says the game lacks drama or surprises.
---

# Godot Events & Narrative

## Overview

Strategy and management games tell stories mostly through systems: autonomous characters, pressures, and the player's choices, with a light layer of written events on top. Crusader Kings mixes simulation, characters with personalities, and scripted vignettes; RimWorld uses an AI storyteller that deals threats and windfalls; Frostpunk turns laws into moral choices that the ending reflects. Build the same layers, data-driven.

## Layers

1. **Systemic stories**: needs, moods, traits, relationships, disasters arising from simulation state. Free content; make them visible (notifications, chronicle entries).
2. **Event director**: decides *when* something happens, based on pacing (time since last major event, colony wealth/strength, difficulty).
3. **Events / storylets**: small authored pieces with conditions, text, options, and effects, defined as data.
4. **Decisions with weight**: laws, edicts, policies, research paths — costly or irreversible, with delayed consequences.
5. **Goals and scenarios**: milestones, quests, scenario objectives, endings that comment on the path taken.
6. **Chronicle**: a log of notable moments the player can reread or share (screenshots, "history of your village").

## Event Data Model

```gdscript
class_name EventDef extends Resource
@export var id: StringName
@export var weight: float = 1.0
@export var cooldown_days: int = 10
@export var min_day: int = 0
@export var conditions: Array[ConditionDef] = []   # e.g. stock < X, season == winter, has_trait
@export var title_key: String        # localized via tr()
@export var body_key: String
@export var options: Array[EventOption] = []       # each with text_key, conditions, effects
@export var tags: Array[StringName] = []           # threat, windfall, social, story
```

Conditions and effects are small Resources (`ConditionDef`, `EffectDef`) with a `check(state)` / `apply(state)` method so designers (and modders) can combine them without code.

## Director (storyteller)

- Track "tension": rises with time and prosperity, drops after threats.
- Each in-game day, roll for an event with probability based on tension and difficulty; choose by weighted random among events whose conditions pass and cooldowns expired.
- Guarantee breathing room after major threats; cap the number of simultaneous crises.
- Expose director personalities as difficulty choices (calm builder, balanced, chaotic).

## Choice Design

- Options should trade between values the game measures (food vs mood, safety vs growth, short vs long term).
- Show immediate effects; hint at delayed ones.
- Avoid one obviously correct answer; make extreme paths powerful but costly.
- Remember choices: later events reference earlier ones (flags in save data).

## Writing And Localization

- All text through translation keys and `tr()`; keep sentences short; variables via placeholders (`{name}`, `{amount}`).
- Tone consistent with the game (cozy, grim, humorous); keep a style guide in `docs/writing.md`.
- If AI tools generate text that ships, it falls under Steam's AI content disclosure (see `godot-steam-release`).

## Testing

- Headless: run 100 in-game years with a seed; log which events fired, how often, and tension curves. Flag events that never fire or fire too often.
- Validate every EventDef at startup (missing keys, unreachable options).

## References

- Templates: `references/event-templates.md`
