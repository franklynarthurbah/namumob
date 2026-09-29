# Copy everything below the line into the repo root as `CLAUDE.md`
---
# CLAUDE.md — NAMULINDA repo rules
Unity 6000.3.9f1 mobile extraction shooter (bodycam look, stunt bikes). Server-authoritative. Self-hosted backend + admin panel.

## Where things live
- Spec pack: `docs/prompt-pack/` (read on demand; never load all of it).
- Master prompt: `docs/prompt-pack/00_START_HERE/01_MASTER_PROMPT_CLAUDE_OPUS_5_5.md`
- Locked decisions: `docs/prompt-pack/00_START_HERE/02_LOCKED_DECISIONS_AND_CONSTANTS.md`
- Session memory: `docs/STATE.md` (update every session) · `docs/DECISIONS.md` · `docs/QUESTIONS.md` · `docs/LICENSES.md`
- Layout: `/Client` Unity · `/Server` .NET · `/AdminWeb` React · `/Infra` Docker/K8s/CI · `/Tools/Blender` bpy scripts · `/Docs`

## Commands (edit UNITY path once)
```
UNITY="/Applications/Unity/Hub/Editor/6000.3.9f1/Unity.app/Contents/MacOS/Unity"   # macOS
# Windows: "C:\Program Files\Unity\Hub\Editor\6000.3.9f1\Editor\Unity.exe"  Linux: ~/Unity/Hub/Editor/6000.3.9f1/Editor/Unity
"$UNITY" -batchmode -nographics -quit -projectPath ./Client -logFile - -executeMethod Namulinda.EditorTools.CompileCheck.Run
"$UNITY" -batchmode -projectPath ./Client -runTests -testPlatform EditMode -testResults ./TestResults/edit.xml -logFile -
"$UNITY" -batchmode -projectPath ./Client -runTests -testPlatform PlayMode -testResults ./TestResults/play.xml -logFile -
dotnet build ./Server && dotnet test ./Server
(cd AdminWeb && npm ci && npm run lint && npm run build && npm test)
docker compose -f Infra/compose/dev.yml up -d
blender -b -P Tools/Blender/build_kit.py -- --out Client/Assets/Art/Generated
```
(`-nographics` is fine for compile/tests, not for baking or shader work.)

## Rules
1. Server-authoritative: the client sends input only. Never trust client values (position, damage, inventory, prices, receipts).
2. No secrets in the client or git. Config via env/secret stores. Never log tokens, receipts, passwords.
3. Unity: no `Update()` gameplay logic in server sim (use the NamuNet fixed tick); zero steady-state GC allocations in hot paths; no LINQ in hot paths; no `Find*`, no `Resources.Load`; content via Addressables; data via ScriptableObjects/JSON with schema + validation.
4. Every gameplay rule lives in `Namulinda.Sim` (engine-light, shared by client prediction and server). Rendering/UI never mutate sim state.
5. UI is UI Toolkit (UXML/USS). Touch targets ≥ 9 mm (≈ 48 dp). Honour safe areas (Android 16 edge-to-edge).
6. Keep budgets from `07_UNITY_ENGINEERING/03` – a budget breach fails the task.
7. Small conventional commits; feature branches per phase; PR description = what/why/how tested.
8. Do not guess Unity/Android/iOS APIs; verify in 6000.3 docs or with a compile check. Mark volatile facts [VERIFY] and check them.
9. Before finishing a task: compile, run tests, run the phase gate checklist, update `docs/STATE.md`.
10. Ask the owner only via `docs/QUESTIONS.md`; continue with the safest default.
