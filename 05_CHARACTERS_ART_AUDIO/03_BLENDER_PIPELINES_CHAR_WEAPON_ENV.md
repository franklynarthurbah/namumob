# Blender pipelines (characters, weapons, environment) and automation
> Read after: 01_ART_DIRECTION · Used in: Phase 0 (tooling), Phases 3–7 · Pin the Blender LTS version in `Tools/blender.version` [VERIFY current LTS].

## 1. Conventions
Metric units, 1 unit = 1 m. Names: `SM_` static mesh · `SK_` skinned · `M_` material · `T_<name>_A/N/M` textures (albedo/normal/mask) · `A_` animation · `LOD0..3` suffix · collision proxies `col_box_*`, `col_sphere_*` · sockets as empties `sock_*`. Transforms applied, origin at base (props) or at feet (characters). Triangulate on export. Max 4 bone influences/vertex.

## 2. Export (FBX) [VERIFY with an import test in Phase 0]
Forward −Z, Up Y, apply transforms, scale 1.0, tangent space on, no leaf bones, armature root named `root`. An **import test** in Unity (Editor script) checks bounds, pivot, socket names, bone count and scale for every asset; failures break CI.

## 3. Characters
Quad mesh with 4–5 edge loops at joints; base bodies sculpted/retopo'd once, clothing fitted with shrinkwrap + data transfer; weights via automatic weights + cleanup, then mirrored; blend shapes ≤12 per body; head sculpts share topology so hair/hats fit. Animations are exported as separate FBX clips (root at origin, no root motion). Source of clips: hand-keyed in Blender, licensed animation packs retargeted to Humanoid, or licensed mocap; log licences.

## 4. Weapons
Separate parts for animation: body, mag, bolt/slide, trigger, charging handle, safety, hammer/pump. Pivots at mechanical axes. Two meshes per weapon: **FP** (12–18k tris, interior detail) and **TP** (3–5k tris). Sockets: `sock_muzzle`, `muzzle_tip`, `sock_optic`, `sock_under`, `sock_mag`, `sock_stock`, `sock_laser`, `sock_eject`, `sock_flash`, hand IK targets `ik_hand_R/L`. Textures: 1024–2048 atlas, mask channels as §5 of the art bible; emission mask for ammo LED (diegetic ammo counter). **`gen_weapon.py --spec Data/Items/<id>.json`** builds a proportionate T0 weapon from a spec (barrel length, receiver profile, magazine curve, stock type) with all sockets and UVs, so AI can generate all 17 weapons quickly; artists later replace meshes without changing sockets.

## 5. Environment
Kit pieces snap to a 0.5 m grid; pivot bottom-left-back; UV1 trim/tiling, UV2 unique non-overlapping for lightmaps; collisions as simple boxes; LODs by `lod_bake.py`; materials from shared atlases. See Map A build guide for kits and budgets.

## 6. Automation (repository `/Tools/Blender/namu_blender/`)
`export_fbx()` (settings above) · `validate_scene()` (scale, transforms, names, tris, ngons, UV range, materials, sockets) · `bake_ao()` · `bake_trim.py` (procedural node materials → trim textures) · `make_contact_sheet.py` (turntable/thumbnail per asset into `docs/art-review/`) · `gen_*` generators from data. Each generator accepts `--seed`, writes a manifest JSON with tri counts/bounds/hash; CI compares hashes for determinism.
Commands: `blender -b -P Tools/Blender/<script>.py -- --out Client/Assets/Art/Generated --seed 1`.
Optional: a community Blender bridge lets an agent drive the Blender GUI interactively [VERIFY]; the pipeline must not depend on it.

## 7. Textures
Procedural Blender materials baked to trims; hand-painted layers in Krita or Blender texture paint; normals from high-poly bakes; compress ASTC (6×6 albedo, 4×4 normal). Optional commercial tools (Substance) allowed for artists, outputs only.

## 8. Tier plan (what AI does vs humans)
| Asset class | T0/T1 by AI scripts | T2 by humans |
|---|---|---|
| Buildings, props, containers, ramps | yes | base hero props |
| Weapons | blockouts + UVs + sockets | final FP hero models and animations |
| Characters | base topology + kit outfits | final sculpt, faces, hair cards, weights polish |
| Vehicles | parametric bikes | final bike models and skins |
| Animation | procedural/IK + retargeted packs | mocap/hand polish for FP |
Budget the human T2 work explicitly before beta.

## 9. Acceptance
Every exported asset passes `validate_scene()` and the Unity import test · generators are deterministic (same seed → same hashes) · contact sheets exist for every asset in review · no asset with unlogged licence.
