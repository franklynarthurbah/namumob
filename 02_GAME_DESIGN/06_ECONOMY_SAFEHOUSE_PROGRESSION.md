# Economy, traders, Safehouse, progression, Aptitudes
> Read after: 05_WEAPONS_GEAR · Used in: Phase 5 (meta), Phase 8 (Feed Pass), backend ledger design (08/02)

## 1. Currencies
| Currency | Source | Sinks | Rule |
|---|---|---|---|
| **Scrip** | selling loot, contracts, extraction bonuses | buying gear, repairs, insurance, Safehouse upgrades, crafting | earned only |
| **Feed Credits (Cred)** | purchase; small amounts from Feed Pass free track | cosmetics, Feed Pass, name change | **never buys power** |
| **Rep** (per trader, L1–L4) | selling/buying, contracts | unlocks stock and better sell rates | earned only |
| **Feed Rep** | tricks (Hype), Reels, streaks | cosmetic tiers, titles | earned only |
| **Materials** (scrap, circuits, fabric, chemicals) | loot | crafting/upgrades | tradeable only via Broker |

## 2. Value and sinks
Sell price = base × sell factor (Rep L1 0.55 · L2 0.60 · L3 0.65 · L4 0.70). Buy price = base × 1.0 (T2+ items require Rep). Sinks: repairs 12% per full durability, insurance 8%, craft costs, Safehouse upgrades. Target: median Standard-bracket run profit +12k Scrip, extraction rate 55%, High Stakes p90 >400k. Tune with backend config, not code.

## 3. Traders
| Trader | Sells | Buys |
|---|---|---|
| **Fixer** | weapons, ammo, attachments | weapons, attachments |
| **Doc** | medical, armor repair kits | medical, valuables (medical) |
| **Wrench** | armor, helmets, backpacks, bikes/vehicle parts, repairs | armor, bike parts |
| **Broker** | keycards, flares, bike crates | everything else + valuables; posts Contracts |
Stock refreshes every 6 h (server-side seed). Rep L2–L4 thresholds: 5k / 25k / 90k trade volume.

## 4. Safehouse (hub)
| Module | Function | Levels |
|---|---|---|
| **Stash** | storage slots | L1 60 · L2 80 (5k) · L3 100 (12k) · L4 130 (30k) · L5 160 (70k) · L6 190 (150k) · L7 220 (300k) · L8 250 (600k) · L9 280 (1.1M) · L10 300 (2M Scrip) |
| **Safe Case** | protected slots | 2 → 6 (levels 2/5/8/10 add one each) |
| **Medbay** | free healing/repairs of bandages; heals between runs | L1–L5 |
| **Workshop** | craft ammo, meds, attachments; weapon upgrades | L1–L8 |
| **Garage** | bike/vehicle storage and tuning | L1–L6 |
| **Radio Room** | Contracts slots (3 → 6) | L1–L5 |
| **Armory** | 3–10 loadout presets | L1–L5 |
| **Feed Studio** | Reels editor, share | L1–L3 |
| **Trophy Wall** | cosmetic display (themes purchasable) | — |
Upgrades also consume materials (scaling with level). Timers are short (≤30 min) and **cannot be skipped with Cred**.

## 5. Contracts
Daily (3), weekly (3), season (10). Types: extract with item X, kill N Concession Guards, land a trick chain of Hype ≥Y, ride N km, clear a base. Rewards: Scrip, Rep, materials, Feed Rep, occasional cosmetic (never power).

## 6. Operator level and Aptitudes
XP per level: `xp(L) = 300 × L^1.35` (cumulative to level 100). Sources: extraction, kills, damage, revives, loot value, contracts, Hype.
**Aptitudes:** 6 branches × 5 nodes × up to 3 ranks; 1 point per level (60 by level 100; 2 extra from milestones); each rank +1.5–2% to one stat, capped at +10% total per stat:
Endurance (stamina, weight limit) · Handling (ADS speed, reload) · Field Medic (heal speed, revive speed) · Ghost (footstep radius, prone speed) · Rider (boost recharge, landing damage) · Salvager (loot speed, container rare chance +≤5%). Respec costs Scrip.

## 7. Unlock cadence
Level 1 Fixer basics · 5 Doc · 8 bikes (Garage) · 10 Broker contracts · 15 Standard bracket · 20 Wrench T2 · 30 High Stakes · 40 Underline map · 50 Halo Core trade.

## 8. Seasons
12 weeks. Feed Pass and leaderboards reset; Scrip, gear, Rep and Aptitudes persist. Season theme adds contracts, cosmetics and a small map change (props only at first).

## 9. Health metrics (dashboards)
Scrip created/destroyed per day · median profit by bracket · extraction rate by bracket/map · gear value distribution · % of players at zero stash · time-to-first-extraction · churn after first loss. Admin panel has knobs for sell factors, drop tables, fees (versioned, staged).
