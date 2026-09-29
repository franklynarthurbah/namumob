# BASE A2 — VAULT TOWER (risk tier 5, boss BANKER)
> Position (3150,3950) · footprint 120×120 m · elevation 45 m · Spire District · loot tier up to 5

## 1. Role
A 38-storey bank tower in the CBD: vertical PvPvE with the best guaranteed cash loot and constant third-party pressure from the plaza, sky bridge and rooftops.

## 2. Approaches
1. **Plaza** from Spine Highway (fast, exposed). 2. **Sky bridge** from Skybridge Mall (roofed bridge at floor 22, 90 m long): vertical entry. 3. **Glass-lobby ramp** (`R16` Big Air from canal side, to be added at (3050,3850) heading 45°): bike straight into the lobby. Exit option: **BASE-jump rigs** on the roof (4 pickups) glide to the plaza/canal.

## 3. Playable floors (floor height 4.2 m)
| Level | Contents | Key dimensions |
|---|---|---|
| B2 **Vault** | vault door, deposit-box wall, BANKER arena | 50×36 m |
| B1 | parking, service ramp, loading dock | 100×100 m |
| L0 | lobby atrium (12 m), security, glass elevators ×2 | 60×60 m |
| L1 | mezzanine ring | 4 m wide walkway |
| L2–L4 | bank halls, teller lines, offices | 60×40 m each |
| L12–L13 | executive offices, boardrooms | 40×40 m |
| L22 | sky lobby + bridge | bridge 90×6 m |
| L36 | executive suite | 30×30 m |
| Roof | helipad, AC units, BASE rigs | 40×40 m |
Cores: 2 stairwells (A/B), 2 glass elevators (exterior; shootable cables cause drops), service shaft (vent crawl L12↔B1).

## 4. Sightlines and chokes
Lobby atrium (crossfires from L1/L2), bank halls with counters as cover lanes, L22 bridge (long straight lane 90 m: deadly, smoke is essential), stairwell landings. Roof snipers see the whole plaza; countered by the vent shaft or by baiting with the elevator.

## 5. Hazards and interactables
Elevator ride (loud, exposed) · **vault door**: keycard from BANKER or 60 s drill (alarm: reinforcements arrive at plaza in 30 s) · breakable glass panels (visual, bullets pass with reduced damage) · power breaker floor 12 toggles elevators.

## 6. AI (≈14 + boss)
3 Concession Guard squads (12) stacked on L0–L4/L12; 2 roof snipers; **BANKER** (riot shield front, opens vault on death). Elite squad on L36 wakes at 04:00/12:00.

## 7. Loot (≈55 containers + 4 hero)
Safe-deposit boxes 20 (valuables), teller drawers (gold bars, Scrip-rich items), exec PCs/files 14, weapon crate 3, general 12, medical 4. Hero: **Vault ledger**, gold bar stack (3), T4 sniper crate, executive keycard.

## 8. Blender kit
Highrise kit + bank interior: teller counters, deposit-box wall, vault door (hero 4k tris), atrium chandelier, elevator cabs, glass shader. Exterior facade ≤6k tris LOD0 with LOD1 2k; interiors streamed per floor block (L0–L4, L12–13, L22, L36, B1–B2).

## 9. Unity notes
Floor-block sub-scenes with occlusion portals; window-view impostors; glass = simple transparent shader (no refraction); elevators as kinematic platforms on the server; BASE rig glide uses the parachute controller with wide flare.

## 10. Balance/telemetry
Clear 8–12 min; heavy third-party rate expected (>40% of raids that enter get contested). Track: entry route (plaza/bridge/lobby ramp), elevator deaths, vault opens, BASE-jump usage, bridge deaths (if >25% of L22 crossings die, add cover).
