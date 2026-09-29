# Client hardening and platform attestation
> Read after: 01_THREAT_MODEL · Used in: Phase 9 · [VERIFY] exact SDK names/APIs at implementation time

## 1. Goals
Make casual tampering (memory editing, speed hacks, repackaged APKs) harder and detectable, and let the backend ask "is this a genuine, untampered app on a genuine device/OS?" before trusting a session for matchmaking or purchases. This is defense-in-depth on top of server authority (`01 §4`), not a replacement for it.

## 2. Android
- **Play Integrity API:** request a token at app start, before ranked/High-Stakes matchmaking, and before purchases; verify server-side (device integrity, app integrity, licensing verdict); cache a short-lived risk score, not the raw token.
- **IL2CPP + code stripping/minification**, symbol stripping in release, root/Magisk heuristic checks (best-effort signal only, not a hard block, since false positives hurt legitimate users), debugger/Frida/Xposed-hook heuristic detection, signature/package-name self-check, SafetyNet-successor guidance per current Play docs [VERIFY].
- Detect known emulator farms via Play Integrity device verdicts; treat pure emulator use as a risk signal, not an automatic ban (some players legitimately test on emulators).

## 3. iOS
- **App Attest** (DeviceCheck framework): generate a key at install, attest once, then use assertions for sensitive calls (matchmaking ticket request, purchase validate); verify assertions server-side against Apple's service.
- Jailbreak heuristics (best-effort signal) · IL2CPP with symbol stripping · anti-debug (`ptrace` deny where allowed by policy) [VERIFY App Store policy compliance for any anti-debug technique used].

## 4. Anti-tamper in the client
Integrity check of critical assemblies/assets at boot (hash compare against a signed manifest fetched from the backend, not embedded only); periodic re-checks; obfuscation of sensitive constants (item value tables live server-side anyway, limiting the payoff of client extraction); memory-scan resistance is best-effort — do not over-invest given diminishing returns (§ `01_THREAT_MODEL §5`).
**Never implement:** kernel-level anti-cheat drivers (inappropriate for a mobile game and a huge trust/privacy liability), reading of other apps' data, network scanning of the player's LAN, anything not disclosed in the privacy policy.

## 5. Session and risk scoring
Backend combines: attestation verdict freshness, device/account age, IP/ASN reputation (basic), number of accounts per device, historical flags → a **risk score** (`low/medium/high`). Effects: `low` normal play; `medium` extra server-side scrutiny + step-up attestation before purchases; `high` shadow-review queue, restricted matchmaking pool, or required re-verification. Scoring never silently bans; it feeds the human/detector pipeline in `03`.

## 6. Secure local storage
Refresh tokens and any local secrets in platform secure storage (Android Keystore-backed EncryptedSharedPreferences equivalent / iOS Keychain); never in PlayerPrefs in plaintext; wipe on logout; certificate pinning for the API host with a safe fallback (staged rollout of pin changes to avoid bricking on rotation) [VERIFY current best practice, since pinning can be brittle].

## 7. Build integrity pipeline
Reproducible CI builds, signing keys never on developer machines, checksums published internally per build, automated diff of release notes vs binary changes for audit.

## 8. Acceptance
Play Integrity/App Attest verified server-side end-to-end in staging · tampered-binary test (modified APK) is flagged · risk scoring visible in the admin anti-cheat console · no player-visible false hard-ban from heuristic-only signals (heuristics feed review, not auto-ban) · privacy policy lists every signal collected here.
