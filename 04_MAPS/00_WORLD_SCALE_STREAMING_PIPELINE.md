# World scale, streaming and generation pipeline (all maps)
> Read after: 02_GAME_DESIGN/02 · Used in: Phase 3 (Slice-01), Phase 7 (full Map A)

## 1. Scale
Map A: 6000×6000 m = **12×12 cells of 500 m**. Grid letters A–L west→east, rows 1–12 north→south (`col = floor(x/500)`, `row = 12 − floor(z/500)`). Maps B/C: 4000 m (8×8 cells). Cell scene naming: `MapA_C{col}{row}` e.g. `MapA_H11`.
**Slice-01** (built first): cells G–I × rows 10–12 (x 3000–4500, z 0–1500): Harbor Bastion, Pier 9, part of Harborline, lake edge, ramps, 3 POIs, one extraction point, AI garrison, boat + bike.

## 2. Streaming rules (client)
| Ring | Radius | Contents |
|---|---|---|
| Active | 500 m | full meshes, colliders, navmesh link, props, loot, AI proxies |
| Visual | 1,500 m | LOD1 buildings, terrain, impostors, no small props, no colliders except roads/large buildings |
| Far | 3,000 m | terrain low LOD + skyline impostors + fog |
Load 3×3 cells around the player plus 1 cell lookahead along velocity (riding). Async additive scene loads, activation throttled to ≤8 ms/frame. Memory: ≤13 cells resident. Addressables groups per 3×3 **tile pack** so downloads can be per-region; fog + bodycam vignette hide seams.
**Server variant:** the same pipeline emits **server cells** (collision + navmesh + spawner data only, no renderers/materials). Server loads cells that contain players/AI/vehicles.

## 3. Terrain and water
Unity Terrain per cell: heightmap 513² (~0.97 m/sample), max 4 texture layers, shared edge samples for seamless stitching. Height data from `Tools/Blender/terrain_gen.py` (noise + hand-authored masks) or Gaea/World Machine if the owner prefers; output 16-bit RAW. Trees/grass via GPU-instanced detail with strict counts per tier. Lake: single water mesh, simple URP water shader (shore foam, depth fade), no realtime reflections/refraction on tier L.

## 4. Roads, ramps, splines
Unity Splines package for roads/rails/canals. Roads are generated meshes with decal overlays; each surface has a physics material tag (tarmac/dirt/wet/sand) used by vehicles. Ramps are placed from `layout.json` using the catalogue in `03_VEHICLES/01`.

## 5. Buildings
~120 **modular pieces** (walls 4×3 m, corners, windows/doors, floors, stairs, roofs, balconies, facades per district style) → assembled by rule-based generators from `layout.json` (district density, height range, palette). **Bases and hero POIs are hand-authored from the same kits + unique props.** Interiors: kit rooms with 8 layouts per building class; only enterable buildings get interiors (~30% of footprint), others are façade-only with sealed doors.

## 6. Culling, lighting, navigation
Baked occlusion culling per cell · LOD groups on everything >200 tris · cull distance per layer (small props 60 m) · one directional light with baked lightmaps (1–2 atlases 1024² ASTC per cell) + light probes/APV [VERIFY mobile support] · time-of-day variants: Dawn, Noon, Dusk (default), Night · NavMesh baked per cell (Human agent), 8 m overlap or off-mesh links at borders · **PVS**: coarse potential-visibility between 32 m sub-cells baked by ray sampling, used by the server to withhold replication (see `10_SECURITY_ANTICHEAT/01`).

## 7. Data-driven placement
`layout.json` (see Map A) drives: Blender generators, Unity **MapBuilder** editor tool (spawns `AISpawnPoint`, `LootContainer`, `ExtractionPoint`, `VehicleSpawn`, `Ramp`, `SoundZone`, `ExposureZone`, `PlayerSpawnHint`), the server world bootstrap, and the minimap. One source of truth; never hand-place gameplay markers in scenes.

## 8. Pipeline (CI target `make map-a`)
1 terrain gen → 2 road/spline gen → 3 building/kit assembly → 4 export FBX + collision proxies → 5 Unity import + prefab + scene generation → 6 bake lighting/occlusion/navmesh → 7 PVS bake → 8 Addressables build (client + server catalogs) → 9 automated flythrough (20 waypoints) recording frame time, memory, draw calls → 10 report to `docs/reports/`.

## 9. Map budgets (per tier, in-view)
| | L | M | H |
|---|---|---|---|
| Triangles | 350k | 600k | 900k |
| Draw calls/batches | 150 | 220 | 300 |
| Unique materials | 60 | 90 | 120 |
| Resident memory (whole app) | 1.4 GB | 1.8 GB | 2.4 GB |
| Cell load hitch | <30 ms | <25 ms | <20 ms |

## 10. Acceptance
Slice-01 flythrough meets tier L budget · cell streaming causes no frame >50 ms during a 90 km/h ride · server memory per match <2 GB · MapBuilder rebuild is deterministic (hash-equal scenes).
