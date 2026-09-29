# NAMULINDA fusion design, feature matrix, risks, sources
> Read after: 01 and 02 · Used in: all phases as intent reference

## 1. Pillars
1. **Every raid is a recording.** The screen is a Kestrel bodycam feed; footage is reviewable and shareable ("Reels").
2. **Greed vs. exit.** Loot value rises with time and danger; extraction is always a decision.
3. **The whole basin is your track.** Huge maps, stunt bikes, ramps and rooftops make travel skillful and fun.
4. **Fair power.** Skill and time decide fights; money buys style, never stats.
5. **Runs on real phones.** Tier L devices (3 GB RAM) are a first-class target; data-light netcode.

## 2. Unique selling points
Diegetic bodycam UI tied to real signal quality · stunt-bike traversal with Hype→Boost · huge streaming map with authored bases · fair cosmetics-only monetization · low-end device support · original setting and identity.

## 3. Feature matrix
| Feature | Metro Royale | Bodycam | Namulinda |
|---|---|---|---|
| Extraction loop, stash, traders | Yes | No | **Yes** |
| AI factions + bosses | Yes | Zombies mode only | **Yes** |
| Bodycam optics/OSD | No | Yes | **Yes (diegetic, signal-linked)** |
| Minimal HUD | No | Yes | **Presets** |
| Muzzle-true weapons | No | Yes | **Yes (deterministic)** |
| Vehicles | Yes | No | **Yes + stunt system** |
| Huge open map | Medium | Small arenas | **6×6 km streaming** |
| Arena modes | No | Yes | **Bodycam Arena** |
| Mobile | Yes | No | **Yes** |
| Monetization | Currency/pass | Premium game | **Cosmetics + pass, no P2W** |

## 4. Risk register
| ID | Risk | Mitigation | Gate |
|---|---|---|---|
| R1 | 48 players on 36 km² is heavy on phones | Slice first, interest management, AI LOD, streaming cells, tiered player counts | G2, G7 |
| R2 | Custom netcode complexity | Vertical slice in Phase 2 with fallback to Netcode for Entities | G2 |
| R3 | Art volume | Tier 0/1/2 art plan, kit-built modules, marketplace polish | G3, G6 |
| R4 | Cheating | Server authority, PVS-based replication, attestation, detection, bans | G9 |
| R5 | IAP/store rejection | Compliance checklist, no randomized paid rewards, server validation | G8 |
| R6 | Hosting cost | Tier 1 hosting, per-match container packing, autoscale rules | G4, G10 |
| R7 | Legal/IP | Originality rules, asset license log, trademark check | every phase |
| R8 | Low-end performance | Budgets per tier, profile on device every phase | every gate |
| R9 | Bike netcode | Kinematic arcade model, not PhysX rigidbody | G2, G3 |
| R10 | Content volume | Data-driven items, modular kits, staged map rollout (A first) | G6, G7 |

## 5. Owner notes
Add your own observations from the four videos below (timestamp + idea).

## 6. Sources and verification list
**Read as titles/descriptions only:** the four YouTube videos supplied by the owner.
**Written sources used:** Bodycam — Wikipedia article, Reissad Studio site, Steam page, 80.lv developer interview (2023), enduins/dlcompare news (Aug–Sep 2026). Metro Royale — Baidu Baike entry, Sportskeeda guides, community wikis, several commercial guide sites (treat their numbers as unverified). Unity — Unity 6.3 LTS announcement (2025-12-04) and docs, multiplayer overview docs, Multiplay shutdown notices (service closed 2026-03-31), In-App Purchasing changelog. Stores — Google Play target-API page (API 36 from 2026-08-31), Apple developer news (Xcode 26/iOS 26 SDK from 2026-04-28; new age ratings 13+/16+/18+). Netcode options — Fish-Net docs (RollbackManager is a Pro feature), dev.to networking-stack comparison (2026).
**[VERIFY] at build time:** package versions and Unity 6.3 minimum OS levels · APV/GPU Resident Drawer behaviour on mobile · UI Toolkit world-space support · Blender current LTS · Play Integrity and App Attest plugin options · LiveKit/Agora licensing · Metro Royale lobby size · store policies on the day of submission · hosting provider regions and prices.
