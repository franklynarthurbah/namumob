# BASE A1 — HALO CONTROL CENTER (risk tier 5, boss ARCHITECT)
> Position (900,5350) · footprint 220×180 m · ridge elevation ≈230 m · loot tier up to 5 · Read with `02_LAYOUT_DATA.json` (`A1_halo`) and `02_GAME_DESIGN/07`.

## 1. Role
The city's dead brain: highest-risk PvE fortress and the only reliable source of **Halo Cores**. Squads come for cores, then race the Dimming to a nearby extraction (EP01 Reservoir, EP09 Cable Car).

## 2. Approaches (all bike-usable)
1. **Ridge Road** switchbacks from the south-east: fast, exposed to plateau snipers. 2. **West terraces**: footpaths and retaining walls, best cover. 3. **Service ramp** (`R15`, Big Air, to be added at (1050,5250) heading 300°): clears the 12 m dry moat onto the parking terrace. Gate bridge is a choke covered by 2 sentries.

## 3. Layout (floor height 4.5 m; atrium 13.5 m)
| Level | Contents | Key dimensions |
|---|---|---|
| Exterior | fence + dry moat, gatehouse (S), parking terrace (SE), service road ring | moat 12 m wide |
| **Roof plateau** | 24-petal dish array (Ø 60 m), catwalks, 4 support towers (+18 m) with sniper nests | plateau 120×90 m |
| L3 | Operations gallery (glass, open plan) with mezzanine ring | 60×40 m |
| L2 | Labs, comms lab, offices | 20×14 m rooms |
| L1 | Lobby/atrium with sky bridge; security desks | atrium 36×28 m |
| **B1** | Server hall (rack rows, 2.4 m aisles), breaker room B1-E | 80×50 m |
| **B2** | Generator hall + **Core Chamber** (boss arena) | 50×36 m / 40×30 m |
Stairs: 2 main cores + 2 fire stairs; 1 freight lift to B2 (loud); roof reachable by vent ladder or dish maintenance stairs.

## 4. Sightlines and chokes
Atrium is a vertical crossfire (L1–L3). Server hall lanes are 60 m long with rack cover every 6 m. Plateau overlooks 1.5 km westward. Chokes: gate bridge, atrium stairs, freight lift, B2 blast door.

## 5. Hazards and interactables
Halo EMP pulse every 30–90 s (bodycam glitch, gadgets off) · **live floors** in B1 (switch at B1-E breaker) · drone docks (destroy to stop respawns) · **Core door**: keycard from ARCHITECT's drop or 40 s hack (noisy) · generator restore gives lights + opens B2 blast door.

## 6. AI garrison (≈16 + boss)
2 sentries (gate), 6 Halo drones (roaming L1–L3), 2 Concession Guard squads (8) on L1–L3 patrols, 2 wardens (B1). **ARCHITECT** in the Core Chamber (drone swarm of 6, EMP every 30 s, retreats behind server-rack cover). Elite squad (4) wakes at 04:00 or 12:00.

## 7. Loot (≈60 containers + 5 hero spawns)
General 14 · tool 12 · PC/file 18 · weapon crate 4 · safe 2 · ammo 6 · medical 4. Tier 3 above ground, 4 in B1, 5 in B2. Hero: **Halo Core** (boss), vault-grade keycard, T4 weapon crate, prototype scanner, "Architect's ledger" (25k).

## 8. Blender kit and hero props
Kit: Halo control kit (glass curtain panels, stepped terraces, cable trays, catwalks, racks). Hero props (≤10): dish segment, atrium halo-ring emitter, control desk, server rack row (≤600 tris/rack), generator, core pedestal, gate turret, drone dock, breaker panel, keycard reader. Single-vantage budget ≤120k tris (tier M).

## 9. Unity notes
Additive scenes: `MapA_Base_A1_Halo` (exterior) + streamed floor scenes `_L1_L3`, `_B1`, `_B2`; occlusion portals at doors; audio zones (large hall reverb atrium, dry hum server hall); drones use steering (no NavMesh); NavMesh off-mesh links at stairs/lift.

## 10. Balance and telemetry
Squad clear time 9–14 min; median time-in-base 7 min; expected haul 120–220k Scrip + core. Log: entry route, first alarm time, deaths by cause (drone/EMP/guard/PvP), core extraction rate, roof camp duration (>90 s flagged for design review).
