# Movement, combat, ballistics (deterministic simulation spec)
> Read after: 02_MATCH_FLOW · Used in: Phases 1–2 (`Namulinda.Sim`), Phase 6 (tuning) · Tunables live in ScriptableObject/JSON.

## 1. Kinematic controller (server-authoritative; client predicts)
| Item | Value |
|---|---|
| Capsule | 1.8 m / r 0.35 (crouch 1.2, prone 0.5) |
| Speeds (m/s) | walk 3.0 · jog 4.4 (default) · sprint 6.6 · crouch 2.0 · prone 1.0 · ADS ×0.55 · swim 2.2 |
| Accel / air control | 25 m/s² / 0.25 |
| Jump | v=4.2 m/s (≈0.9 m), 0.3 s cooldown, −8 stamina |
| Vault ≤1.3 m (0.6 s) · Mantle ≤2.0 m (1.0 s, −20 stamina) · Ladder 2.2 m/s |
| Slide | from sprint, 1.1 s, 7.5 m/s decaying |
| Lean | ±20° roll, ±0.32 m lateral |
| Stamina | 100; sprint −11/s; regen +16/s after 1.2 s; sprint locked <10 until 30 |
| Weight | soft limit 30 kg; −1.5% speed per kg above; no sprint >45 kg; immobile >60 kg |
| Fall | >3 m: 8 dmg/m beyond 3 m; >10 m lethal without chute |
| Parachute | free-fall 55 m/s down; canopy 6.5 m/s down, 16 m/s forward; auto-open at 80 m AGL |
Implementation: custom collide-and-slide with `Physics.SphereCast/CapsuleCast` in fixed steps; no `CharacterController`, no Rigidbody. `Physics.simulationMode = Script`. Same code compiled into client and server.

## 2. Input validation (server)
Yaw/pitch delta ≤40°/tick · move vector magnitude ≤1 · button rates sane · commands must arrive in sequence within a window. Violations are counted and reported to the anti-cheat pipeline (no instant kick).

## 3. Weapon pose solver (muzzle-true, shared client/server)
```
weaponDir ← critically-damped spring toward camera aim (k = 38, c = 12; heavier guns lower k)
          + recoil offset + stance/move sway (deterministic noise seeded by playerId, tick)
muzzlePos = chestAnchor + rot(weaponDir) * (weaponSocket + muzzleOffset(attachments))
shotDir   = weaponDir rotated by recoil-pattern sample + spread sample
```
Fixed 30 Hz steps; the client renders an interpolated version. Shots come from the muzzle, so hip/ADS offsets and corner peeks are honest. Assist preset adds an optional convergence toward screen centre (documented, accessible, server-known).

## 4. Firing model
- Fire commands carry a tick and shot count. Rates up to 15 shots/s are resolved sub-tick (ordered within the tick).
- **Hitscan** for everything except **DMR/sniper/launcher/grenades** (projectiles with drop: g=9.8, speeds: DMR 780, sniper 900, launcher 120 m/s).
- **Recoil:** deterministic per-weapon pattern (vertical climb + horizontal drift); recovery 60%/s after release; ADS −30%, crouch −15%, prone −30%; movement adds spread.
- **Spread:** PRNG seeded from (matchSeed, playerId, shotIndex) so client prediction of visuals matches the server.

## 5. Hit registration and lag compensation
1. Client includes the `renderTick` it was interpolating when firing.
2. Server rewinds enemy **hitbox states** to that time, clamped to the last 250 ms and validated against measured RTT ±60 ms.
3. Server builds shot pose (§3), raycasts vs rewound hitboxes and un-rewound static world (with material penetration for thin cover).
4. Applies damage; emits `HitConfirm`, `DamageEvent`, kill events.
Hitboxes are **stance-based sets** (standing/crouch/prone × lean) of 7 shapes: head sphere r 0.13 · chest capsule · abdomen box · 2 arm capsules · 2 leg capsules, positioned from replicated state (position, yaw, stance, lean); no animator on the server for players.

## 6. Damage model
```
dmg = base × rangeMult(d) × zoneMult × (1 − DR_eff) × pen_mult(materials) × G_DMG
zoneMult: head 2.0 · thorax 1.0 · abdomen 0.95 · upper arm 0.7 · forearm 0.6 · thigh 0.75 · calf 0.6
rangeMult: 100% to fall_start, linear to min% at 2.2×fall_start (per weapon, min 60–75%)
```
**Armor:** classes AC1–AC5, DR 25/35/45/55/65%, durability 40/60/80/100/120. **Ammo pen** (1–6) vs class: pen − class ≥ +1 → DR ×0.5; ≥ +2 → DR ×0.25; durability loss = raw damage × 0.5; armor at 0 durability gives no DR. **Helmets** HC1–HC4 reduce the head multiplier to 1.8/1.6/1.45/1.3.
**Health:** 100 HP. **Bleed:** limb hits have 25% chance (heavy bleed 50% from ≥.338/slug) → −1 HP/s until bandaged; leaves a blood trail. **Fracture:** falls/heavy limb hits → −25% speed until splinted.

## 7. Time-to-kill guidance
Reference (before misses): AR unarmored 3–4 shots (~0.2–0.3 s); AR vs AC3 5–7 shots (~0.4–0.5 s). Tuning target from playtests: effective **AR vs AR at 30 m, 200 ms RTT → 0.7–1.1 s**. Global knob `G_DMG` (default 1.0) and per-weapon tables are hot-reloadable from the backend config.

## 8. Aim assist (touch)
Friction only: −25% look sensitivity while the reticle is within 2° of a hitbox at ≤40 m. Off in Arena ranked. No magnetism, no auto-fire. Applied client-side before yaw/pitch is sent; server validates angular speed.

## 9. Acceptance tests
Client prediction error <5 cm p95 at 150 ms RTT + 3% loss · 1000-shot determinism test (client vs server) matches · lag-comp test: shooter aiming at a target's rendered position hits at 200 ms RTT ≥95% · all constants come from data, not literals.
