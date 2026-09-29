# Detection pipeline, bans, appeals
> Read after: 01, 02 · Used in: Phase 9 · Pairs with admin console `08_BACKEND_SERVER_ADMIN/04 §"Anti-cheat console"`

## 1. Detection philosophy
Layered signals, human-in-the-loop for anything that ends a career, fast automated action only for high-confidence, high-severity, low-false-positive cases (e.g. cryptographically invalid packets, provably impossible movement). Everything else queues for review with evidence.

## 2. Server-side statistical detectors (run on `Namulinda.Workers`, fed by match events/telemetry)
| Detector | Signal | Response |
|---|---|---|
| **Movement envelope** | speed/accel/teleport beyond `Sim` limits, no-clip through validated collision | auto-correct + count; repeated → flag |
| **Aim analytics** | snap angle deltas, flick-to-hitbox correlation, reaction time distribution vs population, tracking smoothness outliers | flag with severity from z-score vs cohort (bracket/device/input method) |
| **Wallbang/prefire anomalies** | firing accurately at unseen targets before PVS reveal | high-severity flag (this should be structurally impossible if `07/02 §10` is correct — treat any occurrence as a priority bug *and* a cheat signal) |
| **Recoil/spread perfection** | statistically perfect pattern control far outside human variance | flag |
| **Economy anomalies** | improbable loot value velocity, settlement conservation violations, currency graph outliers (many-to-one transfers) | auto-hold suspicious settlement for review; flag account pair |
| **Multi-accounting/boosting** | shared device/IP clusters with alternating win patterns | flag, no auto-ban (many false positives e.g. families) |
| **Packet integrity** | AEAD auth failure, replay window violation, malformed protocol | auto-drop + rate-limit source; sustained abuse → temporary IP throttle |
| **Vehicle envelope** | speed/air-time outside `03_VEHICLES` envelopes | auto-correct + count |
| **Client tamper** | asset/assembly hash mismatch, attestation failure pattern | flag, request step-up attestation |
Each detector emits `{account_id, match_id, detector, severity 1-5, evidence_ref, score}` to `anticheat_flags`; scores decay over time unless reinforced.

## 3. Human review workflow
Queue sorted by severity × confidence → reviewer opens the Anti-cheat console (event timeline, 2D replay from `.nrec`, cohort comparison) → outcomes: dismiss (feeds detector tuning as a negative label), warn, mute, temp ban, permanent ban, or **shadow review** (extra scrutiny, no visible action) for ambiguous cases. Two-reviewer agreement required for permanent bans below a very high automated-confidence threshold.

## 4. Automated high-confidence actions (no human needed)
Only for: invalid packet auth beyond tolerance, movement teleport far beyond any legitimate lag-compensation window, confirmed duplicate/forged settlement signature, known-malware-signature memory patterns from the anti-tamper check. These produce an immediate **temporary** restriction (not permanent) pending human confirmation within 24 h, to bound false-positive harm.

## 5. Penalty ladder
| Offense | First | Repeat |
|---|---|---|
| Toxicity/chat | warning → 24 h mute | 7 d → 30 d → perma-mute |
| Team-killing/griefing | Scrip fine + matchmaking penalty | temp ban |
| Boosting/multi-accounting | warning + rank reset | temp ban |
| Confirmed cheat software | **permanent ban**, device + payment-instrument flags where legally permitted | — |
| Economy exploit (player-caused) | rollback + warning | temp/perm ban if repeated/malicious |
Hardware/device bans (not just account) are used only for confirmed cheat-software cases and only where the platform and law allow it; document the legal basis [VERIFY].

## 6. Appeals
In-app/website appeal form → ticket with evidence bundle shown to a different reviewer than the original action → SLA 72 h → outcome communicated with a reason category (not the full detector internals, to avoid teaching evasion) → repeat offenders after a successful appeal are watch-flagged rather than fully cleared from history.

## 7. Ban wave hygiene
Batch and delay reveals of pattern-based detections (don't act instantly on every case) to avoid teaching cheat developers the exact detection boundary; vary timing; keep detector internals out of public patch notes.

## 8. Cheat-selling site response
Do not visit or link cheat marketplaces in search (`harmful_content_safety` applies); handle via legal (DMCA/cease-and-desist) and by rotating anti-tamper signatures; track via community reports instead of researching the sites directly.

## 9. Metrics
Flags per 1,000 matches by detector, false-positive rate from appeal outcomes, time-to-action, repeat-offender rate, community-reported vs detector-found ratio, ban-wave impact on aim-metric population distributions.

## 10. Acceptance
Every detector has a documented false-positive mitigation · no permanent ban path skips either two-reviewer agreement or the bounded automated-with-24h-confirmation rule · appeals SLA met in a dry run · public documentation never reveals exact thresholds.
