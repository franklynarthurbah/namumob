# LOCKED DECISIONS, CONSTANTS, ASSUMPTIONS, LEGAL GUARDRAILS
> Read after: 01_MASTER_PROMPT · Used in: every phase · Change only through `docs/DECISIONS.md` with a reason.

## 1. Locked technical decisions
| ID | Decision | Why |
|---|---|---|
| D-01 | **Unity 6000.3.9f1** (6.3 LTS). No editor upgrade without owner approval. **URP only.** | LTS stability; URP is the mobile pipeline. |
| D-02 | **Android:** arm64-v8a only, IL2CPP, **targetSdk 36** (Google Play requirement for new apps/updates since 2026-08-31), min API 26 [VERIFY vs Unity 6.3 minimum + 16 KB page-size support], Vulkan first with GLES3 fallback and a device blocklist. **iOS:** arm64, IL2CPP, min iOS 16, Metal, built with Xcode 26+ / iOS 26 SDK (Apple upload requirement since 2026-04-28). | Store rules + performance. |
| D-03 | **UI = UI Toolkit** (UXML/USS) for menus and HUD: text-based, diffable, AI-friendly. Vector icons via UI Toolkit SVG support (new in 6.3). uGUI only if something is impossible [VERIFY world-space UI support]. | Reproducible from code. |
| D-04 | **Input System** package; custom on-screen touch controls (UI Toolkit); optional gyro aim; fully editable HUD layout. | Mobile ergonomics. |
| D-05 | **URP Render Graph**, Forward path by default, ONE fullscreen `BodycamLens` pass, baked lighting + light probes, 1 shadow cascade on mobile. | Bandwidth on tile GPUs. |
| D-06 | **Networking = custom "NamuNet" over Unity Transport (DTLS)**. Not NGO, Netcode for Entities, Fish-Net, Mirror or Photon. Fallback if Gate 2 fails: Netcode for Entities. | Full control of authority, encryption, interest management; no per-CCU fees or paid rollback add-ons. |
| D-07 | **Dedicated server:** Unity Dedicated Server build (Linux x64, headless), one process per match, Docker image, 30 Hz. | Same gameplay code as client. |
| D-08 | **Backend:** .NET 10 LTS ASP.NET Core (modular monolith first), PostgreSQL 17+, Redis 7+ (or Valkey), S3-compatible storage, SignalR realtime. | One language family with Unity; shared DTOs. |
| D-09 | **Admin panel:** React + TypeScript + Vite SPA on a separate Admin API; WebAuthn passkeys + TOTP, RBAC, immutable audit log. | Owner-operable. |
| D-10 | **Hosting:** Tier 1 = Docker Compose + own `fleet-manager` on VPS/bare metal; Tier 2 = Kubernetes + Agones when concurrency demands. **Unity Multiplay Game Server Hosting ended 2026-03-31: never depend on it.** | Self-hosted per owner request. |
| D-11 | **Blender:** pin one LTS in `Tools/blender.version` (4.5 LTS unless a newer LTS exists [VERIFY]); FBX export; headless scripts only. | Reproducible art. |
| D-12 | Repo: `/Client` (Unity) `/Server` (.NET) `/AdminWeb` `/Infra` `/Tools/Blender` `/Docs`. Namespaces/assemblies `Namulinda.*`. | Clarity. |
| D-13 | **Monetization:** cosmetics + bodycam HUD themes + Feed Pass. No pay-for-power. No paid randomized rewards. No ads. | Fairness + regulatory safety. |
| D-14 | **Voice:** quick-chat + ping wheel at launch; voice via self-hosted LiveKit (or Agora) in Phase 6 [VERIFY licensing]. | Scope control. |

