# "Bodycam" (Reissad Studio) — analysis for Namulinda
> Read after: 01_METRO_ROYALE_ANALYSIS · Used in: Phases 1–2 (camera/combat), Phase 5 (Arena mode)
> Reference video (title only readable): "The Most Realistic FPS Game is Absolutely Savage..." — mentions gun game, trench warfare and drones.

## 1. Facts
French indie studio Reissad Studio; Unreal Engine 5; Windows; Early Access since 2024-06-07. Historically listed modes: Body Bomb, Team Deathmatch, Versus, Hardpoint, Gun Game, Deathmatch, Zombies; the Steam page currently highlights Deathmatch, Team Deathmatch and Wingman. The **v0.8 "Locked & Loaded"** update (announced for 2026-09-02) brings redesigned loadouts, rebuilt weapon animations (stress-influenced reloads), updated environmental audio, the **Trenches** map (tight trench CQB plus long open sightlines), **FPV drones** and **RC ground vehicles**.

## 2. Signature systems
- **True body-cam view:** the camera is body-mounted, not head-centred; fisheye distortion, chromatic aberration, sway, sensor noise, exposure shifts, some motion blur; **faces pixelated** like redacted footage.
- **Minimal HUD:** no crosshair or ammo counter; feedback from animation and sound.
- **Weapon handling:** inertia and procedural recoil; weapons do not track screen centre; **shots leave the barrel**.
- **Ballistics/injury:** short projectile travel time; headshots lethal; limb hits can cause **hemorrhage** with a blood trail; ragdolls.
- **Audio as tactics:** reverb and occlusion make space readable by ear.
- **Short lethal fights**, callouts and clutch plays.

## 3. Mobile adaptation
| Bodycam feature | Namulinda mobile adaptation | Reason |
|---|---|---|
| No crosshair | 3 presets: **Feed** (none; tiny dot only in ADS), **Assist** (default, faint dot), **Classic** | Touch aiming needs feedback |
| No ammo counter | Diegetic: weapon LED / wrist readout + audio; optional numeric | Keep immersion, stay accessible |
| Shots from muzzle | Deterministic **WeaponPoseSolver** shared by client and server (see 02_GAME_DESIGN/03) | Server-verifiable, still muzzle-true |
| Strong fisheye | Effective ~100° feel; slider; comfort mode | Motion sickness, thumb readability |
| Motion blur | Off on tiers L/M; light directional streak on H while sprinting | GPU bandwidth |
| Ragdoll | Server death poses from anim set; client ragdoll cosmetic only, pool ≤8 | Netcode cost |
| Pixelated faces | LOD-friendly mosaic on head submesh (≤15 m), toggle | Cheap, on-brand |
| Occlusion audio | 3-level raycast occlusion + optional visual footstep cue | Speakers/no headphones |
| Reactive reloads | 3 variants (calm/stressed/injured) via animation layer blend | Feel with low animation cost |
| High lethality | Lower TTK than typical mobile BR but no random one-shots (armor classes) | Fairness on high latency |
| Hemorrhage trail | Bleeding leaves decals visible ≤12 m for 30 s | Tracking gameplay |
| Modes | **Bodycam Arena:** TDM 6v6, Hardpoint, Gun Game, Wingman 2v2 (generic names) | Quick sessions |
| FPV/RC drones | Post-launch **Recon Drone** (scout) countered by **Signal Jammer** | Scope control |

## 4. Authenticity vs playability
Player setting **Feed Authenticity** 0–100 (Off / Light / Full). Default Light on tier L, Full on tier H. All effects scale with it. A **photosensitive-safe** toggle removes flashes/tearing/strobes.

## 5. Do NOT copy
Bodycam's name, logo, maps, weapons, sounds, animations, HUD or code.
