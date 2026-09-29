# AI enemies, garrisons, bosses
> Read after: 03_MOVEMENT_COMBAT · Used in: Phase 3 (slice AI), Phase 6/7 (all tiers, bosses)

## 1. Architecture (server only)
Same replication as players (position, yaw, stance, weapon) so clients reuse player rendering. **Utility AI** picks goals (Patrol, Investigate, Engage, Flank, Cover, Retreat, Heal, Call backup) scored at **10 Hz**; movement/steering at 30 Hz. Navigation: Unity AI Navigation NavMesh baked per 500 m cell and streamed with the cell. Perception is deterministic and budgeted (rays per tick cap).

## 2. Perception
Vision cone 110°, range 60/90/120 m (T1/T2/T3), reduced 40% in darkness/Dimming/smoke, flashlights and lasers are detected +50%. Hearing radii: walk 12 m · sprint 25 m · unsuppressed shot 150 m · suppressed 45 m · bike 80 m (electric 15 m) · explosion 250 m. States: Idle → Suspicious (3 s) → Alert → Combat → Search (20 s).

## 3. Tiers
| Tier | HP | Gear | Accuracy | Reaction | Behaviour |
|---|---|---|---|---|---|
| **Scrap Crew** T1 | 80 | pipe pistols, old shotguns, bolt rifles; no armor | 35% | 0.7 s | groups 2–4, retreat when half down |
| **Concession Guard** T2 | 100 | AR/SMG, AC2, HC1 | 55% | 0.5 s | squads of 4, flank, grenades, callouts |
| **Halo Remnant** T3 | drone 60 (fly) · sentry 300 (fixed) · warden 250 | integrated weapons | 60% | 0.3 s | drones scout and beep; sentries cover zones; EMP glitches bodycams |
| **Elite** | 180 | AC4, HC2 | 65% | 0.3 s | guaranteed Epic drop |
| **Boss** | 600–900 effective | AC5/HC3 + mechanic | 70% | 0.25 s | see §5; legendary table |
Bracket scaling: Rookie −15% accuracy, +200 ms reaction; High Stakes +10% accuracy.

## 4. Garrisons and budgets
Bases: 8–16 AI in 2–4 groups with patrol graphs and an **alarm**: gunfire or sighting alerts the whole base within 20 s. POIs: 2–6 AI. Roads/outskirts: roaming crews. Total population Map A ≈150 AI, **active at once ≤60**; AI further than 250 m from any player and unseen are frozen on a schedule and despawn/respawn by cell. Server budget target: AI ≤4 ms per tick on Map A.
Drops: T1 Common/Uncommon · T2 Uncommon/Rare · T3 Rare/Epic + Halo cores (rare) · Elite Epic · Boss Legendary + keycard.

## 5. Bosses (Map A)
| Boss | Base | Mechanic |
|---|---|---|
| **ARCHITECT** | A1 Halo | drone swarm (6 drones), EMP pulse every 30 s (bodycam glitch + gadget disable) |
| **BANKER** | A2 Vault | riot shield front; opens vault door on death (keycard) |
| **FOREMAN** | A3 Ember | fire hazards, explosive tanks; LMG suppression |
| **SKYMARSHAL** | A4 Skyhook | long-range bolt rifle + spotter drones; moves between tower and hangar |
| **SURGEON** | A5 Mercy | heals AI allies, syringe slow (−30% speed 3 s) |
| **HARBORMASTER** | A6 Harbor | crane sniper nest + dockside ambush trucks |
Bosses spawn awake at match start in **Standard/High Stakes**; in **Rookie** they start at 06:00. They persist until killed; drops mark the base on the minimap for 60 s.

## 6. Testing
Deterministic AI tests with recorded seeds · perception rays under budget · 48 bots + 60 active AI at 30 Hz within 12 ms average server tick · telemetry: kills by tier, time-to-clear per base, deaths to AI vs players.
