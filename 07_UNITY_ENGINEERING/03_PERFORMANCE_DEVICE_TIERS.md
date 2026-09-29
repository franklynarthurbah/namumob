# Performance budgets and device tiers (budgets are gates, not goals)
> Read after: 07/01 · Used in: every phase; measured on real devices at every gate

## 1. Tiers
| Tier | Device class (examples, [VERIFY availability]) | RAM | Target |
|---|---|---|---|
| **L** | Helio G/Unisoc/Snapdragon 4xx-6xx phones (e.g. Tecno/Infinix/Redmi entry, Galaxy A1x), iPhone 8/X | ≥3 GB | 30 fps |
| **M** | Snapdragon 7xx, Dimensity 700-900, Galaxy A3x/A5x, Pixel 6a, iPhone XR/11/12 | 4–6 GB | 45 fps (60 optional) |
| **H** | Snapdragon 8-series, Galaxy S22+, Pixel 8, iPhone 13+ | ≥8 GB | 60 fps (90 optional) |

## 2. Settings matrix
| Setting | L | M | H |
|---|---|---|---|
| Render scale / max long edge | 0.70 / 1280 | 0.85 / 1600 | 1.0 / 2400 cap |
| Shadows | baked static + blob for characters | 1 cascade 40 m | 1 cascade 90 m soft |
| Texture mip limit | 1 (half-res) | 1 env, 0 characters/weapons | 0 |
| LOD bias / mid-prop draw distance | 0.7 / 60 m | 1.0 / 90 m | 1.3 / 130 m |
| Particles alive | 400 | 800 | 1500 |
| Bodycam lens | light (no noise/compression), FXAA | standard, FXAA | full, SMAA |
| Characters at LOD0/1 | 10 | 20 | 28 |
| Animation update reduction | every 2nd frame beyond 25 m | 40 m | 60 m |
| Audio voices | 24 | 32 | 48 |
| Feed Authenticity default | Light | Standard | Full |

## 3. Frame budgets
Frame time: L 33.3 ms · M 22.2 ms (45 fps) / 16.6 (60) · H 16.6 / 11.1.
**CPU main thread, tier M @45 fps (ms):** Sim+Net 2.5 · animation 2.5 · physics queries 1.2 · render submission 5.0 · HUD 1.4 (menus 0.8) · audio 0.8 · misc scripts 1.5 → ≈15 ms with ≥25% headroom. **GPU tier M:** scene 12 · lens pass 1.2 · UI 0.6 · post 1.0.
**Draw calls:** L 150 · M 220 · H 300. **Visible triangles:** L 350k · M 600k · H 900k.

## 4. Memory (whole app)
L ≤1.4 GB · M ≤1.8 GB · H ≤2.4 GB (iOS ≤1.6 GB on 4 GB devices). Managed heap ≤150 MB; textures ≤300/450/600 MB; meshes ≤100 MB; audio ≤60 MB (tier L). Texture streaming on. Zero per-frame allocations in Sim, Net, HUD.

## 5. Download, loading, data
Initial install ≤250 MB (Safehouse, tutorial, core); **Map A pack ≤700 MB**, other maps ≤500 MB each, downloaded per map with Wi-Fi advice and resume. Cold start → Lobby ≤12 s (M) / 20 s (L). Raid load ≤25 s (Slice ≤10 s). Reconnect ≤2 s. In-match data: see NamuNet budgets (≈15 MB per 25 min).

## 6. Tier detection and adaptation
First launch: read SystemInfo (RAM, GPU, cores, graphics API) → provisional tier → 8 s benchmark scene → store result; manual override in Settings. Backend supplies device-model/GPU-driver allow/deny lists (e.g. force GLES3 on a bad Vulkan driver). Adaptive Performance/thermal hooks [VERIFY]; dynamic render-scale steps of 0.05 to hold the target FPS; **Battery saver** option (30 fps cap).

## 7. Thermal and battery
Sustained 25-min raid on the tier-M reference device stays ≥80% of target fps; tier L holds 30 fps without throttling below 24 fps p1.

## 8. Profiling and CI
Unity Profiler, Frame Timing Manager, Memory Profiler, Snapdragon Profiler, Xcode Instruments/Metal capture. Automated **PerfRun** scenes (Slice flythrough, Base A6 fight with bots, bike ride at 90 km/h) run per release on real devices or a device farm [VERIFY provider] and record p50/p95/p99 frame times, memory and draw calls; thresholds fail the build.

## 9. Optimization playbook (in order)
SRP Batcher + shared materials → GPU instancing for props → LODs and cull distances → baked occlusion → mip streaming and texture caps → shader variant stripping → skinned-mesh reduction (combine, LOD, animator culling) → physics layer matrix + batched raycasts (`RaycastCommand`) → Burst/Jobs for interest management and AI perception → pooling everywhere → reduce transparency/overdraw → dynamic resolution.

## 10. Acceptance
Every phase gate includes a PerfRun on one device per tier · budgets recorded in `docs/STATE.md` · any budget breach blocks the phase.
