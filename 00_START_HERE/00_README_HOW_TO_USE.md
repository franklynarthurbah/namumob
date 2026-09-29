# NAMULINDA — Prompt Pack v1 (read this first)

Built for: **Claude Opus 5.5** in an agentic setup (e.g. Claude Code with file + shell access)
Engine: **Unity 6000.3.9f1** (Unity 6.3 LTS) · 3D: **Blender** · Platforms: **Android + iOS** · Backend: **self-hosted** (game servers + API + admin panel)
Pack date: 2026-09-28

## 1. What this pack is
57 separate specification/prompt files (56 Markdown + 1 machine-readable JSON), ≈37,000 words total. Each file covers one topic (a map, a base, the netcode, the admin panel, the anti-cheat...) so Opus 5.5 reads only what a phase needs instead of one giant prompt. Every file starts with a line saying what to read before it and which phases use it.

## 2. Honest expectations
- The pack lets an AI build a complete architecture and a playable vertical slice fast. AI writes code, Blender `bpy` scripts (procedural modular kits), shaders, data tables, backend and admin panel well. It cannot hand-sculpt AAA hero art, record voice/foley, or replace human playtesting. See `05_CHARACTERS_ART_AUDIO/03` for the Tier 0/1/2 art plan (procedural blockout → kit-built → artist/marketplace polish).
- A 36 km² map with 48 players is ambitious on phones. The plan proves a 1.5 km "Slice" and the network/performance gates first, then scales (Phase 3 → Phase 7). Do not skip gates.
- Anything volatile (store rules, package versions, prices) is tagged **[VERIFY]**: Opus must check official docs at build time.
- Video research note: the four YouTube links could only be read as titles/descriptions (YouTube blocked automated access). Metro Royale and Bodycam were researched from written sources instead. Add your own timestamps/notes to `01_RESEARCH/03`.

## 3. Owner checklist (you must provide)
1. Apple Developer + Google Play Developer accounts; signing keys (keep offline backups).
2. A Mac with Xcode 26+ for iOS builds (Apple requires the iOS 26 SDK for uploads since 2026-04-28 [VERIFY]).
3. Hosting accounts + budget (see `08_BACKEND_SERVER_ADMIN/05`), a domain name, an email for alerts.
4. Legal: privacy policy, terms, trademark search for "Namulinda", music/font/asset licenses.
5. Answers to the open questions in `00_START_HERE/02_LOCKED_DECISIONS_AND_CONSTANTS.md` §5.
6. Installed: Unity Hub + Unity 6000.3.9f1 (Android Build Support incl. SDK/NDK/JDK; iOS Build Support on macOS), Blender (pinned LTS), .NET SDK 10, Node LTS, Docker, Git.

## 4. How to run it
1. Create an empty git repo. Put this pack at `docs/prompt-pack/` (keep folder names).
2. Copy `03_CLAUDE_MD_TEMPLATE.md` to the repo root as `CLAUDE.md`.
3. Start Opus 5.5 in the repo root. Paste `01_MASTER_PROMPT_CLAUDE_OPUS_5_5.md` as the first message.
4. Run phases from `07_UNITY_ENGINEERING/05_PHASED_BUILD_PLAN_WITH_PROMPTS.md` in order, one phase per session/branch.
5. At the end of every phase: tests pass, gate checklist ticked, commit, `docs/STATE.md` updated.
If you only have chat without file access: paste the master prompt, then one detail file per message; ask for complete files, not fragments.

## 5. Folder map
| Folder | Contents |
|---|---|
| 00_START_HERE | this file, master prompt, locked decisions, CLAUDE.md template |
| 01_RESEARCH | Metro Royale analysis, Bodycam analysis, fusion + sources |
| 02_GAME_DESIGN | concept/lore, match flow, combat, bodycam render, weapons/gear, economy, AI, modes |
| 03_VEHICLES_STUNT_BIKES | vehicle roster + stunt system, physics/netcode/Blender |
| 04_MAPS | world pipeline; Map A (overview, data JSON, extraction/hazards, build guide, 6 base files); Maps B, C, D |
| 05_CHARACTERS_ART_AUDIO | art bible, operators, Blender pipelines, animation/VFX/audio |
| 06_UI_UX_BRANDING | logo + app-icon prompts, UI system, HUD, screens, UI Toolkit build |
| 07_UNITY_ENGINEERING | setup/architecture, NamuNet netcode, performance, builds/CI, phased plan |
| 08_BACKEND_SERVER_ADMIN | backend, API/DB, dedicated server + matchmaking + fleet, admin panel, DevOps, live-ops |
| 09_MONETIZATION_IAP | store design + catalog, IAP implementation + compliance |
| 10_SECURITY_ANTICHEAT | threat model, client hardening, detection/bans, infra security |

## 6. Session hygiene
One phase per session · commit small · never let the AI "improve" locked decisions silently (change via `docs/DECISIONS.md`) · run the game on a real low-end Android phone every phase · keep secrets out of git.
