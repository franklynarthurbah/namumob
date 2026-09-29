# Bodycam camera rig, render pass and OSD
> Read after: 03_MOVEMENT_COMBAT · Used in: Phase 1 (rig + pass), Phase 5 (OSD skins) · Budget: 1.2 ms on tier M at 1080p×0.75 render scale

## 1. Camera rig
`BodycamRig`: ChestAnchor (child of player root; height 1.35 m stand / 1.0 crouch / 0.35 prone; offset +0.08 m right, +0.06 m forward) → CameraRoot → Camera.
- Vertical FOV 68° (player slider 60–80) plus lens distortion for a ~100° feel.
- **Torso lag:** camera follows aim through a critically-damped spring (60–90 ms); the weapon leads (hands move first).
- **Bob:** breathing 0.6–1.2 cm; walk 1.5 cm; sprint 3–4 cm + 1.2° roll; recoil kick applies 35% to camera, rest to weapon.
- **Bike view:** handlebar/helmet-cam preset (pitched up 6°, extra shake, dirt overlay) with optional chase cam.

## 2. `BodycamLens` fullscreen pass (URP Fullscreen Pass Renderer Feature, Render Graph API)
ONE pass, last before UI: [VERIFY API names in URP 17.x docs]
| Stage | Default | Tier notes |
|---|---|---|
| Barrel distortion k1 0.18, k2 0.05 + edge chromatic offset 0.6–1.2 px | on | all |
| Vignette + corner softening | on | all |
| Exposure hunting: attack 0.25 s, decay 0.9 s, overshoot 8%, driven by **Exposure Zone volumes** (indoor/outdoor/night), no GPU readback | on | all |
| Sensor noise: blue-noise 128² tiled, stronger in darkness | on | half-res noise on L |
| Compression artifacts: 8×8 block posterize, triggered by fast rotation, damage, jammers | events | M/H |
| Rolling-shutter skew ∝ yaw rate | off | H only |
| Sharpen (small) | on | all |
| Directional motion streak while sprinting | off | H only |
Anti-aliasing: FXAA on L/M; SMAA on H. No MSAA on L. Never sample more than 3 textures per pixel beyond scene colour.
**Feed Authenticity** 0–100 scales every effect (Off / Light / Full). **Photosensitive-safe** removes tearing/flash/strobe.

## 3. Event effects
| Trigger | Effect |
|---|---|
| Dimming / EMP / Signal Jammer | line tearing, RGB split, frame holds (0.1–0.3 s), macroblocks |
| HP <25% | desaturate 30%, slow pulse vignette |
| Explosion nearby | white flash 0.15 s + compression burst + tinnitus (audio) |
| Rain/mud/blood near lens | lens-dirt overlay (2 layered alpha textures, animated) |
| Bleeding | subtle red edge pulse (never blocks aim) |
| Redaction | enemy/squad **faces mosaic** (10×10 cells) within 15 m via head-submesh shader |

## 4. OSD (UI Toolkit overlay, crisp, not baked)
Top-left `● REC 00:12:34` (blinks 1 Hz) · date/time stamp `2049-06-14 21:33:07` · unit id `KST-4471` · battery % (raid time remaining, cosmetic) · **SIGNAL** bars from real RTT/loss (4 bars ≤60 ms, 3 ≤110, 2 ≤180, 1 ≤300) · mic bars driven by loudness (gunfire/voice) · GPS tick when extraction is available. Font JetBrains Mono, white 90% with 1 px shadow. Skins purchasable (layout/colours only).
Crosshair presets: **Feed** none (dot only in ADS), **Assist** faint dot (default), **Classic** standard.

## 5. Performance rules
Half-res intermediates only; no extra full-screen copies; Native RenderPass on; the pass is a single fragment shader (Shader Graph fullscreen or HLSL). Fallback material variant for tier L that drops noise/compression.

## 6. Acceptance
Frame-time budget met on tier M · screenshot tests at Authenticity 0/50/100 · UI text remains legible at 100 · photosensitive mode verified · effects respond to network signal in Feed OSD.
