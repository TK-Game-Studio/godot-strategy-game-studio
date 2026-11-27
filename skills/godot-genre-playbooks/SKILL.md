---
name: godot-genre-playbooks
description: Design playbooks for strategy and management subgenres built in Godot 4 - city builder, colony sim, survival/society builder, 4X, grand strategy, RTS, tactics, tycoon, roguelite builder, and RPG/strategy hybrids. Use when the user is choosing or mixing subgenres, asks how a genre usually solves a problem (late game, snowballing, micromanagement, pacing), or wants a feature list, core loop, or MVP scope for a specific strategy/management genre.
---

# Godot Genre Playbooks

## Overview

Each subgenre has a known core loop, known failure modes, and known solutions. Use this skill to pick a subgenre (or a deliberate hybrid), set the MVP scope, and avoid problems the genre has already solved. Pair every playbook with `godot-game-studio` for the overall plan.

## Pick The Playbook

| Subgenre | Core question for the player | Signature failure mode | Reference |
|---|---|---|---|
| City builder | Where and in what order do I build? | Late-game sprawl becomes maintenance | `references/builders-colony-survival.md` |
| Colony sim | How do I keep these individuals alive and happy? | Stories repeat; micromanagement | `references/builders-colony-survival.md` |
| Survival / society builder | What am I willing to sacrifice to survive? | Dominant "cruel but optimal" path | `references/builders-colony-survival.md` |
| 4X | How do I grow faster than rivals? | Snowball + late-game drag | `references/4x-grand-strategy.md` |
| Grand strategy | How do I steer a realm through history? | Opaque systems, blobbing | `references/4x-grand-strategy.md` |
| RTS / real-time tactics | Can I out-plan and out-execute? | APM walls, unit pathing jank | `references/rts-tactics-tycoon-hybrids.md` |
| Tycoon / business | How do I turn a profit in a moving market? | Solved economy, spreadsheet feel | `references/rts-tactics-tycoon-hybrids.md` |
| Roguelite builder | Can I win this run with what I'm offered? | Runs feel samey | `references/rts-tactics-tycoon-hybrids.md` |
| RPG / strategy hybrid | Who are my characters and what do they become? | Two half-games | `references/rts-tactics-tycoon-hybrids.md` |

## How To Use A Playbook

1. Name the primary subgenre and at most one secondary flavor. More than two usually means two half-games.
2. Copy the playbook's core loop and adapt verbs to the fantasy.
3. Choose answers to the signature failure mode before building content.
4. Scope the MVP using the playbook's "MVP slice" list; everything else goes to a later list in `DESIGN.md`.
5. Pick 2–3 reference games to study for specific sub-systems, not to clone.
6. Route implementation: economy → `godot-economy-balance`, agents and opponents → `godot-agents-ai`, maps → `godot-procedural-worlds`, events/laws/story → `godot-events-narrative`.

## Cross-Genre Principles

- Readable cause and effect beats realism. If the player can't see why something happened, it feels random.
- Every growth curve needs a counter-pressure (upkeep, unrest, threats, rivals, entropy).
- Automate what has been mastered: offer automation/delegation as the empire grows so attention stays on decisions.
- Telegraph threats; let the player prepare.
- Give the late game a new question, not just bigger numbers (crisis, endgame scenario, new region, legacy goal).
- Keep the session shape in mind: short runs (roguelite), chapters/scenarios, or endless sandbox each need different pacing.

## References

- City builder, colony sim, survival/society: `references/builders-colony-survival.md`
- 4X and grand strategy: `references/4x-grand-strategy.md`
- RTS, tactics, tycoon, roguelite builder, RPG hybrids: `references/rts-tactics-tycoon-hybrids.md`
- Shared patterns and recent hits: `../godot-game-studio/references/genre-design-patterns.md`
