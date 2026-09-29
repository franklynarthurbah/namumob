# Monetization design and catalog (purchasables)
> Read after: 02_GAME_DESIGN/06, 05_CHARACTERS_ART_AUDIO/02 · Used in: Phase 8 · Prices are starting points; use store price tiers and regional pricing [VERIFY]

## 1. Principles
Cosmetics and style only · **no pay-for-power, no pay-to-progress, no paid randomness** · transparent prices · no ads · no fake scarcity or fake countdowns · minors protected · currency is account-bound and non-tradeable.
**Forbidden for purchase (hard rule, enforced by an admin validator):** weapons, ammo, armor, medical items, gadgets, Scrip, materials, stash/Safe Case upgrades, XP or Rep boosters, timer skips, matchmaking advantages, insurance speed-ups.

## 2. Real-money products (store SKUs)
| Product id | Type | Grants | Price (USD tier) | Cred per $ |
|---|---|---|---|---|
| `cred_60` | consumable | 60 Cred | 0.49 (if the store allows) | 122 |
| `cred_120` | consumable | 120 | 0.99 | 121 |
| `cred_650` | consumable | 650 | 4.99 | 130 |
| `cred_1400` | consumable | 1,400 | 9.99 | 140 |
| `cred_3000` | consumable | 3,000 | 19.99 | 150 |
| `cred_7800` | consumable | 7,800 | 49.99 | 156 |
| `cred_16500` | consumable | 16,500 | 99.99 | 165 |
| `starter_runner_bundle` | non-consumable, one-time | outfit + bike skin + weapon wrap + HUD theme + 300 Cred | 4.99 | — |
| `pass_sN_premium` | consumable (season-scoped entitlement) | Feed Pass premium track for season N | 9.99 | — |
| `pass_sN_premium_plus` | consumable | premium + 20 tier skips (no power) | 19.99 | — |
Local currency prices always come from the store product metadata. Consider lower-priced packs for markets with lower purchasing power and carrier-billing options where the store supports them [VERIFY].

## 3. Cosmetic price ladder (Cred)
| Rarity | Typical price | Examples |
|---|---|---|
| Standard | free (levels/contracts) | recolours, basic outfits |
| Refined | 300 | outfit pieces, bike decals, weapon wraps |
| Prime | 800 | full outfits, animated wraps, HUD themes |
| Mythic | 1,800 | signature sets with unique animations/VFX (cosmetic only) |
Other: emotes 150–300 · bodycam HUD themes 400 · bike skins 500–1,500 · weapon skins 600–1,800 · outfit sets 1,200–2,400 · parachute trails 400 · Safehouse themes 600 · nameplates/banners 100–300 · name change 200 (first free). Bundles discount 15–25%.
**Kestrel bodycam frames and OSD skins** (layout/colour/typography of REC/timestamp overlay) are a signature product line; they must never reduce readability (validator: contrast + safe-area checks).

## 4. Feed Pass (season, 12 weeks)
60 tiers, free + premium tracks, tier XP 1,000 (rising slightly) earned by playing; premium purchase unlocks retroactively. Free track: Scrip, materials, a few cosmetics, 300 Cred total. Premium: cosmetics, HUD skins, emotes, and a **Cred rebate of ~900 over the season** (players can fund the next pass by playing). Milestones at 10/20/30/40/50/60 with exclusive recolours (no power). Weekly catch-up XP bonus for late starters.

## 5. Rotating store
Daily/weekly featured rows; event cosmetics during events; a **Vault** where past items return on a schedule. Labels: "returns: yes/unknown". Only real timers.

## 6. Player protection
Purchase confirmation modal with price and currency · default monthly spend caps by age bracket for minors (configure by region with counsel [VERIFY]) · rely on Google Family Link and Apple Ask to Buy/parental controls · no gifting at launch · clear refund/support links · receipts by store.

## 7. Fraud and abuse
Fake receipts and replay (unique transaction ids, account binding) · refund abuse and chargebacks (debt tracking, flags) · grey-market currency sellers (account-bound currency, anomaly detection on gift-like patterns) · promotional-code abuse.

## 8. Catalog data (server truth)
```json
{ "sku":"cos_out_dustrunner_prime","type":"cosmetic","slot":"outfit","rarity":"prime","price_cred":800,
  "active_from":"2026-11-01T00:00:00Z","active_to":null,"tags":["bike","dust"],"bundle_of":[],"version":3 }
{ "sku":"cred_650","type":"currency","store_product_id":{"google":"cred_650","apple":"cred_650"},"grants":{"cred":650} }
```
Validator rules: no forbidden category, no random reward fields, price ladder ranges respected, localization keys present.

## 9. Metrics and experiments
Conversion, ARPDAU, first-purchase time, pass attach rate, refund rate, Cred sink/source balance. Price tests only with guardrails (D1, complaints). No dark patterns: no forced purchases, no misleading close buttons, no interruptive full-screen offers during raids.

## 10. Acceptance
No SKU can be created that violates §1 · every SKU has preview, price, rarity, localization · Feed Pass rebate math verified by test · admin store tools log every change.
