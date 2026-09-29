# Brand, logo and app-icon prompts
> Read after: 02_GAME_DESIGN/01 · Used in: Phase 0 (icon placeholder), Phase 5 (final brand), Phase 10 (store assets)
> Workflow: generate options with an image model → pick → have Claude rebuild the winner as clean SVG → export all sizes. Image generators are for exploration; the shipped logo is the vector rebuild.

## 1. Brand brief
**Name:** NAMULINDA · **Tagline:** *Loot. Record. Extract.* · **Personality:** resilient, sharp, tech-lit, warm (not grim). **Palette:** Night Slate `#0D1117`, Laterite `#C8461F`, Sun Gold `#F2B33D`, Bone `#ECE7DB`, Feed Cyan `#35E0D2` (accent), REC Red `#FF2E3A` (dot only). **Type:** custom wordmark based on heavy extended geometric sans (Chakra Petch-like), UI in Chakra Petch/Barlow.
**Mark idea — "Lens-N":** an **N** built from two tunnel-arch pillars and one sharp diagonal **ramp** stroke (rising left→right), inside a six-blade **camera-aperture ring**, a small **REC dot** at the upper right, **viewfinder corner brackets** around the mark. Must read at 48 px.
**Wordmark idea:** NAMULINDA in wide heavy caps; crossbars of both **A**s replaced by a thin scanline gap; the **I** carries a red REC dot as its tittle; the **U** is a tunnel arch; small gold `[ ]` brackets at the ends.
**Do:** flat vector, chamfered corners, high contrast, ≤4 colours per lockup. **Don't:** gradients in the master, skulls, crosshair clichés, weapons, national flags, anything resembling the marks of PUBG, Bodycam or Metro titles.

## 2. Variants to deliver
Primary lockup (emblem above wordmark) · horizontal lockup · wordmark only · emblem only · monochrome (bone on dark, dark on bone) · micro-mark (aperture ring + N + dot, no brackets) for ≤32 px · animated splash.

## 3. Image-model prompts (adjust flags to your generator's current version)
**P1 — Emblem exploration** (Midjourney / Flux / GPT-image / Firefly)
```
Flat vector emblem logo for a mobile tactical extraction shooter called NAMULINDA. A bold geometric letter N built from two tunnel-arch pillars and one sharp diagonal ramp stroke rising left to right, enclosed in a hexagonal camera-aperture ring with six blades, small solid red recording dot at upper right, viewfinder corner brackets framing the mark. Palette: laterite red #C8461F, sun gold #F2B33D, bone white #ECE7DB on night slate #0D1117, tiny cyan #35E0D2 signal accent. Symmetrical clean geometry, thick strokes readable at 48 px, chamfered corners, no gradients, no text, centered on plain dark background, professional game studio branding, vector look, high contrast.
Negative: text, letters other than N, watermark, photo, 3D render, gradients, clutter, cartoon, skull, weapon, crosshair, flag.
Suggested flags: --ar 1:1 --style raw --stylize 150 (Midjourney) · generate 16 variations
```
**P2 — Wordmark** (use a text-accurate model such as Ideogram or GPT-image; verify spelling N-A-M-U-L-I-N-D-A)
```
Logo wordmark "NAMULINDA" in heavy extended geometric sans-serif capitals, angular cuts inspired by camera viewfinder brackets. Both letter A have the crossbar replaced by a thin horizontal scanline gap. The letter I has a small solid red recording dot as its dot. The letter U is shaped like a tunnel arch. Wide letter spacing. Bone white #ECE7DB letters on night slate #0D1117, tiny gold #F2B33D corner brackets at both ends. Flat vector, crisp edges, highly legible at small sizes. No extra words, no tagline.
Negative: distorted letters, misspelling, gradients, drop shadows, texture, 3D.
```
**P3 — Full lockup** (after choosing an emblem, attach it as image reference)
```
Stacked logo lockup: the attached emblem centered above the wordmark NAMULINDA, small tagline "LOOT. RECORD. EXTRACT." in monospace below. Flat vector, same palette, generous clear space equal to the height of the N, dark background variant and light (bone) background variant.
```
**P4 — App icon (1:1)**
```
Mobile game app icon, full-bleed square (no rounded corners), centered emblem: bold letter N made of tunnel-arch pillars and a diagonal ramp stroke inside a six-blade aperture ring, red recording dot at upper right. Background: deep night slate with subtle laterite red radial glow from the lower left, faint camera vignette. Sun gold rim light on the ring, high contrast, strong silhouette, 18% safe margin, flat vector shading, no text.
Negative: text, border, rounded corners, photo, clutter, skull, weapon.
```
**P5 — Key art (loading/store hero)**
```
Cinematic key art for a mobile extraction shooter: a rider on a stunt bike mid-air over a canal toward a dark overgrown city skyline at dusk, seen through a wide-angle bodycam lens with subtle fisheye distortion, REC timestamp overlay in the corner, cyan Halo LEDs on distant towers, warm laterite-red light on the ground, dust and sparks, dramatic but not gory. Original characters and vehicles, no real brands.
Negative: text logos, real brands, weapons pointed at camera, gore.
```
**P6 — Store feature graphic 1024×500:** reuse P5 composition wide, left-third clear for the lockup.
**Iteration loop:** pick 3 winners → "same emblem, thicker strokes / fewer blades / simpler bracket" → check at 48/32/16 px → choose one → hand to Claude (§4).

