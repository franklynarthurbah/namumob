# Map A — extraction, hazards, loot, spawns
> Read after: 01_OVERVIEW + 02_LAYOUT_DATA.json · Used in: Phase 3 (slice subset), Phase 7 · All tables are data (`Client/Assets/Data/MapA/*.json`), hot-reloadable from backend config.

## 1. Extraction pool (12; 4 active per match from 08:00)
| ID | Name | Type | Position | Play notes |
|---|---|---|---|---|
| EP01 | Reservoir Helipad | fixed | 2200,5450 | high ground, exposed southern approach |
| EP02 | Solar Landing | fixed | 3300,5250 | open field, very exposed; bikes shine |
| EP03 | Skyhook Apron | fixed | 4600,4600 | near boss SKYMARSHAL; hangar cover |
| EP04 | Stadium Roof Lot | fixed | 4600,3200 | multi-level parking, many angles |
| EP05 | Ember Rail Spur | fixed | 5200,1900 | pipe cover, fire risk |
| EP06 | Pier 9 | boat | 3900,600 | water approach, crane sniper |
| EP07 | Mercy Roof | fixed | 1000,1800 | rooftop, stairwell choke |
| EP08 | Stilt Quarter Dock | boat | 600,600 | boardwalks, flanking by skiff |
| EP09 | Highland Cable Car | fixed | 1300,4500 | ridge, slow ascent |
| EP10 | Old Market Rooftop | fixed | 1600,3000 | ladders/zip; CQB |
| EP11 | West Send-Off Ramp | send_off | 240,3300 | stunt extraction (bike only) |
| EP12 | East Send-Off Ramp | send_off | 5760,3000 | stunt extraction (bike only) |
**Selection (seeded by matchSeed):** exactly 4 active; ≥1 in each of north (z≥3000) and south halves; pairwise distance ≥1,500 m; ≤1 send-off; ≥1 non-boat fixed. Re-roll at 18:00 replaces 2 (never the last remaining fixed if it is the only one in the safe zone). Each EP: radius 8 m, hold 20 s, contested pause, siren audible 300 m, min squad presence rule off.
**Rail pods:** need 2 of 4 substations (S1–S4) activated (each 25 s hold, generates noise/light) → pods at Central/Stadium/Ember stations become boardable for 30 s every 3 minutes. **Boats:** at Pier 9 and Stilt Dock, 4 seats, 25 s boarding. **Last Bird:** one of `LB_SKYHOOK / LB_PIER9 / LB_SOLAR`, revealed at 22:00, must be inside the final safe area (else the nearest).

## 2. Hazards
| Hazard | Where | Effect |
|---|---|---|
| **Dimming front** | global | see `02_GAME_DESIGN/02`; drones from Phase 2 |
| **Halo pulses** | dead grid nodes (Halo, Spire, Ember) | every 30–90 s EMP within 60 m: bodycam glitch 3 s, gadgets off 5 s |
| **Fire/gas** | Ember Works | gas leaks ignite from muzzle flash/explosions: 5 dmg/s + 8 s burn |
| **Live floors** | Halo basement | 8 dmg/s, visible arcs, switchable at breaker |
| **Deep water** | lake, canal | swim 2.2 m/s, no sprint, weapon lowered; heavy gear (>45 kg) −30% swim speed |
| **Canal drop** | Send-Off failure | fall damage + bike lost |
| **Weather** | global | rain: grip ×0.6, AI vision −20%, audio masked; haze: vision −25% |
| **Night variant** | event | ambient dark, flashlights/IR matter |

## 3. Loot
**Containers (≈2,600):** general crates/wardrobes 900 · toolboxes/sheds 700 · medical cabinets 380 · ammo boxes 300 · file cabinets/PCs 180 · weapon crates 120 · safes 20 · boss crates 6. Containers are instantiated by proximity from seeded tables; unopened contents are never sent to clients.
**Density (containers per km²):** Spire 90 · Old Market 70 · Mercy 65 · Rail Yard 60 · Ember 60 · Harbor 55 · Skyhook 55 · Stadium 50 · Highland 45 · Stilt 40 · Dam 35 · Solar 20 · outskirts 12. Bases add 40–70 containers + 3–5 **hero spawns** each.
**Rarity weights by loot tier (%):**
| Tier | Common | Uncommon | Rare | Epic | Legendary |
|---|---|---|---|---|---|
| 1 | 80 | 18 | 2 | 0 | 0 |
| 2 | 55 | 35 | 9 | 1 | 0 |
| 3 | 30 | 40 | 24 | 5.5 | 0.5 |
| 4 | 0 | 30 | 45 | 22 | 3 |
| 5 | 0 | 0 | 35 | 50 | 15 |
Container type biases category (e.g. medical cabinet 70% medical; weapon crate 85% weapons/attachments; safe 60% valuables). Keycards: 12 per match, guaranteed via boss/hero spawns. **Signal Caches:** #1 at 03:00, #2 at 12:00 in random airdrop zones; each has ≥2 Epic and 10% Legendary, visible smoke 300 m, guarded by one T3 group.

## 4. Spawns
**Players:** one of 16 dropship lines (through ≤800 m of centre); squads jump together (leader marks); auto-eject at 02:10; landing is free-choice. Parachute glide range covers ≈3 km from ship line.
**AI:** base garrisons + POI crews + roaming crews (≈150 total, ≤60 active). Elite Waves at 04:00 and 12:00 wake elite squads at 2 random bases each. Bosses spawn at their bases (Rookie: 06:00).
**Vehicles (≈40 bikes, 8 quads, 8 buggies, 4 jeeps, 6 skiffs)** at `vehicle_spawn_zones`; respawn once after destruction at +5 min.
**Ambient:** server-driven distant gunfire events from AI fights, radio chatter lines, Halo status lights, wind/weather.

## 5. Acceptance
Selection rules produce valid sets over 10,000 seeds · loot tables sum to 100% · no container content leaves the server before the player is within 6 m · Signal Cache timing exact ±1 tick · EP timers cannot be advanced by client packets.
