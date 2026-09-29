# Art direction bible
> Read after: 02_GAME_DESIGN/01 · Used in: Phase 1 (materials/palette), Phase 3+ (all assets) · Companion: 06_UI_UX_BRANDING/02

## 1. Style: "grounded stylized realism"
Realistic proportions and lighting logic, simplified surface detail (trim sheets, painterly normals, clean baked AO). Chosen for phones: readable shapes, low texture cost, strong silhouettes. The wide-angle bodycam lens exaggerates near objects: design hands/weapons and interiors to look good at ~100° effective FOV.

## 2. Colour language
| Role | Colour |
|---|---|
| Brand/world | Laterite `#C8461F`, Sun Gold `#F2B33D`, Bone `#ECE7DB`, Night Slate `#0D1117`, Verdant `#3DBB6D` |
| Tech/danger | **Feed Cyan** `#35E0D2` (Halo LEDs, HUD), **REC Red** `#FF2E3A` (alerts, recording) |
Map moods: **A** dusk tungsten + laterite + cyan LEDs · **B** red dust, hard noon, orange haze · **C** teal-grey fog, rust, cold white · **D** emergency red, sparse cyan LEDs, black.
Rarity glow (loot, world-space outlines): Common `#9AA0A6` · Uncommon `#3DBB6D` · Rare `#3B82F6` · Epic `#A855F7` · Legendary `#F2B33D`.

## 3. Readability rules
- Enemy/AI silhouettes must read against every biome: rim value contrast ≥ 30%; faction accents: Scrap (hand-painted marks), Guards (orange armbands), Halo (blue LEDs).
- Squadmates: coloured armband + nameplate only when visible; enemies never get nameplates.
- Interactables: subtle warm light + tiny rotating pip at ≤6 m; loot buildings show faint warm window light.
- Night/dark maps: flashlight beams and IR must never become the only visibility source: keep ambient floor of 8% luminance.
- Never use pure black; darkest value `#0D1117`.

## 4. Materials and texel density
Trim-sheet workflow: one 2048² trim atlas per kit + decal atlas. Texel density: environment 256 px/m (tier L mip-biased to 128), characters 512 px/m (head 1024²), weapons FP 1024 px/m, bikes 512 px/m. Mask map: R metallic, G AO, B emission, A smoothness. No realtime reflection probes; one baked reflection probe per cell.

## 5. Environmental storytelling
Regrowth: vines, moss, pooled water in low areas, roots breaking pavement. Human traces: makeshift shelters (Scrap Crews), Concession orange tape, abandoned bikes, painted route markers. Fictional signage only, no real brands or real scripts. Density: props ≥1 story cluster per 40 m of street, none on ramps/landing zones. No dignity-stripping imagery of real communities.

## 6. Lighting rules
One directional light (baked + 1 realtime shadow cascade), baked GI, light probes, warm/cool contrast (tungsten vs LED). Interior emissives baked. Flashlights and gunfire are the only realtime lights (limit 4).

## 7. Production tiers
| Tier | What | Who |
|---|---|---|
| **T0** | correct-scale grey-box with sockets/colliders | AI scripts (bpy) |
| **T1** | kit-built with trim sheets, UVs, LODs, materials | AI scripts + human review |
| **T2** | hero assets: sculpted/painted detail, bespoke textures | artists, freelancers, licensed marketplace assets |
Ship gates: Slice T1 (+10 T2 props) · Beta T1 everywhere, T2 for bases, weapons, characters · Launch T2 for all player-facing hero content.

## 8. Asset workflow
Concept (image generators for **reference only**, logged) → blockout T0 → kit/T1 → textures → LODs → validation (`validate_asset.py`) → Unity import test → art review checklist → integrate. Folder: `Client/Assets/Art/{Characters,Weapons,Vehicles,Environment,VFX,UI}`; source `.blend` in `/Art/Source` (Git LFS).

## 9. Art review checklist
Scale/orientation correct · tris within budget · ≤3 materials · UVs 0–1, UV2 lightmap non-overlapping (env) · silhouettes readable at 30 m · palette compliance · no baked-in brand marks · license logged · tier L screenshot approved.
