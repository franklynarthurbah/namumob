# Infrastructure and data security
> Read after: 08_BACKEND_SERVER_ADMIN/05, 01_THREAT_MODEL · Used in: Phase 9, ongoing · Consolidates security rules referenced elsewhere so this folder is a complete checklist

## 1. Network
Public surfaces: API (TLS 1.2+/1.3 only, modern ciphers), admin (TLS + IP allowlist option), game-server UDP ports (behind provider DDoS protection) [VERIFY]. Internal: WireGuard mesh for control-plane traffic; Postgres/Redis never internet-reachable; nftables/ufw default-deny with explicit allow rules; fail2ban on SSH; no password SSH auth.

## 2. Secrets management
Single source of truth (SOPS-encrypted files in git, or a secrets manager) · env injection at deploy time, never baked into images · rotation schedule for JWT signing keys (`kid`-based rotation, overlap window), DB credentials, API keys, provider credentials · break-glass procedure documented and rehearsed · secret scanning (gitleaks) in CI blocks merges.

## 3. Data protection
Encryption in transit everywhere (TLS/mTLS/WireGuard) · encryption at rest via disk-level encryption on hosts and encrypted storage buckets · sensitive columns (email) hashed/encrypted at the application layer where feasible · backups encrypted · least-privilege DB roles (application role cannot `DROP`/`ALTER`; separate migration role) · production data never copied to developer laptops; anonymized/synthetic data for local dev.

## 4. Application security
OWASP ASVS-informed review of the API · parameterized queries only (EF Core defaults; no raw string SQL) · strict input validation and size limits on every endpoint · output encoding in the admin SPA (React default escaping; no `dangerouslySetInnerHTML` on untrusted data) · CSRF defenses on any cookie-based admin session · dependency scanning (Dependabot/Renovate + `dotnet list package --vulnerable`, `npm audit`) · container image scanning (Trivy) in CI · SAST on PRs for the API and admin code.

## 5. Access control
Principle of least privilege for every service account and human account · admin RBAC per `08/04 §5` · production access requires MFA and is logged · no shared credentials · quarterly access review (remove stale accounts/keys).

## 6. Incident response
`docs/security/INCIDENT_RESPONSE.md`: roles, severity levels, communication templates (players, stores, regulators where required), timelines for breach notification per applicable law [VERIFY jurisdiction requirements], forensic log retention (audit log hash-chain, access logs) sufficient to reconstruct an incident, post-incident review template.

## 7. Compliance touchpoints
Privacy policy accurately lists all data collected (cross-check against telemetry taxonomy and anti-cheat signals) · data subject rights (export, delete) implemented and tested (`account DELETE` flow) · age-appropriate design considerations for any under-18 accounts · store data-safety forms match reality · vendor/subprocessor list maintained.

## 8. Physical/provider considerations
Choose hosting providers with reasonable physical security and compliance certifications for the regions you operate in [VERIFY]; understand each provider's shared-responsibility model; keep an inventory of every provider and what data/keys they can access.

## 9. Ongoing hygiene
Quarterly: dependency and image audit, access review, secret rotation check, restore-drill, threat-model re-read against any new feature, residual-risk document update (`01_THREAT_MODEL §7`). Annually: a broader external review or penetration test if budget allows.

## 10. Acceptance
No secret ever appears in git history (scanned) · a simulated credential leak can be fully rotated within the runbook's stated time · data-subject delete request removes/anonymizes data within the policy's stated window and is verified by test · quarterly checklist has an owner and a due date.
