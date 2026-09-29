# Vehicle roster and the stunt-bike system
> Read after: 02_GAME_DESIGN/03 · Used in: Phase 3 (Dustrunner v1), Phase 6 (full roster), map authoring (ramps)

## 1. Roster
| ID | Type | Seats | Top km/h | 0–60 s | HP | Special | Noise m | Value |
|---|---|---|---|---|---|---|---|---|
| veh_bike_dustrunner | Trail/stunt bike (default) | 2 | 105 | 3.2 | 200 | balanced, Boost 2.5 s | 80 | 9,000 |
| veh_bike_kicker | Stunt frame | 1 | 90 | 2.8 | 150 | +40% air rotation, +25% trick speed, 4 trick slots | 70 | 12,000 |
| veh_bike_bruiser | Heavy enduro | 2 | 115 | 3.8 | 300 | ram ×1.5, front fairing DR 20% | 90 | 20,000 |
| veh_bike_voltx | Electric | 2 | 100 | 2.6 | 180 | silent (15 m), Boost recharges 5%/s while coasting | 15 | 28,000 |
| veh_quad_scorpion | Quad | 2 | 80 | 3.5 | 260 | best climbing/rough terrain | 70 | 10,000 |
| veh_buggy_gazelle | Buggy | 2 | 110 | 4.0 | 350 | roll cage | 90 | 16,000 |
| veh_jeep_rhino | Armored jeep | 4 | 95 | 5.5 | 800 | armored glass, ram ×2 | 100 | 45,000 |
| veh_skiff_skimmer | Jet skiff | 2 | 70 (water) | 3.0 | 220 | water only | 85 | 14,000 |
| skyrail pod | Scripted rail | 4 | 60 | — | — | Map A: 4 stations on a loop | — | — |
World vehicles spawn at garages/roadside for free (any player may use). **Bike Crate token** brings a personal bike from the Garage (counts in Gear Value and can be lost). No fuel; HP only; at 0 HP the vehicle explodes (60 dmg in 4 m after 3 s). Repair kit (Wrench) restores 100 HP over 6 s.

## 2. Combat from vehicles
Rider: pistols only at −40% accuracy, or no weapon (melee off). Pillion: pistols/SMGs at −20%. Passengers in buggy/jeep: any weapon except sniper/LMG/launcher. Ramming: dmg = max(0, v_rel − 8 m/s) × 3.5 (×class multiplier), riders take half the impact when hitting static objects.

## 3. Stunt system
- **Preload:** hold Jump up to 0.6 s to compress suspension → up to +40% takeoff impulse.
- **Trick window:** after 0.35 s airtime and >1.2 m above ground.
- **Tricks** (equip 2 per bike, Kicker 4):
| Trick | Time s | Hype |
|---|---|---|
| Seat Grab | 0.8 | 40 |
| Can-Can | 0.9 | 55 |
| Table Top | 1.0 | 75 |
| Nac-Nac | 1.1 | 85 |
| Superman | 1.2 | 90 |
| Cliffhanger (Kicker) | 1.3 | 110 |
| 360 Barspin | 1.4 | 130 |
| Backflip (needs 360° pitch) | 1.6 | 150 |
- **Input (touch):** tap trick buttons in the right-thumb zone, or swipe up/left/right/down inside it. Airborne pitch/roll on the left stick.
- **Landing quality** (angle error to ground normal): ≤10° **Perfect** ×1.5 · ≤25° **Clean** ×1.0 · ≤45° **Sloppy** ×0.5 + 10 dmg · >45° **Crash** (rider 15–40 dmg, bike −30…−80 HP, tricks lost).
- **Hype** = Σ trick Hype × landing multiplier × chain multiplier (chain ×1→×3 within 2.5 s between landings; each new distinct trick +0.25).
- **Boost energy** (max 100): Perfect +25, Clean +15, Sloppy +5. Boost: +25% top speed/accel for up to 2.5 s (usable at ≥30).
- **Feed Rep** from Hype: Hype/50, cap 400 per raid; cosmetics only. Diminishing returns: 3rd identical jump at the same ramp within 60 s pays ×0.5, then ×0.25.
- Hype sets the **Style bonus** at Send-Off Ramps: +5% (Sloppy) … +15% (Perfect chain ≥ ×2) of carried loot value.
- Noise: engine, takeoff and landing emit sound events that alert AI.

## 4. Ramp catalogue (for map authors; `g_air` = 12 m/s²)
| Type | Dimensions | Typical use |
|---|---|---|
| Kicker | 8 m long, 1.6 m high, 18° lip | 8–12 m gaps at ~60 km/h |
| Step-up | 10 m, 2 m high | 12–18 m gaps |
| Big Air | 16 m, 4 m high, 24° | 25–40 m gaps at ~90 km/h |
| Quarter-pipe | r 6 m, 3 m high | vertical trick launch |
| Whoops | 8 bumps 0.6 m high every 3 m | rhythm section, boost farming |
| **Send-Off Ramp** | 24 m long, 6 m high, 26° lip, canal gap 34 m | extraction stunt, min 88 km/h at lip |
Range ≈ v² sin(2θ)/g_air. Every ramp has yellow chevron take-off markers and a landing slope ≤1:8 (≥12 m long). Gap tables are validated by an automated jump test in Unity (bike at speed X must land within ±2 m).

## 5. Controls (touch)
Left stick steer/lean · right cluster: Throttle (hold; auto-throttle option), Brake, Jump/Preload, Boost, Trick zone · top: mount/dismount, seat swap · optional tilt steering. Camera: handlebar cam (default) or chase.

## 6. Anti-abuse
Diminishing Hype at repeated ramps · Feed Rep/Hype disabled in Rookie lobbies' first 3 minutes · speed/air-time envelope checks (see `10_SECURITY_ANTICHEAT/03`).

## 7. Acceptance
Dustrunner reaches 105 km/h in the specified time (±5%) · Big Air jump test lands within ±2 m · Hype/Boost server-computed only · 12 visible vehicles at 60 fps on tier M.
