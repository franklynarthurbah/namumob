# MAP C — SALTMARSH HARBOR (tidal wetlands, ship graveyard, offshore rig)
> Read after: 04_MAPS/00 and Map A files (same JSON schema for `MAP_C/layout.json`) · Phase 11 content
> 4000×4000 m (8×8 cells) · 32 players · 20 min · elevation −2…40 m · default light Dawn/Dusk, fog common

## 1. Identity
Boats, boardwalks and a colossal ship graveyard. The map **changes with the tide**, so routes flip: flats are shortcuts at low tide and lakes at high tide.

## 2. Tide cycle (server-authoritative, deterministic)
| Time | State |
|---|---|
| 00:00–05:00 | **Low** (flats walkable, mud slows −25%) |
| 05:00–06:00 | rising (warning horn, water climbs 0 → +1.2 m) |
| 06:00–10:00 | **High** (flats 1.2 m deep: swim 2.2 m/s, skiffs cross straight, raised boardwalks only) |
| 10:00–11:00 | falling |
| 11:00–15:00 | Low · then repeats every 10 min |
Water height is a replicated scalar; buoyancy and depth-based movement use it. Rig causeway is submerged at High (boat/swim access only).

## 3. Districts
| District | Center | Size | Notes |
|---|---|---|---|
| **Lagoon Village** | 1000,1000 | 900×700 | stilt houses, boardwalks, market boats |
| **Cannery Row** | 2200,1400 | 700×500 | brick/steel canneries, piers |
| **Ship Graveyard** | 2800,600 | 1000×600 | rusting hulls, cranes, dry dock |
| **Salt Pans** | 1400,2600 | 900×700 | white evaporation ponds, dykes, glare |
| **Mangrove Maze** | 600,2200 | 700×900 | dense roots, low visibility, skiff channels |
| **Old Ferry Terminal** | 3200,2000 | 500×400 | ramps, ticket halls |
| **Lighthouse Rig** | 3400,3400 | 500×500 (offshore) | jack-up rig, 400 m off the coast |
Routes: Boardwalk Loop · Dyke Road (Salt Pans) · Shore Drive · water lanes. Stunt lines: **Wreck Hop** (hull-to-hull), **Dyke Leap**, **Bridge Gap** Send-Off.

## 4. Bases
### C1 DRYDOCK LEVIATHAN (2800,600 · risk 5 · boss CAPTAIN)
A 300 m tanker in a giant dry dock. Decks: main deck (containers/cranes), superstructure (7 floors), engine room, 3 cargo holds (flood at High), bridge (sniper), mooring gallery. Modules 4×4 m deck pieces. Approaches: quay ramps, dock crane rope, water via lock gate. Garrison ≈16 (Guards + 4 drones); CAPTAIN on the bridge (heavy armor, flare mortar zones). Loot: T5 vault in the engine room, ship's safe, hero cargo.
### C2 CANNERY ROW COMPLEX (2200,1400 · risk 4 · boss SALTER)
Canneries with conveyor lines, **cold storage** (dark, −10% speed, +30% loot chance), boiler room (steam vents), pier warehouses. Approaches by boardwalk, boat, or roof ramp. Garrison ≈12; SALTER (thrower of salt-bombs: blind 2 s, heavy armor). Loot: food/material valuables, medical cache, T3–4 weapons.
### C3 LIGHTHOUSE RIG (3400,3400 · risk 5 · boss DRILLER)
Jack-up rig: 3 legs with ladders, main deck, living quarters (3 floors), drilling floor with derrick, crane, helideck (EB "Rig Helideck"). Connected by a causeway (Low tide only) or boat/swim. Storm lightning strikes near the rig every 40 s (telegraphed 3 s, 80 dmg radius 6 m). Garrison ≈12 + DRILLER (cable-winch pull-in, shotgun). Loot: rig safe, T4 sniper crate, Halo core chance 10%.

## 5. Extraction pool (8; 3 active from 06:30)
Rig Helideck · Ferry Terminal Ramp · Lagoon Dock · Salt Pans Jetty · Cannery Pier · Graveyard Crane Platform · Mangrove Skiff Channel · **Bridge Gap Send-Off** (bike over a ruined bridge, 34 m).

## 6. Hazards
Tides (above) · mud (grip 0.5, −25% speed) · fog (AI vision −25%, sound carries) · storm lightning near the rig · deep water gear rule: >45 kg swim −30%.

## 7. Blender kits
Marsh kit (stilt houses, boardwalks, dykes, salt-pan berms), mangrove (roots mesh 600 tris + impostors), Ship kit (hull sections, deck plates 4×4 m, hatches, bulkheads, pipes), Rig kit (truss legs, decks, derrick, crane, helideck), cannery kit (conveyors, vats, cold-room doors). Water: URP simple water with foam, depth fade, tide-driven height; tier L flat shader.

## 8. AI/loot/vehicles
AI ≈65 (≤28 active). Loot tiers 2–5; ≈1,000 containers. Vehicles: 14 skiffs, 8 bikes, 4 quads, 2 buggies, 1 armored boat at the Ferry Terminal.

## 9. Acceptance
Tide state is a pure function of match time · at High tide no player can walk through deep water without swim rules · skiff buoyancy stable at 30 Hz with 4 sub-steps · rig causeway collision toggles server-side.
