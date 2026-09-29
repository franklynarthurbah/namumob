# Builds, release, testing, CI
> Read after: 07/01, 07/03 · Used in: Phase 0 (scripts), Phase 10 (release) · [VERIFY] store rules on the day of submission

## 1. Android (AAB)
IL2CPP, **ARM64 only**, Vulkan first with GLES3 fallback + backend device blocklist, **targetSdk 36** (Google Play requirement for new apps/updates since 2026-08-31), minSdk 26 unless Unity 6.3 requires higher [VERIFY], **16 KB page-size compatibility** for native libs [VERIFY Unity 6000.3 support and third-party plugins], ASTC textures, Addressables + Play Asset Delivery for large packs, App Bundle split by ABI/density/texture format, Play App Signing (keep upload key offline), R8/minify on with keep rules for plugins, edge-to-edge UI. Predictive back handled. Play Integrity API plugin configured (`10_SECURITY_ANTICHEAT/02`).
## 2. iOS
Build with **Xcode 26+ / iOS 26 SDK** (Apple upload requirement since 2026-04-28), min iOS 16, arm64, Metal, IL2CPP. Capabilities: In-App Purchase, Push Notifications, App Attest. `PrivacyInfo.xcprivacy` completed (Unity and plugins' required-reason APIs) [VERIFY]. Export compliance: the app uses standard encryption → declare. **Account deletion in-app** and **Sign in with Apple** parity if other third-party sign-ins are offered [VERIFY guideline text]. Answer Apple's updated age-rating questions honestly (violence, chat, in-app purchases).
## 3. Dedicated server (Linux)
Unity Dedicated Server build target (Linux x64, headless, IL2CPP), stripped, `NAMU_SERVER`. Docker image: slim Debian/Ubuntu base + required native libs, non-root user, read-only filesystem except `/tmp`, health endpoint, Prometheus `/metrics`, graceful SIGTERM (finish match or hand-off), logs as JSON to stdout. Image tagged by build number; content hash printed at boot.
## 4. Versioning and channels
`MAJOR.MINOR.PATCH+build`; `protocol` and `content_hash` in handshake; backend `min_supported_build` (forced update) and `kill_switch` flags. Channels: **internal → closed beta (Play closed testing / TestFlight) → open beta → production** with staged rollouts (5% → 25% → 50% → 100%). New personal Play accounts may need a closed-test period with a minimum tester count before production [VERIFY]; organization accounts differ.
## 5. CI (GitHub Actions or GitLab CI)
Jobs: `lint` · `unity-editmode` · `unity-playmode` · `sim-plain-dotnet-tests` · `server-dotnet-test` · `adminweb-test` · `blender-validate` · `map-build` · `build-android` (GameCI editor image or self-hosted runner) · `build-ios` (macOS runner) · `build-server-linux` · `docker-build` · `security-scan` (dependency + secrets + container) · `perfrun` (nightly). Cache `Library/`; artifacts retained 30 days; secrets only via CI secret store; unsigned PR builds, signed release builds on tags.
## 6. Test pyramid
Unit (Core/Data/Sim/Net crypto/serializers) · determinism (client vs server hashes) · integration (server + BotClient scenarios: loot, extract, death, reconnect) · load (bots to 48/match, many matches) · network (netem profiles: 50/150/300 ms, 0/3/8% loss) · security (replay, tamper, fuzz, out-of-range inputs) · device smoke (install, login, deploy) · soak (2 h raids loop) · UI screenshot/monkey · localization overflow.
## 7. Crash/ANR and telemetry
Crash reporting via Sentry (Unity SDK) or Firebase Crashlytics [VERIFY choice, privacy disclosure]; native symbols uploaded per build; ANR watchdog. Keep crash-free sessions ≥99.5% and stay below Google Play's bad-behaviour thresholds [VERIFY numbers]. Gameplay telemetry per `08_BACKEND_SERVER_ADMIN/06`.
## 8. Store release checklist
Store listing, screenshots (per device class), feature graphic, icon (`06_UI_UX_BRANDING/01`), privacy policy URL, terms URL, support email · Play Data Safety form and IARC rating · Apple App Privacy details + age rating · IAP products created/approved (`09_MONETIZATION_IAP`) · reviewer notes with demo account and server status · reviewer-accessible tutorial path · **all servers live during review** · localized metadata for launch languages · export/legal declarations · data deletion flow and URL.
## 9. Rollback
Keep previous 2 server images and DB migrations reversible; feature flags for new systems; forced-update gate available before the store update propagates; incident template in `docs/runbooks/`.
## 10. Acceptance
One command builds each artifact from a clean checkout · signed release build passes store pre-checks (Play pre-launch report, TestFlight processing) · rollback rehearsed once before beta.
