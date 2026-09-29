# Menus, screens and user flows
> Read after: 02_UI_DESIGN_SYSTEM · Used in: Phase 5 (meta UI), Phase 8 (store), Phase 10 (FTUE polish)

## 1. First-time flow
Splash (animated logo) → **age gate** (13+; region-specific higher ages) + terms/privacy consent → **guest login** → **Tutorial Raid 1** (PvE, 8 min: move, look, shoot, loot, bike, extract) → Safehouse intro (stash, trader) → Tutorial Raids 2–3 (medical/armor, Dimming, ping) → prompt to **link account** (Google Play Games / Apple / email) after first extraction; notification permission asked after first extraction, not at launch.

## 2. Navigation map
```
Splash → (Login/Link) → LOBBY (Safehouse hub, bottom nav)
  ├─ Deploy → Mode/Map/Bracket → Squad → Kit check → Matchmaking → Dropship lobby → RAID → Summary → LOBBY
  ├─ Loadout/Stash → Item detail · Presets · Sell/Trade
  ├─ Traders (Fixer/Doc/Wrench/Broker) · Workshop · Garage · Safehouse upgrades
  ├─ Operator (customization) · Aptitudes
  ├─ Store → Featured · Cosmetics · Bundles · Currency → Purchase confirm
  ├─ Feed Pass · Contracts · Events · Leaderboards
  ├─ Social (friends, party, recent) · Mail/Notifications · Reels
  └─ Settings (Controls, Graphics, Authenticity, Audio, Accessibility, Language, Privacy, Account, Support)
```

## 3. Screen inventory
| Screen | Layout | Primary actions | Data |
|---|---|---|---|
| **Lobby (Safehouse)** | 3D hub with operator on stand (real-time, ≤60k tris), bottom nav, top currency chips, mail/settings, party bar left | Deploy (biggest CTA), open modules | profile, modules, party |
| **Deploy** | left: mode cards; centre: map card with bracket selector; right: squad slots + Gear Value meter | Ready/Queue, change kit | modes.json, gear value |
| **Loadout/Stash** | left: stash grid/list with filters/sort; right: paper-doll slots + weight bar + Gear Value; bottom presets | equip, compare, auto-fill Standard Kit | inventory service |
| **Item detail** | modal with stats, attachments, value, durability, compare delta | equip, sell, repair, modify | items.json |
| **Trader** | portrait + Rep bar; tabs Buy/Sell/Contracts | buy, sell | trader stock (server seed) |
| **Workshop** | recipe list, requirements, timer | craft, upgrade | recipes |
| **Garage** | 3D bike, stats, tricks equipped, skins | equip tricks, skin | bikes.json |
| **Operator** | 3D preview, slot tabs, colour masks | equip/preview | cosmetics |
| **Store** | featured hero card, category tabs, item grid; 3D preview modal | buy with Cred, buy Cred via store | catalog, entitlements |
| **Feed Pass** | horizontal tier track, free/premium rows, claim all | claim, buy pass | pass progress |
| **Contracts** | daily/weekly/season lists with progress | claim, reroll (1/day) | contracts |
| **Summary ("Footage Review")** | profit ledger, XP, Rep, kills, damage, timeline, Reel highlights | continue, share Reel, report | settlement |
| **Settings** | left category list, right panels; controls editor opens HUD editor | apply/reset | prefs |
| **Matchmaking overlay** | REC-dot "Searching…", elapsed, cancel | cancel | ticket status |
| **Dropship lobby** | squad ready check, countdown | ready | match |
| **Report player** | reason list + optional note | submit | reports |

## 4. UX rules
Two taps from Lobby to queue with last kit · persistent last selections · **confirmation** for deploying High Stakes value, selling Epic+, spending Cred, deleting presets · never hide prices · no pressure timers except real event timers · progress persists across app kills · offline: menus readable, deploy disabled with clear reason, tutorial works offline.

## 5. Store UX (important for compliance)
Cosmetics-only badge on every item · price shown from the store SKU (localized) · **Restore Purchases** button (iOS requirement) · refund/support link · purchase confirmation modal shows what/how much · post-purchase "Equip now" · no randomized paid rewards (no odds screens needed) · minors: age-based spend limits, parental prompt · no ads.

## 6. States
Loading skeletons, empty (no items / no contracts), error with retry, maintenance modal (from remote config with ETA), forced-update modal (version gate), network-lost banner with reconnect countdown.

## 7. Implementation notes
One `UIDocument` per layer (HUD, Menus, Modals, Toasts); `ScreenRouter` with back stack + Android back handling (predictive back); ViewModels expose observable properties; screens never call the network directly (services + repositories). See `05_UI_TOOLKIT_IMPLEMENTATION_A11Y_L10N.md`.

## 8. Acceptance
All screens navigable via automated UI test · Deploy in ≤2 taps with a saved kit · every purchase path shows correct price/currency and handles interruption · back button/gesture works on every screen.
