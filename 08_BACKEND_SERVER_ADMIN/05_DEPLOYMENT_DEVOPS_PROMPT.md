# Deployment and DevOps prompt (self-hosted)
> Read after: 03, 04 · Used in: Phase 4 (staging), Phase 10 (production) · Prices/regions/providers are [VERIFY]

## 1. Repository `/Infra`
```
compose/{dev,staging,prod}.yml   caddy/Caddyfile   monitoring/{prometheus.yml,alerts.yml,grafana/,loki/}
ansible/ (host bootstrap, hardening)   terraform/ (optional)   k8s/ (Tier 2: agones, api, workers)
scripts/{deploy.sh,rollback.sh,backup.sh,restore.sh,rotate-keys.sh}   runbooks/
```
## 2. Tier 1 topology
- **Control host(s):** Caddy (TLS), API, workers, admin SPA (static), PostgreSQL, Redis, Prometheus, Grafana, Loki, Alertmanager, exporters. Start on one strong VPS; move Postgres to a second host or a managed service when load grows.
- **Game hosts (per region):** Docker + host agent + `namulinda-server` images. Separate providers/regions allowed. Public **UDP 20000–20999** (+20999 probe); agent API only on the WireGuard mesh to the control host.
- **Domains:** `api.<domain>` (API), `admin.<domain>` (admin, IP-restricted), `cdn.<domain>` (Addressables + assets from S3-compatible storage behind a CDN), `status.<domain>`.
## 3. Compose services and rules
Health checks on every service · resource limits · `restart: unless-stopped` · non-root users, `read_only: true` where possible, `cap_drop: [ALL]`, `no-new-privileges` · Postgres/Redis bound to the private interface only · secrets via Docker secrets or SOPS-encrypted env, never in git · separate networks (public, app, data, monitoring).
## 4. Storage and CDN
S3-compatible bucket(s): `bundles` (public via CDN, immutable hashed paths), `replays` (private, signed URLs), `telemetry` (private), `backups` (private, object-lock if available). Cache headers: bundles immutable 1 y; catalogs short TTL.
## 5. Backups and recovery
PostgreSQL continuous WAL archiving (pgBackRest or WAL-G) + daily base backup to object storage; retention 14 daily / 8 weekly; **monthly restore drills** recorded in `docs/runbooks/restore-drill-YYYYMM.md`. Redis treated as ephemeral. **RPO 5 min, RTO 2 h.** Config and infra are code in git.
## 6. Observability and SLOs
Prometheus scrape of API, workers, fleet, game servers, hosts; Loki for logs; Grafana dashboards (API, matchmaking, fleet, server tick, economy, security). SLOs: API availability 99.9%, matchmaking p95 <45 s at peak, allocation success ≥99%, server tick p99 ≤25 ms, crash-free ≥99.5%. Alerts (Alertmanager → email/Discord webhook): API 5xx, DB replication/backup failure, disk >80%, allocation failures, server tick p99, ledger reconciliation mismatch, spike in bans/flags, certificate expiry, unusual admin actions.
## 7. Hardening baseline
Unattended security updates · SSH keys only, no root login, fail2ban · nftables/ufw default deny · WireGuard between hosts · Cloudflare (or equivalent) in front of API/admin with WAF + rate limits; provider UDP DDoS protection for game hosts · image scanning (Trivy) + SBOM · dependency updates (Renovate/Dependabot) · secret scanning (gitleaks) · rotate JWT signing keys with `kid` (script + runbook) · least-privilege DB roles (app, migrator, readonly analyst) · separate admin plane · time sync (chrony).
## 8. CI/CD
Build images → push to registry (GHCR or self-hosted) → `deploy.sh` does `docker compose pull && up -d` with health gates. **Blue/green for API** (two stacks, Caddy switches upstream). **Migrations** run as a separate job after an automatic backup snapshot, must be reversible or expand/contract. `rollback.sh` restores previous images and flips Caddy. Staging mirrors production shape; a load-test environment can be spun up on demand.
## 9. Tier 2 (Kubernetes + Agones)
Trigger: sustained demand beyond manual host management (rule of thumb: >2,000 concurrent or frequent scaling). Managed Postgres, node pools (services vs game), cluster autoscaler, Agones fleets per region, GitOps (Argo CD/Flux) optional. Same images and config.
## 10. Provider and region selection
Measure real latency from target countries first. Prefer providers with UDP DDoS mitigation and nearby locations (check African availability such as Johannesburg/Cape Town/Lagos/Nairobi options and European fallbacks) and predictable egress pricing [VERIFY]. Start with 1–2 regions, add by data.
## 11. Env/secrets inventory
`DB_CONNECTION`, `REDIS_URL`, `JWT_SIGNING_KEY(S)`, `ADMIN_JWT_KEY`, `SERVER_SIGNING_PUBKEYS`, `GOOGLE_PLAY_SERVICE_ACCOUNT`, `APPLE_IAP_KEY(id, issuer, p8)`, `PLAY_INTEGRITY_CONFIG`, `APPLE_APP_ATTEST_TEAM_ID`, `S3_*`, `SMTP_*`, `FCM_*`, `APNS_*`, `SENTRY_DSN`. All rotated on a schedule; access via a secrets manager or SOPS; never printed.
## 12. Disaster runbooks (write in `/Infra/runbooks/`)
DB failover/restore · region loss (redirect matchmaking) · compromised secret (rotate + revoke sessions) · mass cheating (ban wave + config hardening) · rollback release · payment provider outage · DDoS response.
## 13. Acceptance
Fresh servers reach a working staging with one Ansible + one deploy command · restore drill succeeds · blue/green switch without dropped requests · alerts fire in a simulated failure · security baseline scan has zero criticals.
