---
name: godot-audio
description: Designs and implements audio for Godot 4 strategy and management games - adaptive music with AudioStreamInteractive and playlists, ambient soundscapes that react to the settlement, positional 3D sound at zoom levels, UI sounds, notification priority, buses and mixing, volume settings, and soundtrack planning. Use when the user asks about music, sound effects, ambience, audio that is too loud/startling/repetitive, or audio settings.
---

# Godot Audio

## Overview

In strategy and management games, players listen for hours. Audio must inform without fatiguing: music that follows the game state, ambience that tells the player how the settlement is doing, and UI/notification sounds that are soft, distinct, and prioritized.

## Bus Layout

`Master` → `Music`, `Ambience`, `SFX` (world), `UI`, `Voice` (if any). Expose each in settings with persisted volumes (linear slider → `linear_to_db`). Add a gentle limiter/compressor on Master.

## Music

- Godot 4.3+ `AudioStreamInteractive`: add clips (e.g. calm, busy, tension, victory) and a transition table; switch clips by name from game state (season, threat level, time of day). Prefer automatic or cross fades on beat boundaries.
- `AudioStreamPlaylist` for shuffled calm tracks; `AudioStreamSynchronized` for layered stems (add percussion layer as the city grows).
- Silence is valid: allow gaps between tracks in long sessions.
- Danger music only for real danger; don't train players to ignore it.

## Ambience That Reflects State

- Layers driven by simulation: birds/wind (nature), crowd murmur and work sounds (density near camera), weather, seasonal layers.
- Crossfade layers with tweens based on camera zoom and what's on screen.
- Zoomed out: broad ambience; zoomed in: positional building sounds (`AudioStreamPlayer3D` with sensible `max_distance` and attenuation).
- Limit simultaneous positional sounds (voice stealing / per-type caps) to avoid noise walls in big cities.

## UI And Notifications

- One soft family of UI sounds (click, hover, confirm, cancel, error) at low volume.
- Notification priority tiers: info (subtle or none), warning (distinct, soft), critical (clear but not startling). Never use sharp, sudden sounds in a cozy game.
- Rate-limit repeated notifications (e.g. "no food" at most once per in-game day).

## Implementation Notes

- An `Audio` autoload that listens to simulation signals and decides what plays; game code never plays music directly.
- Use `AudioStreamRandomizer` for pitch/volume variation on repeated sounds (footsteps, chopping).
- Import settings: OGG Vorbis for music/ambience loops (enable loop), WAV for short SFX.
- Respect a "mute when unfocused" option.

## Sourcing

- Licensed libraries, commissioned composers, or original work; keep license records in `docs/audio-licenses.md`.
- AI-generated audio that ships must be disclosed on Steam (see `godot-steam-release`).

## Checklist

- [ ] All buses exposed in settings and persisted
- [ ] No sound exceeds the UI's loudness by surprise
- [ ] Music changes with state, never cuts abruptly
- [ ] 60-minute session without fatigue (repetition, harshness)
- [ ] Positional audio capped in dense areas