## 4. Claude prompt — rebuild the winner as SVG and export assets
```
You are a brand production designer. Input: docs/brand/chosen_emblem.png and the brand brief in docs/prompt-pack/06_UI_UX_BRANDING/01_BRAND_LOGO_APP_ICON_PROMPTS.md.
1) Rebuild the emblem, wordmark and lockups as clean SVG on a 512×512 grid using only paths/polygons/circles (no raster, no filters). Ring outer r=232, inner r=200, six blades; keep strokes ≥24 units. Provide: emblem.svg, wordmark.svg, lockup_stacked.svg, lockup_horizontal.svg, mono_light.svg, mono_dark.svg, micro.svg.
2) Verify legibility at 16/32/48/64 px by rendering PNGs (resvg/cairosvg/Inkscape CLI) into docs/brand/preview/ and fix any clogged detail.
3) Export app assets (script Tools/Brand/export_icons.py): iOS 1024×1024 PNG without alpha and full-bleed; Android adaptive icon layers 432×432 px (foreground safe circle 264 px, background layer, monochrome layer for themed icons); Play Store 512×512 PNG; legacy Android 48/72/96/144/192; notification icon (white on transparent, 96 px); feature graphic 1024×500.
4) Provide a Unity import note and place assets in Client/Assets/Branding/ and store-listing assets in /Brand/store/.
Do not embed any font; convert text to outlines. Log the prompts and tools used in docs/brand/PROVENANCE.md.
```

## 5. Technical specs
| Asset | Spec [VERIFY at submission] |
|---|---|
| iOS icon | 1024×1024 PNG, no alpha, no rounded corners; also prepare layered assets (foreground/mid/background + dark/tinted variants) for Apple's layered icon workflow |
| Android adaptive | foreground & background 108 dp (432 px @xxxhdpi); keep art inside the 66 dp (264 px) circle; monochrome layer for themed icons |
| Play listing icon | 512×512 PNG, 32-bit |
| Feature graphic | 1024×500 |
| Notification icon | flat white silhouette on transparent |
| Splash | Android 12+ splash icon on a solid brand background; Unity splash configured to brand |

## 6. Animated logo (splash, ≈3.0 s, real-time in UI Toolkit)
0.0–0.6 s black + blinking REC dot · 0.6–1.4 s aperture iris opens revealing the N with a 6-frame chroma split · 1.4–2.2 s wordmark scanline reveal · 2.2–3.0 s `NAMULINDA // SIGNAL OK` timestamp line, hold, fade. Skippable after 1 s, first launch only plays full.

## 7. Legal
Run trademark searches (USPTO, EUIPO, WIPO and the launch countries) for "Namulinda" and the mark before launch. Keep prompt/tool logs. Prefer the vector rebuild (human-reviewed) as the registered mark.
