# API surface, database schema, lifecycles
> Read after: 01_BACKEND_ARCHITECTURE · Used in: Phase 4, 8, 9 · Generate EF Core migrations from this; keep OpenAPI in `/Docs/api/`

## 1. Public API (`/v1`, player JWT unless noted)
| Area | Endpoints |
|---|---|
| Auth (no JWT) | `POST auth/guest` · `auth/google` · `auth/apple` · `auth/refresh` · `auth/logout` · `POST auth/link` · `DELETE account` (30-day grace) |
| Config (no JWT) | `GET config` (min build, flags, maintenance, device profiles) · `GET content/manifest` |
| Profile | `GET/PATCH profile` · `GET safehouse` · `POST safehouse/upgrade` · `GET/POST aptitudes` |
| Inventory | `GET inventory` · `POST inventory/ops` (batch: move, equip, sell, buy, repair, craft; idempotent) · `GET/PUT loadouts/{slot}` |
| Traders | `GET traders/{id}/stock` · `POST traders/{id}/trade` |
| Contracts/Pass | `GET contracts` · `POST contracts/{id}/claim` · `GET pass` · `POST pass/claim` |
| Store | `GET store/catalog` · `POST store/purchase-cosmetic` (Cred) · `POST iap/validate` · `POST iap/restore` · `GET entitlements` |
| Matchmaking | `POST matchmaking/tickets` · `GET matchmaking/tickets/{id}` · `DELETE …` · party: `POST party`, `/invite`, `/join`, `/leave` + SignalR hub `/hubs/party` |
| Social/other | `GET leaderboards/{id}` · `POST reports` · `POST telemetry` (batched, gzip) · `GET reels/{matchId}` |
## 2. Internal API (`/internal`, game-server/worker credentials)
`POST servers/register|heartbeat` · `POST matches/{id}/started|events` (batched kills, extractions, flags) · **`POST raids/{id}/settle`** (signed) · `POST anticheat/flags` · `GET matches/{id}/manifest`.
## 3. Admin API (`/admin/v1`, separate listener)
Players (search, 360 view, timeline, notes) · inventory adjust (reason required; two-person above threshold) · currency grant/revoke · bans (create/revoke/appeals), kicks, mutes · reports queue · anti-cheat flags/cases · matches (list, detail, kill, drain) · servers/fleet (list, cordon, drain, deploy) · config versions (diff, stage, publish, rollback) · store (SKUs, offers, sales, seasons) · events calendar · announcements/push · audit log (read-only, hash verify) · admin users/roles/API keys · data export/delete · maintenance toggle.

## 4. Core schema (PostgreSQL; EF Core code-first)
Tables: `accounts`, `account_identities`, `devices`, `sessions`, `profiles`, `inventory_items`, `ledger_entries`, `item_events`, `raids`, `matches`, `match_players`, `purchases`, `entitlements`, `catalog_skus`, `pass_progress`, `bans`, `reports`, `anticheat_flags`, `servers`, `admin_users`, `admin_credentials`, `admin_sessions`, `audit_log`, `config_versions`, `events`.
```sql
CREATE TABLE ledger_entries(
  id bigserial PRIMARY KEY, account_id uuid NOT NULL, currency text NOT NULL,
  delta bigint NOT NULL, balance_after bigint NOT NULL CHECK (balance_after >= 0),
  reason text NOT NULL, ref_type text, ref_id text, idempotency_key text NOT NULL,
  created_at timestamptz NOT NULL DEFAULT now(), UNIQUE(account_id, idempotency_key));
CREATE TABLE inventory_items(
  id uuid PRIMARY KEY, account_id uuid NOT NULL, def_id text NOT NULL,
  state jsonb NOT NULL DEFAULT '{}', location text NOT NULL, slot text, raid_id uuid,
  version int NOT NULL DEFAULT 0, created_at timestamptz DEFAULT now(), updated_at timestamptz DEFAULT now());
CREATE INDEX ON inventory_items(account_id, location);
CREATE TABLE raids(
  id uuid PRIMARY KEY, match_id uuid NOT NULL, account_id uuid NOT NULL, status text NOT NULL,
  manifest_before jsonb NOT NULL, settlement jsonb, gear_value bigint NOT NULL, bracket text NOT NULL,
  created_at timestamptz DEFAULT now(), settled_at timestamptz, UNIQUE(match_id, account_id));
CREATE TABLE purchases(
  id uuid PRIMARY KEY, account_id uuid NOT NULL, platform text NOT NULL, product_id text NOT NULL,
  transaction_id text NOT NULL UNIQUE, status text NOT NULL, amount_micros bigint, currency text,
  created_at timestamptz DEFAULT now(), granted_at timestamptz, refunded_at timestamptz);
CREATE TABLE bans(
  id uuid PRIMARY KEY, account_id uuid NOT NULL, kind text NOT NULL, reason_code text NOT NULL,
  evidence_ref text, created_by text NOT NULL, created_at timestamptz DEFAULT now(),
  expires_at timestamptz, revoked_at timestamptz, appeal_status text);
CREATE TABLE audit_log(
  id bigserial PRIMARY KEY, actor_type text, actor_id text, action text NOT NULL, target_type text,
  target_id text, before jsonb, after jsonb, ip inet, at timestamptz DEFAULT now(),
  prev_hash bytea, hash bytea NOT NULL);       -- hash chain for tamper evidence
```
Personal data columns are encrypted or hashed where possible (email, IP hash); retention policy in `06`.

## 5. Raid lifecycle (state machine)
`locked` → (`extracted` | `dead` | `mia` | `refunded`).
1. **Enter (match formed):** in one transaction lock selected items (`location = in_raid`, `raid_id`), snapshot `manifest_before`, compute gear value/bracket, mint MatchTicket.
2. **Settle** (`/internal/raids/{id}/settle`, Ed25519-signed by the assigned server; body: outcome, items_out with origins, stats, event-log hash):
 - verify signature, server assignment, idempotency by `raid_id`;
 - **conservation:** returned original item ids ⊆ manifest_before; new items must reference server loot events (container/AI ids) consistent with the match seed and container capacity; anomalies are flagged (not silently accepted);
 - currency changes only from backend-computed bonuses (extraction, contracts), never from the server body;
 - `extracted`: items to stash (overflow → mailbox 7 days); `dead`: carried items destroyed except Safe Case + **insured** (→ `insured_pending`, due 45 min); `mia`: same as dead.
3. **Refund:** if unsettled beyond match max + 10 min or server crashed, restore `manifest_before` to stash and discard loot.

## 6. Purchase lifecycle
`pending` → `validated` (store verified) → `granted` (entitlement + currency ledger, idempotent by transaction_id) → optionally `refunded/revoked` (store notification) . Details in `09_MONETIZATION_IAP/02`.

## 7. Acceptance
OpenAPI generated for all three surfaces · every table has migration + index review · conservation tests reject forged items · audit hash chain verifies · admin actions always produce audit rows.
