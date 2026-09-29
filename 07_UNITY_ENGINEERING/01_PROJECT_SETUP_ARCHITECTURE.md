# Unity project setup and architecture
> Read after: 00_START_HERE/02 · Used in: Phase 0 · Unity **6000.3.9f1** only

## 1. Create the project
Use Unity Hub or CLI to create `/Client` from the **Universal 3D (URP)** template at exactly 6000.3.9f1; commit `ProjectSettings/ProjectVersion.txt`. Configure: IL2CPP, Managed Stripping Medium, Incremental GC on, Color Space Linear, Active Input Handling = Input System only, Api Compatibility .NET Standard 2.1, C# language version supported by 6.3. Player Settings by platform in `04_BUILD_RELEASE_TESTING_CI.md`.

## 2. Packages (use versions Package Manager marks compatible with 6000.3; no previews without approval)
`com.unity.render-pipelines.universal` · `inputsystem` · `addressables` · `ai.navigation` · `animation.rigging` · `splines` · `burst` · `collections` · `mathematics` · `test-framework` · `localization` · `purchasing` (IAP 5.x, `StoreController` API) · `mobile.notifications` · `transport` · `dedicated-server` · `multiplayer.playmode` · `profiling.core` · `memoryprofiler` · optional `adaptiveperformance` (+ vendor provider) [VERIFY] · SVG import: built-in UI Toolkit SVG support in 6.3 [VERIFY whether the vectorgraphics package is still needed]. **Do not** depend on Unity Gaming Services Multiplay or Relay; if a package pulls `services.core`, never initialize it without approval. Third-party libs (BouncyCastle for ChaCha20-Poly1305) go in `Packages/` with licence logged.

## 3. Assemblies (asmdefs; dependency direction is a rule, checked by an editor script)
| Assembly | Purpose | May reference |
|---|---|---|
| `Namulinda.Core` | logging, config, math helpers, service registry, bit I/O | — |
| `Namulinda.Data` | item/def loaders, JSON schemas, content hash | Core |
| `Namulinda.Sim` | deterministic gameplay: movement, weapon pose, damage, vehicles, stunts | Core, Data, Unity.Mathematics |
| `Namulinda.Net` | NamuNet transport/protocol/crypto/replication | Core, Data, Sim |
| `Namulinda.Server` | match director, world state, AI, loot, extraction, Dimming | Core, Data, Sim, Net |
| `Namulinda.Client` | camera/bodycam, input, presentation, audio, VFX | Core, Data, Sim, Net, UI |
| `Namulinda.UI` | UI Toolkit code | Core, Data |
| `Namulinda.Services` | HTTP API clients, auth, IAP, telemetry | Core, Data |
| `Namulinda.Security` | attestation, integrity, secure storage | Core |
| `Namulinda.EditorTools` | MapBuilder, validators, build scripts (Editor only) | all |
| `Namulinda.Tests.*` | EditMode/PlayMode | as needed |
`Namulinda.Sim` must not reference `UnityEngine` except Mathematics and a thin `ISimPhysics` interface (implemented in Client/Server with Physics queries). Scripting defines: `NAMU_CLIENT`, `NAMU_SERVER` (with `UNITY_SERVER`), `NAMU_DEV`.

## 4. Determinism rules
Simulation uses floats; **PRNG and tick ordering are integer-exact** (xorshift seeded from matchSeed/playerId/shotIndex). Client and server may diverge slightly across CPU architectures; reconciliation fixes it. Where Burst is used in Sim, apply `FloatMode.Deterministic` [VERIFY]. Hash tests run per platform. If bit-exactness is ever required, migrate movement to fixed-point Q16.16.

## 5. Data and config
Canonical **JSON** in `/Data/` (shared by Unity and .NET server), validated by schema in CI; loaded at startup with a **content hash** included in the NamuNet handshake. ScriptableObjects only for editor-time references (prefabs). Server process config via command line/env: `--port --matchId --region --map --mode --tickRate --backendUrl`.

## 6. Scenes
`Boot` (bootstrap, version gate, services) · `Lobby` (Safehouse) · `Raid` (additive cell scenes) · `ServerBoot` (headless). No gameplay markers hand-placed: MapBuilder generates them.

## 7. Boot sequence (client)
Init logging → config → version gate (`GET /v1/config`) → attestation warm-up → login → profile → Addressables catalog update → Lobby. Failure at any step shows a specific, retryable error.

## 8. Coding standards
C# 9 · `.editorconfig` + Roslyn analyzers, warnings as errors on new code · PascalCase, private fields `_camelCase` · `readonly struct` for hot data · no public fields unless `[SerializeField]` · zero steady-state GC allocations in Sim/Net/HUD · no LINQ/string concat/boxing in hot paths · no `Find*`, `Resources.Load`, `GetComponent` in Update · pools for VFX/projectiles/UI · `NativeArray` with explicit disposal · Burst/Jobs for heavy loops (interest, perception) · comments explain why, tests explain what.

## 9. Editor automation entry points
`Namulinda.EditorTools.CompileCheck.Run` · `BuildScripts.BuildAndroid/BuildIOS/BuildServerLinux` · `MapBuilder.BuildAll(mapId)` · `Validators.RunAll` (items, JSON, asmdef rules, art import) · `Pvs.Bake(mapId)`. All callable with `-executeMethod`.

## 10. Acceptance
Fresh clone → Phase 0 script builds Android dev APK, iOS Xcode project and Linux server without manual steps · asmdef rules checker passes · `Sim` compiles in a plain .NET test project (proves no hidden Unity dependency) · CI green.
