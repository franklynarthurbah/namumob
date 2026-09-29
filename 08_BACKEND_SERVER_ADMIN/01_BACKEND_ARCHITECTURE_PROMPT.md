# Backend architecture prompt (.NET) — accounts, economy, raids, config
> Read after: 00_START_HERE/02, 07/02 · Used in: Phase 4 (MVP), 8, 9, 10 · Companion: 02 (API/DB), 03 (server/matchmaking/fleet), 05 (deploy)

## 1. Mission
Build the self-hosted backend that owns everything the client must never own: accounts, inventory, currencies, raid lifecycle, matchmaking, store/entitlements, config, telemetry, moderation. Stack (locked): **.NET 10 LTS · ASP.NET Core · EF Core/Npgsql · PostgreSQL 17+ · Redis 7+ (or Valkey) · SignalR**. Modular monolith first, split later only when measured.

## 2. Solution layout
```
/Server
  Namulinda.sln
  src/Namulinda.Shared          DTOs, protocol constants, item-def loader (shares /Data JSON), crypto helpers
  src/Namulinda.Domain          entities + rules (inventory, ledger, raid settlement, brackets, gear value)
  src/Namulinda.Infrastructure  EF Core, Redis, object storage, Google/Apple clients, attestation verifiers
  src/Namulinda.Api             public API (/v1), internal API (/internal), admin API (/admin/v1, separate listener)
  src/Namulinda.Fleet           fleet control plane + host agent
  src/Namulinda.Workers         matchmaker, raid reconciler, anti-cheat processors, jobs
  tests/*                       unit, integration (Testcontainers), contract, load (NBomber/k6)
```
Public, internal and admin surfaces run on **separate listeners/ports** with separate auth schemes.

## 3. Cross-cutting rules
Config from env/secret files only · structured JSON logs (Serilog) · OpenTelemetry metrics/traces → Prometheus · health checks (`/healthz`, `/readyz`) · ASP.NET Core rate limiting + Redis distributed limits · **idempotency**: `Idempotency-Key` header stored 24 h for all mutating player endpoints · validation on every DTO · errors as RFC 9457 problem details · versioned routes `/v1` · UTC everywhere · UUIDv7 ids · optimistic concurrency (`version` columns) · explicit transactions, serializable for money/inventory · pagination cursors · no secrets or tokens in logs.

## 4. Authentication and authorization
**Players:** EdDSA JWT access token (15 min) + rotating refresh token (30 days, device-bound, stored hashed). Providers: guest (device-bound secret), Google Play Games (validate server auth code), Sign in with Apple (validate identity token + nonce), optional email magic link. Linking/merge rules: never merge two accounts with inventory automatically; require choose-one with confirmation. Attestation (Play Integrity / App Attest verdict + freshness) required for: matchmaking tickets, purchases, account linking, high-value trades; risk score can add friction (step-up) rather than instant denial.
**Admins:** separate identity store; **WebAuthn passkeys** (Fido2NetLib) + TOTP fallback; 8 h sessions; optional IP allowlist; RBAC; step-up for destructive actions; two-person approval for large economy edits.
**Service-to-service:** mTLS + audience-scoped JWTs; game servers authenticate with per-launch credentials from the fleet manager (2 h life) and sign settlements with an Ed25519 key.

## 5. Domain rules (server truth)
- **Ledger:** all currency changes go through `LedgerService.Apply(idempotencyKey, entries[])`; balances never negative; cached balance in `profiles` updated in the same transaction; nightly reconciliation alarms on any mismatch.
- **Inventory:** items are instances (UUIDv7) with `location` (stash, loadout, in_raid, insured_pending, mailbox) and JSONB state; every move is an audited event.
- **Raid lifecycle:** lock gear at match formation → settle by server-signed message → validate conservation → apply (extracted / dead / MIA) → refund on timeout. Details in `02`.
- **Brackets:** Gear Value from trader buy prices; `effective_bracket = max(bracket(gearValue), bracket(threatRating))` (Threat Rating = mean extracted value of last 5 raids).
- **Store:** cosmetics for Cred (server-side), IAP validation, entitlements, Feed Pass progress; **no randomized paid rewards**.
- **Config:** versioned remote config (economy factors, loot tables, prices, feature flags, min build, kill switch, maintenance, device profiles) with draft → staged → live → rolled_back.

## 6. Non-functional targets
API p95 <150 ms at 500 concurrent players; stateless API instances (scale out); Postgres pool sizing documented; background jobs as hosted services with Postgres advisory locks; events between components via **Redis Streams** (telemetry, anti-cheat, notifications); zero downtime deploys for API (blue/green).

## 7. Testing
Property tests: ledger conservation and no-negative balances; inventory duplication attempts; concurrent settle/purchase races (Testcontainers); contract tests generated from `Namulinda.Shared` DTOs used by Unity; fuzzed inputs; load test 500 CCU scripted flows (login → queue → settle).

## 8. Deliverables per phase
Phase 4: auth, profile, inventory ledger, matchmaking tickets, raid enter/settle, config, telemetry ingest, admin API MVP. Phase 8: store/IAP/pass. Phase 9: anti-cheat ingestion + ban system. Phase 10: performance and ops hardening.

## 9. Acceptance
Concurrent double-settle produces exactly one settlement · double-spend attempts fail · every mutating endpoint has an idempotency test · migrations are reversible · OpenAPI spec generated and checked into `/Docs/api/`.
