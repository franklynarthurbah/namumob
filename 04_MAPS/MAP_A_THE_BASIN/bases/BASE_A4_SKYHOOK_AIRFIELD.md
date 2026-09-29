# BASE A4 — SKYHOOK AIRFIELD BASE (risk tier 4, boss SKYMARSHAL)
> Position (4900,4700) · footprint 420×300 m (runway 1,200 m E–W extends beyond) · elevation 60 m · Last Bird candidate `LB_SKYHOOK` (4700,4700)

## 1. Role
Open-ground fortress: long-range duels, bike speed lanes and aircraft-themed loot. The runway is the fastest bike strip on the map; the tower gives control of the whole airfield.

## 2. Approaches
1. **Airfield Road** (straight boost lane; exposed). 2. **Taxiway curve** from Solar Fields with hangar cover. 3. **Hangar-row step-up** (`R11` at (4800,4600), 16 m) onto Hangar A's apron roof. Exits: EP03 Skyhook Apron (4600,4600).

## 3. Layout
| Structure | Contents | Dimensions |
|---|---|---|
| **Control Tower** | 8 floors, glass cab at 34 m (sniper), stairs + hoist | 14×14 m, 42 m high |
| **Hangar A** | truss roof with catwalks (climbable), maintenance pits | 80×50 m |
| **Hangar B** | wrecked cargo plane (walkable fuselage, wing ramps), boss lair | 80×50 m |
| **Hangar C** | workshop, parts cages, second floor offices | 60×40 m |
| **Terminal** | 2 floors, check-in hall, baggage tunnels | 70×30 m |
| Fuel farm | 4 tanks (1 explosive) | 60×40 m |
| **Radar dome** | signal room with hero terminal | Ø18 m |
| Perimeter | fence, 2 gates, AA bunker | — |

## 4. Sightlines and chokes
Runway is a 1.2 km lane; only smoke and terrain dips break it. Tower dominates the west half. Hangar interiors are CQB with catwalk verticality. Baggage tunnels link Terminal ↔ Hangar C (flank route, 120 m).

## 5. Hazards
Open ground (no cover for 200 m in places) · fuel tank explosion · radar dome hums (masks footsteps within 30 m) · wing ramps allow stunt jumps into Hangar B (`Hype` bonus ×1.2).

## 6. AI (≈14 + boss)
2 Concession Guard squads (8) at Terminal/Hangar C, sentry at the tower base, 3 spotter drones (mark players for SKYMARSHAL), **SKYMARSHAL** (long-range bolt rifle) alternates between tower cab and Hangar B roof.

## 7. Loot (≈45 containers + 4 hero)
Weapon crate 4 · ammo 8 · general 10 · tool 8 · PC/file 5 · medical 4 · safe 2. Hero: **T4 sniper crate**, flight recorder valuable (18k), keycards ×2, pilot armor set (T3).

## 8. Blender kit
Airfield kit: hangar trusses (parametric spans), tower modules, runway lights, fences, aircraft wreck (cargo plane ≤8k tris), baggage carts, fuel tanks. Hero: cab interior, radar dome interior. Runway decals on trim/decal atlas.

## 9. Unity notes
Runway surface tag "tarmac"; taxiway markings use decals; far LOD impostor for hangars; wind audio zone; Last Bird landing script (server event + spline) reused for other maps.

## 10. Balance/telemetry
Clear 6–9 min; expected snipe deaths from tower 25%. Track: approach route, tower ownership time, runway ride speeds, jumps into Hangar B, Last Bird contested outcomes.
