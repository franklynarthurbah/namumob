# BASE A6 — HARBOR BASTION (risk tier 4, boss HARBORMASTER) — **Slice-01 base**
> Position (3500,700) · footprint 450×320 m · elevation 4 m · Harborline · Extraction EP06 Pier 9 (3900,600), Last Bird `LB_PIER9`

## 1. Role
The vertical-slice base: a container-yard maze, crane sniper nests and a water flank. It exercises everything early: bikes on quays, boats, streaming, AI garrison, loot, extraction and the Dimming edge (map south).
**Slice-01 scope:** Harbor Bastion + Pier 9 + Customs Office (3900,1000) + Fuel Depot (4300,1300) + Harbor Motor Pool (VZ_harbor) + lakeshore + ramps R01/R02.

## 2. Approaches
1. **Lakeshore Drive** from west/east (fast, exposed). 2. **Water**: skiffs from the Motor Pool or Stilt Quarter. 3. **Quay Big Air** (`R01` at (3550,760), 30 m across the slip to the pier) and **kicker** (`R02` at (3300,900)). Container stacks form a bike ramp chain on the north side (`R17` step-up at (3450,830), to be added).

## 3. Layout
| Element | Contents | Dimensions |
|---|---|---|
| **Container yard** | stacks 3 high, 12 lanes 6 m wide, gaps and ladders, 2 rooftop "bridges" | 200×120 m |
| **Customs Building** | 3 floors, glass front, baggage hall, safe room | 60×30 m |
| **Gantry cranes ×3** | rails 200 m; crane #2 operator cab at 32 m (sniper); moving hook hazard | rail gauge 25 m |
| **Dry dock** | empty dock with a sunken ship hull (hero) | 120×30×12 m deep |
| Warehouses 1–3 | racking rows, forklifts, offices | 70×40 m each |
| **Pier 9** | 260 m pier, boat slips, EP06 | 260×14 m |
| Harbor office · fuel shed | small interiors | ≈20×12 m |

## 4. Sightlines and chokes
Yard lanes are 6 m wide with T-junctions (ambush heaven); crane cab covers the quay and pier; pier is a 260 m shooting gallery (use smoke/skiffs); warehouses offer interior flank; dry dock ledge is a high-ground overlook.

## 5. Hazards and interactables
Water (swim, no sprint) · **crane hook** sweeps a rail every 40 s (30 dmg, knockdown) · alarm bell (gunfire triggers ambush trucks) · breakable container doors with loot · fuel shed explosion.

## 6. AI (≈12 + boss)
Scrap Crews on docks (6), Concession Guards in Customs (4), 2 **ambush trucks** (each drops 3 Scrap after alarm), **HARBORMASTER** in crane #2 cab (long-range rifle, calls trucks, descends to dock when flanked).

## 7. Loot (≈50 containers + 4 hero)
Container loot 20 (general/tool mix) · warehouse racks 12 · customs PC/safe 6 · ammo 6 · weapon crate 3 · medical 3. Hero: **sunken ship crate** (Legendary 10%), customs safe (valuables), T4 weapon spawn (cranes), keycard.

## 8. Blender kit
Harbor kit: shipping containers (4 colour variants, 120 tris, instanced), stacks generator, gantry crane (6k), dock walls, bollards, pier segments, warehouse shells, ship hull hero (8k), quay barriers. Water plane + shore foam decals handled in Unity. **Slice tier:** T1 everywhere, T2 for 10 hero props (crane cab, ship hull, customs lobby, dry dock rails, pier end, alarm bell, container doors, forklifts, ambush truck, harbor office).

## 9. Unity notes
Cells G–I × 10–12 only; server cell variants; water shader tier L = flat colour + foam; boat controller (skiff) = arcade water physics (raycast buoyancy points) sharing the vehicle sim; crane hook = server-driven kinematic hazard with replicated phase.

## 10. Balance/telemetry
Clear 6–9 min; median profit +18k Scrip at Standard; extraction rate 55% target. Track: approach route mix (road/water/ramp), crane cab deaths, ambush truck alarms, pier crossing deaths, Big Air R01 success rate, time to first extraction.
