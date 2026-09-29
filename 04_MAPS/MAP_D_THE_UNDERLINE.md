# MAP D — THE UNDERLINE (metro CQB)
> Read after: 04_MAPS/00 and Map A files (JSON schema for `MAP_D/layout.json`) · Phase 11 (Underline mode) · Also the visual inspiration for Metro-style tunnel play; all names original
> Footprint 1500×1500 m, 3 levels (−1, −2, −3), floor-to-floor 6 m · 16 players (4 squads) · 15 min · ≈400 containers · cells 250 m (6×6) streamed by level

## 1. Identity
A blacked-out metro network below Namulinda: tight lanes, sound-first combat, light management and a bodycam **IR mode**. Fast, intense, high value per minute.

## 2. Layout
Three lines meet at **NEXUS TERMINAL** (750,750), a four-level atrium (40 m tall):
- **Red Line** W–E: Foundry (150,750) — Nexus — Market Gate (1350,750)
- **Gold Line** N–S: Spire Cut (750,1350) — Nexus — Harbor Cut (750,150)
- **Cyan Line** diagonal: Stadium Cut (1300,1300) — Nexus — Mercy Cut (200,200)
Level −1: concourses, ticket halls, shops. Level −2: platforms and tunnels (200–400 m between stations). Level −3: maintenance corridors, pump rooms, substations, flooded depot. Each station is a POI; 6 surface **elevators** (one per outer station).

## 3. Rooms/"bases"
| ID | Place | Position | Boss | Notes |
|---|---|---|---|---|
| **D1 NEXUS TERMINAL** | central atrium | 750,750 (120×120) | STATIONMASTER | escalator crossfires, clock tower, vault kiosk; garrison ≈12 |
| **D2 SIGNAL CONTROL ROOM** | Level −2/−1 | 1100,500 | DISPATCHER | controls lights, switches and the train; shooting the panel triggers blackout in that sector |
| **D3 FLOODED DEPOT** | Level −3 | 450,1100 | DREDGE | waist-deep water (slow), dry cat-walks, rail carts; ambush pipes |
Sub-station objectives (4) restore lights per sector (25 s hold, noisy).

## 4. Systems
- **Darkness and IR:** emergency lights are red and sparse. Flashlights are visible and detected by AI (+50%). **IR mode** (toggle) adds 25 m visibility, greyscale-green grain, and drains the bodycam **battery** (8 min per raid of continuous IR): battery is a real resource.
- **Train:** a maintenance train runs the Red Line every 4 min (horn 6 s prior). Hits deal 200 dmg. Can be boarded for a fast ride.
- **Vehicles:** rail bike (60 km/h, rails only, 1–2 seats), handcar (4 seats). Free bikes can enter wide tunnels at low speed; ramps in Nexus support stunts (Hype ×1.2 bonus underground).
- **Flooding event:** at 08:00 pump failure floods Level −3 to knee height, at 11:00 to waist (slows −20%/−40%).
- **Ventilation shafts:** vertical shortcuts with ladders (loud).

## 5. Extraction (4 active of 8 from 05:00)
Surface elevators (6; keycard or power) · Rail pod (Nexus) · **Emergency hatch** (any station; 40 s hold, very loud). Dimming equivalent: **Lockdown** sealing outer stations at 09:00, 11:30, 13:30 (shrinks playable area toward Nexus; sealed stations flood).

## 6. AI and loot
AI ≈60 (≤30 active), Halo-heavy: drones in tunnels, sentries at station gates, Guards in stations, Scrap Crews in maintenance. Loot tiers 2–4, density ≈270 containers/km²; hero spawns in D1–D3. Loot notes: cash/valuables at Nexus vault; weapons in Signal Control; materials in depot.

## 7. Blender/Unity kit
Metro kit: tunnel segments (straight 8 m, curved 15°/30°, junction, ventilation shaft), platform modules, escalators, ticket gates, carriages (12 m, 6k tris), trolleys, cables/pipes, fictional signage. Baked lighting with emissive fixtures; **max 4 real-time lights** (player flashlights) on tier M/H, fake cones on tier L; fog volumes and portal occlusion; reverb zones per space; level streaming per cell and per level.

## 8. Acceptance
Train timing deterministic · IR battery drains server-side and cannot be spoofed · flooded volumes slow movement by server rules · 16 players + 30 AI within 8 ms average server tick · tier L holds 30 fps in Nexus.
