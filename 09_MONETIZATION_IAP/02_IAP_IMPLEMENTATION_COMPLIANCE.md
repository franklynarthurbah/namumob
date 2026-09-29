# IAP implementation and store compliance
> Read after: 01_MONETIZATION_DESIGN_CATALOG, 08/02 · Used in: Phase 8, Phase 10 · [VERIFY] API names and policy text on the day you implement/submit

## 1. Client (Unity IAP 5.x, `StoreController` API)
Flow: `Connect()` → `FetchProducts` (ids from the server catalog) → `FetchPurchases` (recover pending) → `PurchaseProduct` → on **pending order** send the store payload to `POST /v1/iap/validate` → when the server answers `granted`, call **`ConfirmPurchase(pendingOrder)`** (only then does the store finish/consume). Failure paths: `OnPurchaseFailed` (show reason), deferred/pending payments (show "pending", grant later), app killed after payment (recover on next `FetchPurchases`), offline (keep pending, retry with backoff). Bind purchases to the account: Google `obfuscatedAccountId` = HMAC(accountId); Apple `appAccountToken` = UUID derived from accountId. Show price from product metadata. **Restore Purchases** button (iOS requirement; harmless on Android). Interfaces `IStoreService` and `IPurchaseValidator` with an in-editor fake store for tests. **Never grant on the client.** Compile-check exact v5 API names.

## 2. Backend validation
- **Google Play:** service account with Play Developer API access; fetch purchase state (`purchases.products.get`/v2 equivalent), check purchased state, `orderId`, obfuscated account id matches, package name, product id; acknowledge/consume if not already done (unacknowledged purchases are refunded automatically after ~3 days); poll **voided purchases** (or use RTDN via Pub/Sub) for refunds/chargebacks.
- **Apple:** verify StoreKit 2 **signed transaction** (JWS with x5c chain to Apple roots) or call the App Store Server API; check bundle id, product id, environment, `appAccountToken`; handle **App Store Server Notifications V2** at `POST /webhooks/apple` (verify signature; handle REFUND, REVOKE, CONSUMPTION_REQUEST); test in Sandbox/TestFlight and with Xcode StoreKit testing.
- **Idempotency:** insert `purchases` row keyed by `transaction_id` (unique) → grant in one DB transaction (ledger entry + entitlement) → mark `granted`. Replays return the same result.
- **Refund/revoke:** claw back available Cred; if already spent, record `cred_debt` on the profile (future purchases repay first); direct-purchase bundles revoke entitlements; repeated abuse raises flags. Never allow negative ledger balances.
- **Fraud controls:** attestation required, per-account and per-device rate limits, anomaly flags (new account + high spend, many devices), receipts never logged in full.

## 3. Catalog sync
Server is the truth (`catalog_skus`); client fetches on Lobby load; store product ids mapped per platform; region availability and schedules honoured; prices shown are store prices.

## 4. Compliance checklist
**Google Play:** use Play Billing for digital goods (alternative/external billing programs only if approved for your region); Data Safety form matches telemetry (purchase history, device identifiers, diagnostics); target API 36 and 16 KB page-size; UGC/chat safety (report, block, filter); no simulated gambling; not a children's app; content rating (IARC).
**Apple:** IAP for digital content; **account deletion in-app**; Sign in with Apple parity when offering other third-party sign-in [VERIFY]; UGC rules for chat (filter, report, block, contact); updated age rating questionnaire (13+/16+/18+); App Privacy details and `PrivacyInfo.xcprivacy`; no tracking → no ATT prompt; Restore Purchases; no loot-box odds needed because there are no paid random items.
**Regions/legal (with counsel):** GDPR/UK GDPR (consent, age thresholds, erasure/export), UK Children's Code, US COPPA (not directed to children; neutral age screen), consumer-protection rules for digital content (immediate-delivery consent wording), taxes handled by stores as merchant of record, country-specific game licensing (e.g. China, Korea) excluded from launch unless licensed.

## 5. Test cases (sandbox + automated)
Success · user cancel · pending/deferred · network drop mid-purchase · app kill after payment before grant · duplicate delivery · refund · chargeback · restore on a new device · account switch on same device · store outage · price change · region change · interrupted confirm · tampered receipt · receipt from another app/account.

## 6. Admin tools
Purchase lookup by transaction id, entitlement fix with reason, refund handling view, revenue dashboard from validated purchases only, catalog validator, SKU scheduler.

## 7. Acceptance
Every test case above passes in sandbox on both stores · no path grants currency without server validation · duplicate `transaction_id` grants once · refund revokes correctly · store review checklist items all ticked before submission.
