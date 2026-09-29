# Map A — Blender + Unity build guide
> Read after: 04_MAPS/00, 01–03 · Used in: Phase 3 (Slice-01), Phase 7 · Art tiers: **T0** blockout (grey boxes, correct scale) → **T1** kit-built with trim sheets → **T2** hero dressing (bases only, artist/marketplace assisted)

## 1. Blender toolchain (headless, in `/Tools/Blender`)
`blender -b -P <script> -- <args>`; every script reads `layout.json`, is seedable and outputs FBX + a manifest JSON (piece names, tris, bounds).
| Script | Output |
|---|---|
| `kit_build.py --kit <name>` | modular pieces for a kit (walls, corners, windows, doors, floors, stairs, roofs, balconies, props) |
| `gen_district.py --district <id> --seed N` | building shells + LODs from kit + district rules |
| `terrain_gen.py --cell <col><row>` | 16-bit RAW heightmap + splat mask per cell |
| `roads_gen.py` | road/rail meshes from spline polylines, surface tags |
| `props_scatter.py` | instanced prop placement data (not meshes) |
| `containers_gen.py` | shipping containers, crates, pallets, barriers |
| `ramp_gen.py` | all ramp types from `03_VEHICLES/01` with chevron markings |
| `lod_bake.py` | LOD1/2 + impostor cards for far buildings |
Blender version pinned in `Tools/blender.version`; add-ons limited to bundled ones.

## 2. Kits (each: 40–120 pieces, one 2048² trim atlas + shared decal atlas)
Highrise (glass curtain wall, concrete frame) · Market low-rise (corrugated roofs, brick, awnings) · Industrial/refinery (pipes, tanks, gantries, flare stacks) · Airfield (hangar trusses, tower, runway lights) · Villa/residential (walls, terraces, pools) · Hospital (wings, corridors, wards) · Harbor (containers, cranes, quays, sheds) · Stilt houses (boardwalks, poles, tarp roofs). Fictional signage only.
**Budgets:** wall module 80–200 tris · window frame ≤120 · mid-rise shell LOD0 ≤3k, LOD1 ≤1k, LOD2 box + impostor · hero prop ≤2k · small prop ≤400 · crane ≤6k. Materials per building ≤3.
**Textures:** albedo (RGB+alpha mask), normal, **mask** (R metallic, G occlusion, B emission, A smoothness); ASTC 6×6 albedo, 4×4 normal; props 512–1024, trims 2048. Decal atlas 2048² (stains, graffiti, cracks, signs). Vegetation atlas 1024.

## 3. Shader/materials
`NAMU_Env_Lit` Shader Graph (URP): albedo/normal/mask, **wetness** (rain, world-space), **moss/dirt** from vertex colour, emission for Halo LEDs, optional Dimming tint. Max 3 texture samples. Variants stripped per tier. One material per trim atlas.

## 4. Unity build (Editor scripts, no hand placing)
1. Import FBX with `AssetPostprocessor` (scale 1, no material import, generate LODs off since baked, colliders from proxies).
2. `MapBuilder` reads `layout.json` → generates cell scenes `MapA_C{col}{row}`: terrain, road meshes, buildings, ramps, spawners, containers, ExposureZones, SoundZones.
3. Static flags (Batching, Occluder/Occludee, Contribute GI), lightmap settings (1024 atlases, ASTC, baked indirect, 1 realtime directional light).
4. Bake occlusion, lighting, NavMesh, PVS (`Tools/PVS/Bake` editor tool).
5. Addressables groups per 3×3 tile pack; server catalog with collision-only cells.
6. Flythrough test scene (20 waypoints) writes frame time/memory/draw calls to `docs/reports/`.

## 5. Milestone art targets
| Milestone | Scope | Tier |
|---|---|---|
| Slice-01 | Harbor Bastion + docks + 3 POIs | T1 (base has T2 hero props: 10) |
| Alpha | full Map A blockout | T0 everywhere, T1 in 4 districts |
| Beta | full Map A | T1 everywhere, T2 in 6 bases |
| Launch | + polish, decals, ambient life | T1/T2 |

## 6. Acceptance
`make map-a` reproducible (same seed → same manifest hashes) · per-cell triangle counts within `00 §9` · texture memory ≤40 MB per resident cell · all kit pieces pass an import test (scale, pivot, naming) · no missing collision on drivable surfaces.
