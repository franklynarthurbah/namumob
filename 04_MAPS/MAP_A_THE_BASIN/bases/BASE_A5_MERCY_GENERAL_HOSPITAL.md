# BASE A5 — MERCY GENERAL HOSPITAL (risk tier 4, boss SURGEON)
> Position (1000,1700) · footprint 300×260 m · elevation 30 m · Mercy Quarter · Extraction EP07 Mercy Roof (1000,1800)

## 1. Role
The medical economy hub: best source of surgical kits, painkillers, scanners and armor repair materials. Long corridors and darkness make it the map's horror-tinged CQB set-piece.

## 2. Approaches
1. **East street** from Old Market (cover-rich). 2. **Lakeshore Drive** from the south (exposed). 3. **Parking-deck service ramp** (`R14` step-up at (900,1600), 13 m) onto the deck roof. Rooftop EP07 is accessible by stairs and the ambulance ramp.

## 3. Layout
| Element | Contents | Dimensions |
|---|---|---|
| **Main tower** | 6 floors: ER (L1, large open), OR suites (L2), ICU corridors (L3, 60 m lanes), wards (L4–L5), roof helipad | 90×40 m |
| Wings A–E | around a central courtyard; connected by skywalks at L2 | 60×25 m each |
| **Parking deck** | 3 levels, ramp loop, roof lot | 100×60 m |
| **Ambulance bay** | garage with vehicles (VZ_mercy) | 40×20 m |
| **Quarantine ward** | sealed, keycard, high loot | 40×20 m |
| **Morgue (B1)** | dark, cold-room lockers, boiler room | 70×30 m |
| Pharmacy | hero loot cage (L1) | 20×12 m |

## 4. Sightlines and chokes
ICU 60 m lanes with door cover; courtyard is a crossfire pit; skywalks are risky bridges; morgue is short-sightline dark rooms; parking deck ramps give bike ambush options.

## 5. Hazards and interactables
Lights flicker and can be shot out (dark rooms need flashlights/IR) · alarm doors close for 20 s when the pharmacy cage opens · morgue lockers can hide loot or a jump-scare-free ambush · SURGEON's syringe slow (−30% speed 3 s).

## 6. AI (≈12 + boss)
2 Scrap Crews (6) in wings, 1 Concession Guard squad (4) in tower L2–L3, **SURGEON** in the OR suite (heals AI allies up to 40%, syringe slow, retreats through OR doors).

## 7. Loot (≈50 containers + 4 hero)
Medical cabinets 22 (70% medical) · general 10 · PC/file 6 · tool 6 · weapon crate 2 · ammo 4. Hero: **medical scanner** (8k), surgical kit stack, T3 armor cache, quarantine keycard.

## 8. Blender kit
Hospital kit: corridors (3.2 m wide), doors, ward rooms, OR equipment, beds, lights (emissive strips), pharmacy cages, elevator cabs, morgue lockers. Wings share modules; ICU corridor uses one 12 m module repeated 5× for cheap draw calls.

## 9. Unity notes
Baked lighting with many emissive strips (no realtime lights except flashlights); flicker via emission animation only; interior occlusion per wing; reverb zones (corridor vs hall).

## 10. Balance/telemetry
Clear 6–9 min; expected medical value/raid 25–60k Scrip. Track: dark-room deaths, pharmacy alarm triggers, extraction via roof vs stairs, SURGEON heal uptime.
