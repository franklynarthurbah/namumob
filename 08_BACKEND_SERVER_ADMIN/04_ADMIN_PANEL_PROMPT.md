# Admin panel prompt — "SCB Operations Console"
> Read after: 01, 02, 03 · Used in: Phase 4 (MVP), 8 (store tools), 9 (anti-cheat console), 10 · Design guidance: `06_UI_UX_BRANDING/02` (calmer variant)

## 1. Purpose and users
One web console to run the game: players, moderation, anti-cheat, live matches, servers, economy, store, events, support, analytics, system settings. Users: Owner, Admin, LiveOps, Moderator, Support, Analyst, Read-only.

## 2. Stack and structure
React 19 + TypeScript + Vite · React Router · TanStack Query + TanStack Table · lightweight charts (uPlot or Recharts) · CSS variables tokens (no heavy UI kit; small headless primitives allowed) · Vitest + Testing Library + Playwright + axe accessibility checks. Served as static files by Caddy at `admin.<domain>`, talking only to `/admin/v1` on the separate admin listener. Folder `/AdminWeb/src/{app,features/*,components,lib,styles}`; typed API client generated from OpenAPI.

## 3. Information architecture
Overview · **Players** · **Moderation** (Reports, Anti-cheat, Bans, Appeals) · **Matches & Fleet** · **Economy** (Config, Loot tables, Prices, Simulator stub) · **Store** (SKUs, Offers, Feed Pass seasons) · **Live Ops** (Calendar, Announcements, Push) · Support · Analytics · **System** (Admin users, Roles, API keys, Audit log, Maintenance/Kill switch, Data requests).
Command palette (Ctrl/Cmd+K), keyboard shortcuts (`/` search, `g p` players, `g m` moderation), deep-linkable filters, saved views.

## 4. Page specifications
- **Overview:** live CCU by region, matches running, matchmaking wait p50/p95, allocation success, revenue today (from validated purchases), crash-free %, open flags/reports, alerts feed. Auto-refresh 10 s.
- **Players:** search by id/name/email (masked)/device/transaction id. **Player 360:** profile, linked identities, devices + attestation history, timeline (logins, raids, purchases, bans, flags), inventory + diff viewer, ledger, matches, notes. Actions (reason required, audited): grant/revoke currency, adjust inventory (two-person approval above threshold), reset/unlink identity, ban/mute/kick, force logout, delete data.
- **Reports:** queue with reason, chat excerpt, match link; assign, resolve, escalate; templates for penalties.
- **Anti-cheat console:** flag queue by severity/detector, per-player risk score, evidence panel (event timeline, 2D map of shots/hits, input stats), compare-with-cohort charts, **ban waves** (select flagged accounts, schedule delayed bans), shadow-pool controls, case notes, outcome labelling (feeds detector tuning).
- **Bans/Appeals:** list, filters, revoke, appeal workflow with SLA timers.
- **Matches & Fleet:** live match list (map, players, elapsed, tick p99), match detail (players, events), actions: end/drain match; hosts by region (capacity, load, version, warm pool), cordon/drain/deploy version, orphan cleanup.
- **Economy:** config versions (draft → staged → live → rolled back) with JSON-schema forms and **diff view**, staged rollout percentages, guardrail metrics; loot tables editor with weight sums validated to 100%; sell factors, fees, Aptitude caps.
- **Store:** SKUs, prices in Cred, store product mapping, schedules, bundles, Feed Pass seasons; preview cosmetic; **cannot create randomized paid items** (validator).
- **Live Ops:** event calendar (drag to schedule), modifiers, announcements/banners (localized), push campaigns with opt-in segments.
- **Support:** purchase lookup by transaction id, restore/entitlement fix, account recovery checklist, data export/delete requests with SLA.
- **Analytics:** KPI dashboards (retention, FTUE funnel, extraction rate by bracket/map, economy health, performance by device tier).
- **System:** admin users, roles, WebAuthn keys, API keys, IP allowlist, **audit log** with hash-chain verification, maintenance mode and kill switch (per feature), data requests.

## 5. RBAC (✓ allowed, A approval needed)
| Capability | Owner | Admin | LiveOps | Moderator | Support | Analyst | Read-only |
|---|---|---|---|---|---|---|---|
| View players/matches | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Ban/mute/kick | ✓ | ✓ | | ✓ | mute | | |
| Currency/inventory edits | ✓ | A | | | small refunds | | |
| Economy config publish | ✓ | A | ✓ | | | | |
| Store/offers | ✓ | ✓ | ✓ | | | | |
| Fleet actions | ✓ | ✓ | | | | | |
| Admin users/roles | ✓ | | | | | | |
| Audit log read | ✓ | ✓ | | | | ✓ | |
| Data export/delete | ✓ | ✓ | | | ✓ | | |
Large edits (e.g. >100k Scrip or >10 items) need a second approver (A).

## 6. Security requirements
WebAuthn required (TOTP fallback), 8 h sessions, idle timeout 20 min · access token in memory, refresh cookie `HttpOnly; Secure; SameSite=Strict` · strict CSP (no inline scripts), no third-party scripts · every mutation sends a reason and lands in the audit log · PII masked by default; "reveal" is an audited action · rate limiting and lockouts · IP allowlist/VPN option · break-glass account stored offline · no admin routes on the public listener.

## 7. Design direction
Operations-console feel tied to the fiction: warm slate neutrals (`#11151B` base, `#171D25` surfaces), Laterite for primary actions, Feed Cyan **only** for live signals, REC Red only for danger. Dense, legible tables with mono numerals (JetBrains Mono), headings in Chakra Petch, body Barlow. Left rail with a region-status ticker; split panes (list + detail); timelines for players/matches; no decorative gradients or card grids of icons; dark default, light theme optional; WCAG AA; responsive down to tablet.

## 8. Tests
Unit/component tests for forms and tables · Playwright E2E for critical flows (login with WebAuthn stub, ban, approve edit, publish config, rollback, maintenance toggle) · axe checks · permission tests per role · audit-log assertions for every mutation.

## 9. Acceptance
Every mutation audited and permission-checked server-side (UI hiding is not security) · config rollback restores prior values exactly · ban wave scheduler is idempotent · Overview loads in <2 s with 10k players · keyboard-only navigation works.
