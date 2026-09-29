# Animation, VFX and audio specification
> Read after: 01_ART_DIRECTION · Used in: Phase 1 (rig/IK), Phase 2 (anim state sync), Phase 6 (full sets)

## 1. Animation
**Layers:** base locomotion (8-way × stance) · upper-body weapon layer · additive lean/aim/recoil · hand/foot IK (Animation Rigging). Sim drives everything (root motion off); the server replicates compact state (stance, move dir/speed bucket, lean, weapon id, action id) and clients derive animation.
**Counts (targets):** TP locomotion 24 + transitions 20 + actions 30 + downed/death 12 + revive 6 + vault/mantle/ladder 12 + swim 4 + parachute 6 + emotes 12 + vehicles ≈40 (mount/dismount L/R, ride, lean, 8 tricks, landing, crash, pillion). FP per weapon class (8 classes × ≈24): idle, walk, sprint, ADS in/out, fire (additive), reload ×3 (calm/stressed/injured) + empty variant, inspect, draw/holster, melee, throw.
Reload clip speed is scaled to `reload_s` from data. Weapons animate mechanical parts (mag, bolt) by bone. Animator LOD: remote characters update every 2nd frame beyond 40 m, every 3rd beyond 80 m; cull when invisible; optimal keyframe reduction.

## 2. VFX (built-in Particle System; **no VFX Graph** on mobile)
| Effect | Notes |
|---|---|
| Muzzle flash | 3 variants per calibre class, pooled, one draw call; suppressed = tiny |
| Tracers | pooled mesh trails, every 3rd round visually |
| Impacts | per material (concrete, metal, dirt, wood, water, glass, flesh-mild): dust + decal; ≤60 decals live, oldest recycled |
| Shells | pooled, 3 s life |
| Smoke grenade, airdrop/flare smoke | soft sheets, overdraw-capped |
| Explosions, fire, gas ignition | flipbooks + light flash |
| Dimming | fog/noise sheets + screen effects from `02_GAME_DESIGN/04_BODYCAM_CAMERA_AND_RENDER_SPEC.md` |
| Halo EMP | expanding ring + bodycam glitch trigger |
| Bike dust/mud/sparks, water splash, rain + screen drops, lens dirt | tied to surface tags |
| Blood | mild, decals + small particles, toggleable |
**Budgets (particles alive):** L 400 · M 800 · H 1500. No alpha-blended full-screen particles. Every effect has a tier L variant.

## 3. Audio
Unity audio + Audio Mixer (categories: Weapons, Footsteps, Vehicles, Ambience, UI, Voice, Music, with ducking). FMOD optional only if licence budget is approved [VERIFY].
**Bodycam mic treatment** (mixer snapshot, scaled by Feed Authenticity): high-pass 120 Hz, low-pass 9 kHz, soft compressor 4:1, clipping on gunshot peaks, wind noise on fast movement, breathing loops tied to stamina, hiss in the Dimming, compressed radio chatter.
**Distances (m):** footsteps walk 12 / sprint 25 · unsuppressed shot 150 · suppressed 45 · bike 80 (electric 15) · explosion 250 · flare siren 300 · helicopter 600 · flare stack roar 300. Server sends sound *events* beyond visual range so distant fights are audible without leaking positions precisely (direction + rough distance bucket).
**Spatial:** stereo panning + 3-level raycast occlusion (low-pass 22 kHz → 2 kHz) + reverb zones (small room, hall, tunnel, canyon, outdoor). Optional **visual footstep cue** (edge indicator) for speakers/accessibility.
**Content plan:** traders (4×≈60 lines), announcer, AI barks (≈60 per faction), squad callouts (localized), foley per surface. Use placeholders (clearly marked) until recorded; licence log required.
**Music:** adaptive layers (Safehouse, low tension, high tension, extraction success, loss). Original composition (hybrid percussion + electronics); temp tracks only if licensed CC0/purchased.
**Budgets:** audio resident ≤60 MB on tier L (Vorbis for long, ADPCM for short); max simultaneous voices L 24 / M 32 / H 48; stream music/ambience; cull far 3D voices by priority.

## 4. Accessibility
Subtitles for all voice; per-category volume; mono downmix; hearing-impaired mode (visual cues for footsteps, reload, extraction siren); photosensitive-safe visuals.

## 5. Acceptance
No animator on server players · 20 animated remote characters within CPU budget on tier M (≤3 ms animation) · muzzle-to-audio latency <60 ms · mix snapshots switch without pops · every SFX/VO has a licence entry.
