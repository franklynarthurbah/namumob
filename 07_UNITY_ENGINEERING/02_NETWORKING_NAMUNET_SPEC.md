# NamuNet — networking specification (client, server, protocol)
> Read after: 00_START_HERE/02 (constants), 02_GAME_DESIGN/03 · Used in: Phase 2 (critical), Phases 3, 6, 7, 9 · Security companion: `10_SECURITY_ANTICHEAT/01`

## 1. Goals and constraints
48 players + ≈60 active AI at 30 Hz, RTT up to 300 ms, 3% loss, mobile data budgets (down avg ≤10 KB/s, peak 24, Data Saver ≤6; up avg ≤2.5), server ≤12 ms average tick on ~2 vCPU. Server-authoritative. No dependency on Unity Multiplay/Relay.

## 2. Layers
Transport (Unity Transport UDP `NetworkDriver`) → **PacketCrypto** (app-layer AEAD) → Channels (unreliable-sequenced, reliable-ordered) → Replication (entities, snapshots, events) → Simulation (tick loop, prediction, reconciliation, lag comp, interest).

## 3. Encryption and framing
Primary: **ChaCha20-Poly1305** app-layer AEAD (BouncyCastle managed or libsodium plugin; RFC 8439 test vectors in unit tests) [VERIFY licence/size]. Optional later: DTLS under it.
Packet: `[u8 type|flags][u8 channel][u64 counter][payload][16-byte tag]`; nonce = direction bit + 64-bit counter; sliding replay window 1024; drop and count invalid. Max datagram 1200 bytes; big reliable messages fragmented.
Session key: backend mints a **MatchTicket** (EdDSA-signed JWT, 60 s life, single use `jti`) containing playerId, matchId, serverId, protocol version, content hash, squadId, loadout hash and a per-session key `k` sealed to the game server's public key; the same `k` is returned to the client over TLS in the ticket response. Directional keys = HKDF(k, clientNonce‖serverNonce). Resume token allows reconnect within 90 s.

## 4. Handshake
`ClientHello{proto, contentHash, ticket, clientNonce}` → server verifies signature/expiry/`jti`/serverId/version → `ServerHello{serverNonce, tickRate, serverTick, mapSeed, playerEntityId, config}` (encrypted). Version or content-hash mismatch → `Reject{reason}` and the client shows the update prompt.

## 5. Channels and messages
- **Unreliable sequenced:** `InputBundle` (C→S), `Snapshot` (S→C).
- **Reliable ordered:** events: `Spawn/Despawn`, `InventoryOp/Result`, `Damage/HitConfirm`, `Kill`, `ExtractionState`, `MatchState`, `Ping/Marker`, `Chat`, `SoundEvent` (unreliable variant for distant fire), `Notice`.
```
InputCmd { u16 seq; u32 tick; i8 moveX; i8 moveZ; u16 yawQ; i16 pitchQ; u32 buttons; u8 weaponSlot; u8 aimFlags; u32 renderTick; u8 renderFrac }
InputBundle { InputCmd[≤4] (newest + 3 redundant); u16 ackedSnapshotTick }
Snapshot { u32 serverTick; u16 ackedInputSeq; u32 baselineTick; entity delta records… }
```
Custom `BitWriter/BitReader`, quantization helpers, handwritten or source-generated serializers (no reflection), round-trip tests.
**Quantization:** position = cell index (u8 col,row) + u16 x + u16 z in-cell (≈7.6 mm) + 14-bit y (−10…270 m); yaw 10 bits; pitch 8 bits; velocity buckets; HP exact for self/squad, 3-bit tier for others.
**Delta compression:** per-entity dirty masks vs the client's last-acked baseline (ring of 32 snapshots per client); full state if the baseline is older than 1 s.

## 6. Tick loop and time
Server order per tick: (1) receive/decode/validate → (2) player sim steps from buffered inputs → (3) AI → (4) vehicles → (5) projectiles + hit resolution (sub-tick ordered) → (6) match systems (Dimming, extraction, loot, timers) → (7) snapshot build + send. Input jitter buffer 3 ticks adaptive; missing input repeats the last for ≤5 ticks then idles.
Client clock sync: ping-based, filtered offset with slew; client sim runs ahead by `RTT/2 + jitter + 1 tick`.

## 7. Prediction and reconciliation (local player and driven vehicle)
Store predicted state per tick; on snapshot with `ackedInputSeq`, compare. Error >0.05 m (vehicle 0.15 m) or state mismatch → rewind to authoritative state and replay unacked inputs. Visual smoothing: offset decays with τ≈100 ms; snap if error >1.5 m. Shots: visuals predicted; hit markers on server `HitConfirm`.

## 8. Remote entities
Interpolation buffer 100 ms (adaptive 66–150 ms with jitter estimator); extrapolate ≤150 ms; animation driven from replicated state.

## 9. Lag compensation
Per-entity hitbox history (last 300 ms at tick resolution). Rewind target = client `renderTick+frac`, accepted if `serverTick − renderTick ≤ 8` and consistent with measured RTT ±60 ms; else clamp to 250 ms and count a violation.

## 10. Interest management
Spatial hash 64 m cells → tiers T0–T3 (constants). **PVS gating:** beyond 30 m, players/AI are replicated only if their cell pair is potentially visible (baked PVS, `04_MAPS/00`); loot contents and hidden containers never replicate before 6 m. Sound events carry direction + distance bucket, not positions. Hysteresis (enter R, leave R+10%); relevancy recomputed every 5 ticks. **Bandwidth governor:** per-client byte budget per tick with a priority accumulator (importance × age); T1 players always included.

## 11. Reliability
Events carry sequence ids, acks piggyback; retransmit at RTO = max(2×RTT, 100 ms); handlers are idempotent.

## 12. Test tooling
`NetSim` (latency, jitter, loss, reorder) in-process and `tc netem` scripts for CI · headless **BotClient** (`Namulinda.BotClient`) running scripted behaviours (loot, ride, shoot) for load tests · deterministic replay of recorded input streams.

## 13. Metrics (Prometheus `/metrics` on the server)
Tick ms histogram, per-connection RTT/jitter/loss/bytes, snapshot bytes, reconciliation error, input starvation, rewind clamps, invalid packets, AI ms, entity counts.

## 14. Gate 2 (must pass before Phase 3)
1. 2 phones + PC vs Linux server over ≥150 ms RTT, 3% loss: prediction error p95 <5 cm; no rubber-banding felt in a 10-min test.
2. 48 bots + 60 AI: average tick ≤12 ms, p99 ≤25 ms on the reference 2-vCPU host.
3. Busy-scene bandwidth (12 players in T1): ≤10 KB/s average per client; Data Saver ≤6.
4. Lag-comp test: aiming at rendered target hits ≥95% at 200 ms RTT.
5. Crypto: tampered/replayed packets rejected 100%; RFC vectors pass.
**If targets fail after two optimization iterations, record the decision in `docs/DECISIONS.md` and switch to the Netcode for Entities fallback plan.**
