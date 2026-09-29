# MAP B — REDLINE RIDGE (highland mining basin)
> Read after: 04_MAPS/00 and Map A files (same data schema: author `MAP_B/layout.json` following `MAP_A/02_LAYOUT_DATA.json`) · Phase 11 content (post-launch)
> 4000×4000 m (8×8 cells) · 32 players · 20 min · elevation 0–420 m · default light Noon/Dusk · laterite red earth, gravel roads

## 1. Identity
An open-pit mining region above the city: terraces, haul roads, rail viaducts, wind farms and a radio summit. Built for **enduro/quads, long jumps and vertical fights**. Unique features: blast events, dust storms, the moving **Redline Freight** train.

## 2. Districts (center x,z · size · notes)
| District | Center | Size | Notes |
|---|---|---|---|
| **Deepcut Pit** | 2000,2000 | Ø900, 120 m deep | spiral haul road, 14 terraces, Big Air jumps across switchbacks |
| **Terrace Farms** | 800,1200 | 1200×900 | contour terraces, retaining walls = cover, bikes follow contour lines |
| **Viaduct Junction** | 3000,2800 | 900×600 | rail viaduct 300 m long, 40 m high, junction yard |
| **Signal Ridge** | 3300,3500 | 700×600 | 420 m summit, radio station |
| **Windfarm Ridge** | 1000,3400 | 1400×500 | 24 turbines; wind noise masks footsteps within 40 m |
| **Smelter Town** | 2500,700 | 900×700 | worker housing, smelter stacks, CQB streets |
| **Rail Depot** | 1600,2800 | 600×400 | train sheds, turntable |
Routes: Haul Road (spiral) · Ridge Line (E–W along the summit) · Terrace Track (contours) · Valley Line (rail service road). Stunt lines: **Pit Leap** (40 m corner gap), **Switchback Chain**, **Viaduct Approach**.

## 3. Bases
### B1 DEEPCUT MINE COMPLEX (2000,2300 · risk 4 · boss OVERSEER)
Crusher plant (3 floors, conveyor bridges), headframe towers (50 m, climbable), ore bins, maintenance yard, **adit** into a 800 m underground rail-cart network (dark; cart vehicles on rails, 4 seats). Approaches: haul road from the north, pit terraces from the south (rock cover), Big Air across the pit corner. Hazards: blast events, cart collisions, falling rock. Garrison ≈14 (Scrap Crews + Guards + 4 drones). OVERSEER: heavy armor, pneumatic hammer (short-range stagger), calls explosive charges on the terraces. Loot: materials/ore valuables, T4 shotgun crate, keycard.
### B2 VIADUCT JUNCTION FORTRESS (3000,2800 · risk 4 · boss SIGNALMAN)
Signal boxes (3), junction yard with wagons, viaduct with maintenance catwalks under the deck (flank routes), station platform, water tower (sniper). **Redline Freight** crosses every 6 min (30 s stops at both ends): players can board its flatcars and fight/extract (see §5). SIGNALMAN controls track switches (redirects the train) from the main signal box. Garrison ≈12. Loot: wagon crates (mixed), signal-box safes, T4 rifle crate.
### B3 SIGNAL RIDGE RADIO STATION (3300,3500 · risk 5 · boss WATCHER)
Lattice mast (60 m, climbable), bunker (2 floors), generator shed, dish farm, helipad (EB3). 2 km sightlines. Approaches: switchback bike route, hang-line from Windfarm Ridge (BASE rigs), scramble up the north scree. WATCHER commands 6 drones and can jam player minimaps for 10 s (counter: destroy the antenna). Garrison ≈12. Loot: comms valuables, Halo core chance (5%), T5 cache (small chance).

## 4. Extraction pool (8; 3 active from 06:30)
EB1 Rail Siding (train pod) · EB2 Smelter Yard · EB3 Signal Ridge Helipad · EB4 Windfarm Substation · EB5 Terrace Cable Car · EB6 **Freight Extract** (moving) · EB7 **Pit Leap** Send-Off (bike) · EB8 Depot Turntable Lift. Rules as Map A §1 (≥1 per half, ≥1,200 m apart).

## 5. Unique mechanics
- **Blast Horn:** 20 s warning, then terrace blasts create rockfall on marked terraces (60 dmg, 10 s). Server-timed every 4 min.
- **Dust storm:** every 6 min for 60 s: vision −30%, audio masking, bodycam extra grain.
- **Redline Freight:** 12-car train on the viaduct; rideable; boarding at the two stops; `Freight Extract` = ride it to the east tunnel (EB6) then extract at arrival if still aboard. Train hits players on tracks for 200 dmg (horn 6 s before).
- Gravel grip ×0.7; pit edges: falls are lethal >10 m.

## 6. Blender kits
Mining kit (conveyors, crushers, headframes, ore bins, haul-truck prop 5k, rails, cart 1.2k, viaduct segments, lattice mast, wind turbine 1.5k LOD0/500/impostor), terrace and pit generators (`terrace_gen.py` contour-based; `pit_gen.py` spiral haul road with radius/slope limits ≤14%).

## 7. Loot/AI
Loot tiers 2–4; ≈1,100 containers; AI ≈70 (≤30 active). Vehicles: 20 bikes (weighted Kicker/Bruiser), 6 quads, 3 buggies, 2 jeeps, carts underground.

## 8. Acceptance
Freight schedule identical on server/client (deterministic) · Pit Leap jump test at 88 km/h succeeds · blast rockfall damage server-authoritative · flythrough within tier budgets (`00 §9`, scaled to 16 km²).
