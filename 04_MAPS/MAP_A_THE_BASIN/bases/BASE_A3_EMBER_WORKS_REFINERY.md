# BASE A3 — EMBER WORKS (risk tier 4, boss FOREMAN)
> Position (5000,1500) · footprint 420×360 m · elevation 25 m · Ember Belt · loot tier up to 4

## 1. Role
Industrial maze full of fire and pipes: the material/crafting hub (circuits, chemicals) with the strongest CQB and hazard play. Excellent for Kicker/Big Air routes across the tank farm.

## 2. Approaches
1. **Refinery Road** (straight boost lane from the west, exposed). 2. **Rail spur** from the east with cover in tank shadows. 3. **Pipe-rack gap** (`R10` Big Air at (4200,1450), 34 m) onto the process-unit roof deck. Extraction: EP05 Ember Rail Spur (5200,1900), Pod Ember.

## 3. Zones
| Zone | Contents | Dimensions |
|---|---|---|
| **Tank Farm** | 8 tanks Ø30–40 m; 3 red (explosive), 5 yellow (gas leaks); ladders to roofs | bunded area 200×140 m |
| **Process Unit** | 2-level pipe racks, catwalks, valve stations, heat exchangers | 160×90 m |
| **Control Building** | 2 floors, glass control room, server closet | 40×30 m |
| **Flare Stack** | 90 m stack, ladder to 60 m platform (sniper nest, 1 slot), roars | base 12×12 m |
| **Loading Racks** | tanker trucks, gantries | 80×40 m |
| Pump House · Maintenance shop · Cooling tower | small interiors, workbenches | ≈30×20 m |

## 4. Sightlines and chokes
Long lanes along pipe racks (40–60 m) broken by columns; flare platform covers the whole yard; tank roofs give 360° but are exposed to flare and process-deck. Chokes: rack crossovers, control building doors, tank ladders.

## 5. Hazards
Gas leak clouds ignite from muzzle flash/explosion: 5 dmg/s + 8 s burn · red tanks: 200 dmg, radius 12 m, 3 s fuse chain · valves can be shut (12 s) to stop leaks · flame vents on FOREMAN's platform (timed 8 s cycle) · steam vents mask vision.

## 6. AI (≈12 + boss)
2 Scrap Crews (6) roaming the yard, 1 Concession Guard squad (4) in the control building, 1 elite sniper on the flare, **FOREMAN** (heavy armor, LMG suppression, flame vents) on the process-unit centre platform.

## 7. Loot (≈45 containers + 3 hero)
Tool 14 · general 10 · ammo 8 · PC/file 5 · weapon crate 3 · safe 1 · medical 4. Materials-rich. Hero: **T4 LMG "Elephant"** crate, keycard (control room), chemical valuables.

## 8. Blender kit
Industrial kit: pipe segments (straight/elbow/T, 3 diameters), racks, valves, tanks (LOD tank 1.5k/500/80), stairs/ladders, catwalk sets, flare stack, tanker truck (3k). Hero: FOREMAN platform with vents, control room console. Use instancing for pipes; single atlas.

## 9. Unity notes
Gas volumes = server-side triggers with GPU-cheap particle sheets; explosion chain handled by server events; occlusion by zone; heat-haze shader only on tier H; audio: flare roar 3D loop (300 m), pipe hiss zones.

## 10. Balance/telemetry
Clear 7–10 min. Track: fire deaths, explosion kills (own vs enemy), flare-platform camp time, material yield per raid. If fire deaths exceed 20% of deaths, widen warning cues.
