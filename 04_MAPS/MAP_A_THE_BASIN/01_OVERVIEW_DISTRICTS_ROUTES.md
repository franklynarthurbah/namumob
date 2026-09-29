# MAP A — "THE BASIN" — overview, districts, routes
> Read after: 04_MAPS/00 · Data: `02_LAYOUT_DATA.json` (authoritative coordinates) · Used in: Phase 3 (Slice-01 = harbor corner), Phase 7 (full)
> Coordinates: metres from SW corner (x east, z north). Grid A–L (west→east), rows 1–12 (north→south).

## 1. Facts
6000×6000 m · 48 players (12 squads) + ≈150 AI · 25 min · elevation 0–260 m · lake on the south edge · ≈900 buildings (≈180 enterable) · 6 base complexes · ≈40 POI compounds · 1 tunnel line + 1 skyrail loop · 2 Send-Off Ramps · default light Dusk (variants Dawn/Noon/Night), 25% chance of rain/haze.

## 2. Districts
| District | Center (x,z) | Size m | Elev m | Character | Loot tier | Notes |
|---|---|---|---|---|---|---|
| **Spire District** | 3000,3800 | 1000×900 | 45 | high-rise CBD, plazas, sky bridges | 3 | Vault Tower (A2); rooftop fights; BASE-jump rigs |
| **Old Market Quarter** | 1700,2900 | 900×800 | 38 | dense low-rise, alleys, market halls | 2 | CQB; rooftop ladders; EP-10 |
| **Rail Yard 9 & Central Station** | 3000,2600 | 1100×500 | 40 | open freight yard, station, skyrail hub | 3 | Line C tunnel entrance; ramps over tracks |
| **Harborline** | 3400,900 | 1500×800 | 3–8 | port, warehouses, cranes | 3 | Harbor Bastion (A6); boats; Slice-01 |
| **Mercy Quarter** | 1000,1700 | 700×600 | 30 | hospital campus, parking decks | 3 | Mercy General (A5) |
| **Ember Belt** | 5000,1500 | 1000×900 | 25 | refinery, tank farms, pipe racks | 3 | Ember Works (A3); fire hazards |
| **Skyhook Airfield** | 5000,4700 | 1400×900 | 60 | runway, hangars, tower | 3 | Skyhook (A4); Last Bird candidate |
| **Highland Estates** | 1500,4600 | 1200×1000 | 120–200 | villas, terraces, pools, ridge | 3 | Halo Control Center (A1) on ridge (900,5350) |
| **Stadium & University** | 4500,3200 | 900×900 | 50 | arena bowl, lecture halls, parking decks | 2 | EP-04; Freewheel Yard at (4300,2200) |
| **Solar Fields** | 3300,5300 | 1500×800 | 70 | open flat arrays, turbines | 2 | long sightlines; EP-02, Last Bird candidate |
| **Kestrel Dam & Reservoir** | 2300,5400 | 700×500 | 150 | dam wall, spillway, control house | 3 | EP-01 |
| **Stilt Quarter** | 700,700 | 700×500 | 0 | houses on stilts, boardwalks | 2 | boat/skiff play; EP-08 |
| Perimeter Canal / Ring | edges | — | 0–10 | canal, causeways | 1 | Send-Off Ramps W/E |

## 3. Routes and traversal
| Route | Path | Use |
|---|---|---|
| **Ring Road** | ~500 m inside the edge, ~19 km loop | fast bike rotation, exposed |
| **Spine Highway** | N–S along x=3000 | central artery, contested |
| **Lakeshore Drive** | z≈900–1100, west→east | harbor to Ember |
| **Rail Corridor** | z≈2700–2900 | E–W tracks + service road, ramps |
| **Ridge Road** | (800,4200)→(2600,5700) | overlooks Old Market; winding |
| **Airfield Road** | (3000,4700)→(5000,4700) | straight, boost lane |
| **Refinery Road** | (3000,1500)→(5000,1500) | pipe cover |
**Stunt lines** (chains of ramps built for Hype): *Canal Run* (perimeter), *Harbor Crane Line*, *Rail Hop* (over tracks), *Rooftop Ramp Chain* (Old Market), *Fairground Loop* (Freewheel Yard).
**Line C tunnel** (Underline-lite): Central Station (3000,2500) → Stadium Station (4500,3100) → Ember Station (4900,1600), ≈3.2 km, dark, 12 side rooms, power-puzzle objectives for Rail Pods. Six surface stair entrances.
**Skyrail:** elevated loop Spire → Stadium → Rail Yard → Old Market (~6.5 km), pods every 90 s, 6 min per loop; pods can be shot; riders are exposed.

## 4. Design rules
- Every base is reachable by **at least 3 bike approaches**: one fast/exposed, one cover-rich, one vertical (ramp).
- No two bases within 1,400 m; no district without a ≥300 m open strip for bike acceleration.
- Long-range positions: Solar Fields, Dam wall, Highland ridge, Skyhook tower, stadium rim. Each has a counter (cover route, smoke line, or bike flank).
- CQB clusters: Old Market, Mercy, Ember pipes, Spire lower floors.
- 30% of enterable buildings are **loot buildings** (marked by faint warm window light), 70% shells.
- Readability: landmark silhouettes visible from 2 km: Spire, Halo array, Ember flare stacks, Skyhook tower, Dam wall, stadium, Harbor cranes.

## 5. Flow expectations
Median squad-to-squad first contact ~4–6 min; hot spots Spire/Halo/Skyhook/Ember (≥30% of landings). Dimming centres bias toward the middle so late fights converge on Spire/Rail Yard/Solar edge.

## 6. Acceptance (map-level)
All coordinates in JSON resolve to walkable/valid terrain · every base has 3 approaches per §4 (automatic path test) · bike route Harbor→Halo ≤ 6 min · flythrough within budgets per `04_MAPS/00`.
