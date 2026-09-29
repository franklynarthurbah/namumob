# Dedicated game server, matchmaking, fleet
> Read after: 01, 02, 07/02 · Used in: Phase 2 (server), Phase 4 (fleet + matchmaker), Phase 10 (scale) · Unity Multiplay is not used (service ended 2026-03-31)

## 1. Game server process
**Lifecycle:** start with args `--port --matchId --region --map --mode --seed --backendUrl --credentialFile` → load content (verify content hash) and server cells → register with backend (`/internal/servers/register`) → `Ready` → accept connections (tickets) → run match → send events/heartbeat every 5 s → on end send settlements → flush recorder → exit code 0. SIGTERM: finish current phase, settle, exit.
**Systems on the server:** NamuNet, players (sim), AI, loot (seeded containers), vehicles, extraction, Dimming, Signal Caches, elite waves, bosses, match state machine, PVS relevancy, match recorder (`.nrec`: compressed inputs + key events, ~1–3 MB/match; 7-day retention, 30 for flagged), Prometheus `/metrics`, health endpoint.
**Crash policy:** crash mid-raid → backend refunds manifest after the reconciler timeout; players see "Match cancelled, gear returned".
**Limits:** ≈2 vCPU and 2.5 GB per match (48 players); watchdog kills stuck processes; no outbound network except backend and metrics.

## 2. Matchmaking
Queues per `(mode, map, region, bracket, squadSize)`. Ticket = party, members, Gear Value, Threat Rating, region pings, created_at, backfill preference.
- **Regions:** client measures UDP ping to each region's probe (port 20999) at lobby load; ticket lists best 2 regions; match in the best common region unless queue time exceeds 40 s.
- **Formation loop (every 2 s):** collect compatible tickets → target 12 squads of 4 (Map A). **Start rules:** full lobby immediately; else after 45 s with ≥4 squads; after 90 s with ≥2 squads; missing density filled with **AI squads** so early/low-population hours still feel like a raid (population multiplier stored in config).
- **Bracket anti-sandbagging:** `effective_bracket = max(bracket(gearValue), bracket(threatRating))`.
- **Squad fill:** solo/duo tickets can opt into fill; no cross-region fill.
- **Allocation:** matchmaker calls fleet → server slot → backend locks gear (raid enter), mints MatchTickets with per-player session keys → delivered via SignalR/polling → client connects.
- **Cancel/timeout:** tickets expire after 5 min; cancelling never locks gear.

## 3. Fleet manager (Tier 1, self-hosted)
**Control plane** (in `Namulinda.Fleet` within the API deployment): host registry with heartbeats, scheduler (least-loaded per region), version rollout (canary 5% of matches), drain/cordon, warm pools per region.
**Host agent** (`Namulinda.Fleet.Agent`) on each game host: talks to the local Docker Engine API, allocates UDP ports from `20000–20999`, launches `namulinda-server:<build>` with CPU/memory limits and a per-launch credential file (2 h JWT), waits for `Ready` (30 s), reports capacity, kills orphans, exposes metrics. Private control channel over WireGuard.
**Capacity model:** `matches_per_host = floor(vCPU / 2 × 0.85)`; hosts needed = `ceil(CCU / players_per_match × 2 vCPU × 1.3 / host_vCPU)` (tune with measured tick cost).
**Rolling update:** cordon host → wait up to 30 min for matches to finish → update → uncordon; canary first.
**Warm pool:** N idle servers per region (config) to keep allocation ≤1.5 s; cold allocation ≤5 s p95.

## 4. Tier 2 — Kubernetes + Agones (when measured demand needs autoscaling)
Same server image. `Fleet` + `FleetAutoscaler` per region, Agones allocator service replaces the host-agent scheduler, Agones SDK sidecar handles Ready/Health/Shutdown. Separate node pools for game servers vs services; managed Postgres. Mapping: allocation request → `GameServerAllocation`; heartbeats → SDK health; drain → cordon nodes.

## 5. Regions and latency
Choose regions by measured player latency, not guesses: probe from target countries, then pick providers with UDP DDoS mitigation and nearby locations [VERIFY availability and prices]. Start with 1–2 regions; keep one global control plane and DB primary; add read replicas only when needed. Player-facing data (profile, inventory) is region-independent.

## 6. Match recorder and evidence
`.nrec` files land in object storage with metadata (match, players, flags). The admin panel links flagged matches to an event timeline (2D map, shots, hits, movement) and offers download for an editor replay tool.

## 7. Acceptance
Warm allocation p95 ≤1.5 s, cold ≤5 s · no orphan processes after 1,000 match cycles · chaos tests: kill server mid-raid → automatic refund; kill agent → running matches continue; network partition control↔host → matches finish and settle later with retry · start rules produce viable matches at 4, 8 and 12 squads · AI backfill keeps median encounter density within ±20% of full lobbies.
