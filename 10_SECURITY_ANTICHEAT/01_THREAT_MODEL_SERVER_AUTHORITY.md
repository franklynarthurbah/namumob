# Threat model and server-authority rules
> Read after: 07/02 (NamuNet) · Used in: Phase 2 (foundations), Phase 9 (hardening) · This file is the "why"; 02/03/04 are the "how"

## 1. Assets to protect
Fairness (no aim/wall/speed/economy cheats) · accounts and inventory · payment integrity · server/infra availability · player data privacy · the brand.

## 2. Threat actors and motives
Casual cheaters (aimbot/ESP for fun) · commercial cheat sellers (profit, persistent) · botters/farmers (currency/rank for resale) · griefers/toxic players · scripted exploiters (API abuse, replay, economy dupes) · malicious insiders (admin abuse) · external attackers (DDoS, infra intrusion, data theft).

## 3. STRIDE-style pass on major flows
| Flow | Key risks | Primary mitigation |
|---|---|---|
| Login/session | credential stuffing, token theft | rate limits, device binding, short-lived JWT, refresh rotation |
| Match join | ticket forgery, ticket replay, wrong-server join | signed single-use MatchTicket, `jti` store, server-id binding |
| In-raid input | speed/no-clip/rapid-fire, spoofed shots | server-authoritative sim, input validation, deterministic weapon pose |
| Visibility | ESP/wallhack via full-state broadcast | interest management + PVS (never send what can't be seen) |
| Settlement | forged loot, item duplication, currency injection | signed settlement, conservation checks, idempotent apply |
| Purchases | fake receipts, replay | server-side store validation, idempotent by transaction id |
| Admin panel | privilege abuse, session theft | WebAuthn, RBAC, audit hash chain, two-person approval |
| Infra | DDoS, container escape, secret leak | provider UDP protection, least privilege, secret manager, image scanning |

## 4. Core doctrine: never trust the client
1. The client is a **renderer + input device**. It predicts locally for feel but the server's state is truth.
2. Anything that would give an unfair advantage if fabricated (position, damage, loot, currency, purchase, cosmetic unlock) is **computed and validated server-side**, never accepted as a client claim.
3. The client never receives data it should not see: no full-map entity list, no unrevealed loot contents, no other players' inventories. This is enforced by NamuNet interest management + PVS (`07/02 §10`), not by client-side hiding.
4. Every privileged action (settlement, purchase grant, ban) is **signed and idempotent**.
5. Secrets (signing keys, service credentials) never ship in the client or admin frontend bundle.

## 5. What client hardening can and cannot do
Hardening (obfuscation, integrity checks, attestation) raises the cost of cheating and catches tampering, but a determined attacker with full control of their own device can eventually defeat *any* client-side check. Therefore hardening is a **speed bump and telemetry source**, not the fairness guarantee — the guarantee is server authority (§4) plus statistical detection (`03`). Do not let hardening confidence justify skipping server-side validation anywhere.

## 6. Privacy-respecting boundaries
Anti-cheat telemetry is limited to what's disclosed in the privacy policy: gameplay inputs/outcomes, performance counters, attestation verdicts, and (only with clear disclosure and platform allowance) a narrow, documented set of integrity signals. No reading of unrelated files, contacts, browsing history, or other apps' data. No microphone/camera access beyond the in-game voice feature the player explicitly enables.

## 7. Assumptions and residual risk
GPU/driver-level or OS-level cheats (some emulator/root scenarios) may evade attestation; mitigate with statistical detection and platform risk signals rather than promising perfect prevention. Document residual risk in `docs/security/RESIDUAL_RISK.md` and revisit quarterly.

## 8. Acceptance
Every flow in §3 has an implemented mitigation traceable to a spec file below · a written statement exists (`docs/security/DOCTRINE.md`) confirming no gameplay-relevant value is ever trusted from the client · quarterly review scheduled.
