# Vehicle physics, netcode, Blender build guide
> Read after: 01_VEHICLE_ROSTER · Used in: Phase 2 (prediction stub), Phase 3 (bike v1), Phase 6

## 1. Physics model (kinematic arcade; NOT PhysX rigidbody)
State: position, velocity, yaw/pitch/roll, angular rates, wheel contacts, suspension, boost, throttle, steer, lean.
- **Integration:** 4 sub-steps per 30 Hz tick (120 Hz) on client and server, identical code in `Namulinda.Sim`.
- **Contact:** 2 sphere-casts (front/rear wheel), suspension travel 0.35 m, averaged ground normal.
- **Longitudinal:** engine curve to top speed, drag ∝ v², rolling resistance, braking 14 m/s².
- **Lateral:** lean ∝ steer × speed (max 45°); turn radius from wheelbase/steer; grip μ by surface (tarmac 1.0, dirt 0.7, wet 0.6, sand 0.5), drift when lateral accel > μg.
- **Air:** ballistic with g_air 12 m/s²; pitch/roll rate 140°/s (Kicker 190°/s); optional self-levelling assist.
- **Collision:** swept capsule along velocity; impact damage = max(0, v_n − 6) × 4; rider ejection >18 m/s.
- **Landing:** resolve contact, compute quality from angle error, apply damage/energy.
- Sleep vehicles idle >5 s; max 24 active vehicles per match.

## 2. Netcode
- Driver predicts own vehicle exactly like on-foot; reconciliation threshold 0.15 m, error smoothed over 150 ms.
- Passengers are kinematically attached (no prediction of their own movement).
- Replicated vehicle state ≈24 bytes: pos (3×20 bit), velocity (quantized), orientation smallest-three (32 bit), flags (contacts, boost, trick id, landing quality).
- Remote vehicles: interpolation with dead-reckoning extrapolation ≤150 ms.
- Hit detection: chassis box + seated rider capsule set; participates in the same 250 ms rewind buffer.
- Boost/Hype/damage are server-computed; clients only send steering/throttle/trick inputs.

## 3. Anti-cheat envelopes
Speed vs class max (+8% tolerance) · acceleration limit · airborne without ramp/jump >5 s → flag · teleport >2.5 m/tick → correction + flag · trick input rate cap.

## 4. Blender build guide (bikes)
**Proportions:** wheelbase 1.35 m, wheel Ø 0.68 m, seat height 0.87 m, bar width 0.82 m.
**Budgets:** LOD0 14–18k tris · LOD1 7k · LOD2 2.5k · LOD3 0.9k · wheels separate meshes · 1 atlas material 2048² (ASTC) + emissive mask · livery via RGB region mask + decal layer for skins.
**Hierarchy/names (FBX):** `root` → `Body`, `Fork`, `Bars`, `WheelF`, `WheelR`, `Swingarm`, `Exhaust`; sockets (empties): `sock_rider`, `sock_pillion`, `sock_hand_L/R`, `sock_foot_L/R`, `sock_headlight`, `sock_tail`, `sock_trail`; collision proxy empties `col_wheelF/R`, `col_chassis`.
**Rider animations:** mount L/R, ride idle, lean blend tree L/R, throttle tuck, brake, air poses + 8 trick clips, landing absorb, pillion ride, crash eject, pillion shoot.
**Generator script:** `Tools/Blender/gen_bike.py --style dustrunner|kicker|bruiser|voltx` creates proportionate blockouts with sockets, UVs, and vertex-colour region masks; artists/marketplace replace meshes without changing sockets.
**Export:** 1 unit = 1 m, forward −Z / up Y in FBX exporter settings [VERIFY per Blender version], apply transforms, tangents on. A Unity import test checks bounds, socket names and wheel radius.

## 5. Acceptance
Reconciliation error p95 <10 cm at 120 ms RTT · 4 sub-step results identical client/server (hash test over 600 ticks) · Bike imports with all sockets validated by an editor test · LOD switch distances 20/45/90 m.
