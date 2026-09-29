# In-raid HUD specification (landscape, reference 1920×1080)
> Read after: 02_UI_DESIGN_SYSTEM, 02_GAME_DESIGN/04 · Used in: Phase 1 (skeleton), Phase 2 (network-driven data), Phase 6 (polish)

## 1. Layout (positions are defaults; the layout editor can move/resize/opacity every control)
```
┌────────────────────────────────────────────────────────────────────────────┐
│ ●REC 00:12:34  KST-4471            [ compass ribbon ]         SQUAD (3 rows)│
│ 2049-06-14 21:33:07  BAT 82%   ▮▮▮▯ SIGNAL   Dimming P2 04:12   kill feed  │
│ [minimap 200²]                                                              │
│ (left stick zone)          · reticle (preset) ·          interact prompt    │
│ lean  lean                                             [ADS] [FIRE]         │
│ [stick]     HP/armor strip                ammo   [Reload][Swap]   [Jump]    │
└────────────────────────────────────────────────────────────────────────────┘
```
| Element | Default rect (x,y,w,h) | Notes |
|---|---|---|
| OSD block | 48,32,420,96 | REC, timer, unit id, date/time, battery, signal bars, mic bars (see `02_GAME_DESIGN/04_BODYCAM_CAMERA_AND_RENDER_SPEC.md`) |
| Compass ribbon | 600,32,720,44 | degrees + N/E/S/W; markers: squad, pings, extraction, danger, Signal Cache |
| Dimming/phase timer | under compass 760,84,400,32 | phase, time to close, arrow to safe centre |
| Minimap | 48,160,200,200 | circular, rotating (setting), fog of unexplored; tap = full map |
| Squad panel | 1552,32,320,3×64 | name, HP bar, bleed/downed icons, distance |
| Kill feed | 1552,232,320,140 | max 4 lines, 5 s |
| Reticle | centre | preset dot/none; hit markers 120 ms |
| Interaction prompt | 960,680,300,90 | hold-ring (0.5–1.0 s configurable) |
| HP/armor strip | 860,990,200,24 | HP bar + armor class pips + bleed/fracture icons |
| Ammo | 1660,940,180,64 | mag/reserve, fire mode, weapon icon; diegetic LED also on weapon |
| **FIRE** | 1700,700, Ø160 | primary; optional second FIRE at left (220,560) |
| ADS | 1520,620, Ø110 | hold or toggle (setting) |
| Jump / Crouch-Prone | 1780,880 Ø110 / 1620,900 Ø96 | tap crouch, double-tap prone; stamina arc around Jump |
| Lean L/R | 300,420 / 420,420, Ø88 | hold |
| Reload / Swap / Throwable / Heal | 1450,780 / 1360,880 / 1250,880 / 1150,880, Ø88 | context greyed when unavailable |
| Sprint lock | 300,340 | pushing stick >80% auto-sprints; icon shows lock |
| Backpack | 1240,120 | opens loot/inventory panel (list-based, drag optional) |
| Ping/comm | 1120,120 | tap = ping, hold = ping wheel (6: enemy, loot, go, wait, extract, danger) |
| Mic/voice | 1000,120 | Phase 6 |

## 2. Mode variants
- **Bike:** speed digits (mono 48 px), Boost bar, Hype counter, trick zone on right thumb, mount/dismount, tilt option. Weapon UI hides unless pillion/rider pistol.
- **Parachute:** altitude, speed, deploy marker, landing suggestion arrow.
- **Extraction:** EP markers on compass, hold ring, chopper ETA; siren edge pulse.
- **Underline:** battery/IR toggle button, darkness assist (subtle edge brightening), train horn indicator.
- **Downed:** crawl stick, pistol only, bleed-out ring, squad request-revive ping.

## 3. Feedback
Directional damage arcs (0.6 s), hit markers, headshot ping, armor-break flash, low-HP pulse, bleed drip at edges, kill confirm text, loot pickup toasts (rarity colour + icon + shape), extraction success sting.

## 4. Touch and layout editor
Multi-touch with per-pointer capture; sticks use dynamic origin (left half) and look zone (right half) with dead zones; sensitivity curves per stance/ADS; optional gyro. Layout presets: **2-thumb**, **3-finger claw**, **4-finger claw**. Editor: drag/resize/opacity per control, snap grid, overlap validation, saved to JSON (cloud-synced). Cannot place controls outside safe area or hide FIRE/Jump.

## 5. Performance and implementation
Retained UI Toolkit tree, no per-frame allocations, update labels only on value change, pooled elements for feed/toasts, USS class toggles instead of style writes, `usageHints` on animated elements. HUD budget **≤0.6 ms/frame CPU on tier M**. HUD reads from a `HudViewModel` fed by sim events (never reads gameplay objects directly).

## 6. Accessibility
Colour-blind palettes + shape icons on rarities, HUD scale 80–130%, high-contrast mode, adjustable hold times, one-handed presets, haptics toggle, subtitles size.

## 7. Acceptance
All elements reachable on 6" and 10" screens; no overlap in default presets on 16:9, 19.5:9, 21:9; HUD CPU within budget; hit marker latency <1 frame after server confirm; layout JSON round-trips.