## 2. Locked gameplay/network constants
| Constant | Value |
|---|---|
| Server tick | 30 Hz (33.33 ms); hit resolution sub-tick timestamped |
| Snapshot tiers | T0 self+squad 30 Hz · T1 ≤150 m 20 Hz · T2 150–400 m 10 Hz · T3 400–800 m 5 Hz (vehicles/loud events) · >800 m none |
| Interpolation delay | 100 ms (adaptive 66–150) |
| Lag-comp rewind cap | 250 ms · playable RTT ≤300 ms (warn at 200) |
| Bandwidth budget (per client) | down avg ≤10 KB/s (peak 24) · **Data Saver** avg ≤6 KB/s · up avg ≤2.5 KB/s (peak 4). ≈15 MB per 25-min match, because prepaid mobile data is expensive |
| Players / match | Map A 48 (12×4) · B 32 · C 32 · D 16 · Skirmish 24 · Arena 6v6 |
| Match length | A 25:00 · B/C 20:00 · D 15:00 · Skirmish 12:00 |
| Squads | Solo, Duo, Squad (4). Party ≤4 |
| Currencies | **Scrip** (earned) · **Feed Credits / Cred** (purchased, cosmetics only) |
| Gear-value brackets (trader buy price) | Rookie <50,000 · Standard 50,000–249,999 · High Stakes ≥250,000 |
| Safe Case | 2 slots base, up to 6 via Safehouse upgrades (never sold) |
| Player capsule | 1.8 m tall, 0.35 m radius (crouch 1.2, prone 0.5) |
| Coordinates | metres, origin SW corner, x east, z north, y up. Grid A–L west→east (500 m), rows 1–12 north→south |
| Device tiers | **L**: 3 GB RAM, Mali-G52/Adreno 610/PowerVR GE8320 class, 30 fps · **M**: 4–6 GB, Adreno 618–642/Mali-G76+/A12–A14, 45–60 fps · **H**: 8 GB+, Adreno 730+/A15+, 60–90 fps |
| Brand palette | Night Slate `#0D1117` · Laterite `#C8461F` · Sun Gold `#F2B33D` · Feed Cyan `#35E0D2` · REC Red `#FF2E3A` · Bone `#ECE7DB` · Verdant `#3DBB6D` |
| Fonts (OFL) | Headings **Chakra Petch** · body **Barlow** · data/OSD **JetBrains Mono** |
| Item IDs | `snake_case` with prefix: `wpn_`, `att_`, `amo_`, `arm_`, `med_`, `gad_`, `veh_`, `cos_`, `mat_`, `key_` |
| Persisted IDs | UUIDv7 · in-match entity IDs: uint16 (players/AI) / uint32 (others) |
| Time | UTC ISO-8601 everywhere; monotonic server tick inside matches |

## 3. Unity layers
`Default, Player, AI, Hitbox, Vehicle, World, Loot, Interactable, Trigger, Water, UI` (create by script; collision matrix in code).

## 4. Repo hygiene
Git LFS for `*.fbx *.png *.psd *.wav *.ogg *.blend`; ignore `Library/ Temp/ Logs/ Builds/`. `docs/LICENSES.md` logs every third-party asset/package with licence and source.

## 5. Open questions for the owner (defaults apply until answered)
1. Studio name / bundle IDs? default `com.yourstudio.namulinda`.
2. Where is your main player base? (drives server regions) default 1 EU + 1 Africa region [VERIFY provider availability].
3. Hosting budget at launch? default Tier 1, 2 game hosts.
4. Age-rating target? default 16+/Teen: realistic violence, mild blood, no gambling.
5. What does "Namulinda" mean to you? default: a fictional basin-city name; lore claims no real-language etymology.
6. Voice chat at launch? default no (Phase 6).
7. Art sourcing (in-house AI-assisted, marketplace, freelancers)? default Tier 0/1 + marketplace polish.
8. Platform order? default Android beta first, iOS TestFlight in parallel.
9. Launch regions? default global except where law restricts.

## 6. Legal / IP / safety guardrails
- Do not use "Metro Royale", "PUBG", "Bodycam" or any of their art, audio, maps, code or trademarks in the shipped game. Mechanics inspiration only.
- Real firearm *types* are fine; do not reproduce manufacturer marks. Weapon names in this pack are original.
- Fictional setting: no real conflicts, real ethnic/national groups as enemies, or real atrocity references. No dismemberment/gore; mild blood only.
- Third-party assets/packages: permissive or purchased, logged in `docs/LICENSES.md`. AI-generated assets: log tool, prompt, date, terms; final shipped art should be human-reviewed.
- Privacy: data minimization, age gate (13+; higher where law requires), in-app account deletion (Apple requirement), consent flows, privacy policy in stores.
- No real-money gambling mechanics; no paid randomized rewards.
- Anti-cheat must be proportionate: only what stores allow and the privacy policy discloses; no scanning of unrelated user files/apps.
