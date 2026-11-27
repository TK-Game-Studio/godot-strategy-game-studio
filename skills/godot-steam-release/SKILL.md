---
name: godot-steam-release
description: Prepares a Godot 4 game for Steam. Use when the user asks about a Steam store page, capsule art, trailer, demo, Steam Next Fest, wishlists, Early Access, Steamworks integration (achievements, cloud saves) with GodotSteam, Windows export, Steam Deck compatibility, the AI content disclosure, or a release plan and launch timing.
---

# Godot Steam Release

## Overview

On Steam, visibility is earned by wishlists, demo engagement, and a store page that sells the hook in seconds. Plan release work alongside development, not after it. Give the user facts and options; for business decisions (price, publisher deals), present trade-offs rather than one answer.

Market facts change quickly; check `references/steam-market-2027.md` and re-verify figures with a web search before relying on them.

## Release Roadmap (adapt to the project)

1. Store page "Coming Soon" as early as the hook is visible: capsule art, 5+ screenshots showing gameplay, short GIF/trailer, clear tags. Start collecting wishlists early.
2. Vertical slice → public demo: the first 30–60 minutes, polished, ending on a cliffhanger moment. Put it live a few weeks before a Next Fest, not months.
3. Steam Next Fest: wishlists collected before the fest matter most; keep building them in the weeks before. Valve gives every game a minimum front-page slot, not equal visibility.
4. Early Access or 1.0: Early Access suits management/colony games with a clear roadmap; launch with enough content that reviews are about the game, not its emptiness.
5. Post-launch: frequent patches in week 1–2, visible roadmap updates, seasonal sales.

## Store Page Essentials

- Capsules readable at small sizes; title legible; art style consistent with the game.
- First 2 screenshots: the busiest, most identity-rich gameplay moments, not menus.
- Short description: fantasy + verbs + hook in one or two sentences.
- Trailer: gameplay in the first 5 seconds; building/growth timelapses work well for builders.
- Tags: genre (City Builder, Colony Sim, Management, Strategy) + mood (Cozy, Relaxing) + feature (Base Building, Resource Management).

## Godot Export And Steamworks

- Export templates must match the editor version exactly. Use a release export for Steam builds; debug cheats/overlays off (`OS.is_debug_build()` gates).
- Steamworks: use GodotSteam (GDExtension, available on the Godot Asset Store) or another maintained Steamworks GDExtension. Match the plugin version to the Steamworks SDK version listed in its compatibility table.
- Typical features: achievements (publish them in the Steamworks backend before testing), Steam Cloud for `user://` saves, rich presence. The Steam overlay generally only works in exported builds launched through Steam.
- Keep a `steam_appid.txt` only for local testing; don't ship it unless the plugin's docs say so.
- Wrap Steam calls so the game still runs without Steam (other stores, dev runs).

## Steam Deck / Steam Machine

To be Verified: the default controller config must reach all functionality without changing settings, text entry must open the on-screen keyboard automatically, text must be readable on the device, and controller glyphs should match. Coordinate with `godot-game-ui` and test at 1280×800.

## AI Content Disclosure

Steam's content survey asks whether the game uses generative AI for content players see, hear, or interact with — in the game or on the store page (art, audio, text, marketing). Since January 2026, AI tools used only behind the scenes for efficiency (e.g. coding assistants) don't need disclosure. Pre-generated assets are disclosed publicly on the store page; live-generated content also requires describing guardrails. Help the user answer honestly; the developer remains responsible for copyright and legality of all content. Note: disclosed AI content can affect player perception — top Next Fest performers disclosed AI use far less often than the rest in late 2026.

## Release Checklist

- [ ] Store page: capsules, screenshots, trailer, description, tags, system requirements
- [ ] Demo build separate app, saves don't conflict with the full game
- [ ] Release export runs on a clean Windows machine
- [ ] Steamworks: achievements published, cloud save paths configured, works offline/without Steam
- [ ] Deck: controller, on-screen keyboard, text size, performance
- [ ] Settings persist; crash-free 60-minute session
- [ ] AI disclosure answered accurately
- [ ] Localization for target markets; store page translated
- [ ] Press kit, Discord/community link, feedback channel in-game

## References

- Market realities 2026–2027: `references/steam-market-2027.md`
