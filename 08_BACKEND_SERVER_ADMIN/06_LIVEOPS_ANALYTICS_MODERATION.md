# Live-ops, analytics, moderation, support
> Read after: 04 · Used in: Phase 4 (telemetry ingest), Phase 10 (dashboards), Phase 11 (live ops)

## 1. Telemetry
**Event schema:** `{event_id, name, ts, player_id (pseudonymous), session_id, build, platform, device_tier, region, props{}}`, batched every 30 s or 50 events, gzip, sent to `/v1/telemetry`; server-side events come from game servers/backends.
**Taxonomy:** session/boot · FTUE steps · raid lifecycle (queue, match, drop, loot, fight, extract/die) · combat (shots/hits aggregated per 10 s, not per bullet) · economy (source/sink) · store funnel (view, select, confirm, validated, granted) · performance (fps buckets, hitches, memory warnings, thermal) · network (RTT/loss/jitter buckets) · security (attestation fail, tamper flags) · UX (screen time, errors).
**Storage:** Postgres partitions for beta → ClickHouse when volume demands. Raw retention 90 days, aggregates 24 months. **Privacy:** no precise location, no contacts, no advertising IDs; player_id rotates on account deletion; disclose in privacy policy.

## 2. KPIs and targets (beta baselines, tune after)
FTUE completion ≥80% · D1 ≥30% / D7 ≥12% / D30 ≥5% · raids per DAU ≥3 · extraction rate 45–60% by bracket · matchmaking p95 <45 s · crash-free ≥99.5% · server tick p99 ≤25 ms · store conversion and ARPDAU tracked (no fixed target before beta). Guardrails for every experiment: crash-free, session length, D1, complaint rate.

## 3. Remote config, flags, experiments
Feature flags with percentage/segment rollout (platform, tier, region, build), kill switches per system (IAP, voice, each mode), A/B tests with pre-declared metric + guardrails, automatic rollback if guardrails breach.

## 4. Live events and comms
Calendar in the admin panel schedules: weekly rotations, 4-week events, community goals, seasons (12 weeks), maintenance windows (announced ≥24 h ahead). Content delivered via Addressables/config; localized banners; **push notifications** through FCM/APNs to opted-in segments only, quiet hours respected, frequency caps.

## 5. Moderation and Trust & Safety
- **Reports:** post-raid report button → queue with chat excerpts, match link, reporter/target history.
- **Auto-mute:** chat filter per language; thresholds mute automatically for review.
- **Ladder:** warning → mute 24 h → 7 d → 30 d → permanent; cheating follows `10_SECURITY_ANTICHEAT/03`.
- **Appeals:** in-game form → admin queue with SLA (72 h) and templated responses; transparent ban notice with reason code.
- **Minors:** age gate; under-16 accounts use quick-chat/ping only (no free text or voice by default); reporting is prominent; escalate grooming/abuse reports to a dedicated high-priority queue with legal-escalation runbook.
- **Voice:** squad-only, push-to-talk default, per-player mute, report with short buffered clip only if legally cleared and disclosed [VERIFY].

## 6. Support
In-game contact form → support queue; purchase issue tool (search by transaction id, view store state, fix entitlement, refund note); account recovery via linked identity + device history; data export/deletion within legal time limits; canned macros in `/Docs/support/`.

## 7. Community
Discord/webhook integration for staff alerts; public status page; patch notes template; known-issues list in-game.

## 8. Season calendar template
Week −4 content freeze · −2 QA/perf gate · −1 store assets/pass content · 0 launch (staged) · 3 mid-season event · 6 balance patch · 9 pre-finale event · 11 finale · 12 wrap + rewards.

## 9. Runbooks
Incident triage · ban wave · economy exploit (freeze affected item defs, rollback ledger slice) · mass refund · store outage · chat abuse surge · legal takedown request.

## 10. Acceptance
Dashboards exist for every KPI above · a flag can disable a feature within 60 s for all clients · appeal → decision flow works end-to-end · privacy disclosures match the collected events (audit script diffs the schema vs the policy list).
