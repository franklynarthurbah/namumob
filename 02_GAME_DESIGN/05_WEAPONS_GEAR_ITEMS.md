# Weapons, ammo, attachments, armor, medical, gadgets, valuables
> Read after: 03_MOVEMENT_COMBAT · Used in: Phase 2 (first 4 weapons), Phase 6 (full roster), Blender weapon pipeline (05/03)
> All names/designs are original; real firearm *types* only. Values in Scrip (trader base value).

## 1. Rarity and tiers
Common ×1 · Uncommon ×1.6 · Rare ×3 · Epic ×6 · Legendary ×12 (multipliers apply to valuables/loot drops). Weapon tiers: **T0** starter · T1 common · T2 uncommon/rare · T3 rare/epic · T4 epic/legendary · T5 boss/cache only.

## 2. Weapons (dmg = body, unarmored, at fall_start)
| ID | Class | Ammo | Dmg | RPM | Mag | Reload s | Eff. range m | Recoil 1–10 | kg | Value | Tier |
|---|---|---|---|---|---|---|---|---|---|---|---|
| wpn_sparrow_p9 | Pistol | 9mm | 22 | 400 semi | 15 | 1.6 | 40 | 2 | 0.9 | 1,200 | T0 |
| wpn_kingfisher_p45 | Pistol | .45 | 30 | 330 semi | 10 | 1.8 | 40 | 3 | 1.1 | 2,400 | T1 |
| wpn_cobra_r357 | Revolver | .357 | 52 | 150 | 6 | 3.2 | 60 | 3 | 1.3 | 9,000 | T2 |
| wpn_mamba_mp9 | SMG | 9mm | 19 | 900 | 32 | 1.9 | 45 | 3 | 2.4 | 5,500 | T1 |
| wpn_hornet_uz45 | SMG | .45 | 25 | 700 | 25 | 2.0 | 45 | 4 | 2.9 | 6,500 | T1 |
| wpn_gecko_pdw5 | PDW | 9mm AP | 18 | 800 | 40 | 2.1 | 55 | 3 | 2.2 | 11,000 | T3 |
| wpn_impala_m5 | AR | 5.56 | 26 | 750 | 30 | 2.3 | 80 | 5 | 3.6 | 9,000 | T2 |
| wpn_sable_sv9 | AR | 5.56 | 24 | 850 | 30 | 2.2 | 75 | 4 | 3.4 | 12,000 | T2 |
| wpn_kudu_kr7 | AR | 7.62x39 | 34 | 600 | 30 | 2.5 | 85 | 7 | 4.0 | 14,000 | T2 |
| wpn_elephant_lm7 | LMG | 7.62x39 | 32 | 650 | 75 | 6.2 | 100 | 8 | 8.5 | 42,000 | T4 |
| wpn_springbok_am6 | DMR | 5.56 | 40 | 320 semi | 20 | 2.5 | 130 | 5 | 4.0 | 18,000 | T3 |
| wpn_oryx_sr14 | DMR | 7.62x51 | 58 | 240 semi | 15 | 2.8 | 160 | 8 | 4.8 | 26,000 | T3 |
| wpn_egret_bs308 | Bolt sniper | 7.62x51 | 85 | 55 | 5 | 3.0 | 250 | 8 | 5.2 | 38,000 | T3 |
| wpn_baobab_br338 | Bolt sniper | .338 | 110 | 45 | 5 | 3.4 | 300 | 10 | 6.4 | 65,000 | T4 |
| wpn_warthog_ps12 | Pump shotgun | 12g | 8×11 | 70 | 6 | 0.5/shell | 12 | 9 | 3.6 | 8,000 | T1 |
| wpn_buffalo_as12 | Auto shotgun | 12g | 8×8 | 260 | 8 | 2.6 | 15 | 8 | 4.6 | 24,000 | T3 |
| wpn_thunder_rl40 | Launcher | 40mm | 300 splash | 1 | 1 | 4.5 | 150 | 6 | 6.0 | 120,000 | T5 |
Melee: `wpn_knife_utility` 35 (backstab ×2) · `wpn_axe_fire` 60 slow · `wpn_crowbar` 45. Semi/auto/burst selectors where noted. Launch build needs T0–T3 rows; T4/T5 arrive with bosses (Phase 6).

## 3. Ammo
| ID | Pen | Dmg mod | Note |
|---|---|---|---|
| amo_9mm | 1 | ×1.0 | |
| amo_9mm_ap | 2 | ×0.95 | PDW only |
| amo_45 | 1 | ×1.0 | |
| amo_357 | 3 | ×1.0 | |
| amo_556 | 3 | ×1.0 | |
| amo_556_ap | 4 | ×0.92 | rare |
| amo_762x39 | 4 | ×1.0 | |
| amo_762x51 | 5 | ×1.0 | |
| amo_338 | 6 | ×1.0 | |
| amo_12g_buck | 1 | pellets | |
| amo_12g_slug | 2 | 90 single | |
| amo_40mm | — | splash 300, r 4 m | |
Stack sizes: pistol/SMG 60, rifle 60, DMR/sniper 30, shotgun 20, 40mm 2.

## 4. Attachments (slots: muzzle, optic, underbarrel, magazine, stock, laser/light)
Suppressor: audible radius ×0.3, −5% range, hides muzzle flash · Brake −12% vertical recoil · Compensator −10% horizontal · Flash hider: muzzle flash invisible beyond 30 m · Optics: red dot/holo 1×, 2×, 4×, 6×, 8× (ADS +30…120 ms with magnification) · Vertical grip −8% recoil · Angled grip +12% ADS speed · Bipod (LMG/sniper) −40% recoil when prone/braced · Extended mag +50% capacity/+10% reload · Quick mag −20% reload · Stable stock −10% sway · Light stock +8% ADS speed · Laser −15% hip spread (enemies can see the dot) · Flashlight (visible, AI notices). Each attachment: weight, value, tags for compatibility. Max 4 attachments per weapon at launch.

## 5. Armor, helmets, rigs, backpacks
| Item | Class | DR | Durability | kg | Value |
|---|---|---|---|---|---|
| Vest AC1–AC5 | 1–5 | 25/35/45/55/65% | 40/60/80/100/120 | 1.5/3/5/7/9 | 2k/6k/15k/40k/90k |
| Helmet HC1–HC4 | 1–4 | head mult 1.8/1.6/1.45/1.3 | 30/45/60/80 | 0.6/0.9/1.3/1.8 | 1.5k/5k/14k/38k |
| Backpack T1–T4 | slots | 12/18/24/30 | — | 0.5/0.8/1.2/1.6 | 1k/4k/12k/30k |
Chest rig: +4/+6/+8 quick slots. Repairs cost 12% of item value at Wrench (per full durability).

## 6. Medical
Bandage (stops bleed, 2 s) · Medkit (+60 HP over 6 s, 5 s use) · Painkiller (suppresses tremor/sway penalties 90 s) · Splint (fixes fracture, 4 s) · Surgical kit (+100 HP, clears bleed/fracture, 8 s, rare) · Adrenal shot (+12% speed, +1 HP/s for 60 s, then 20 s slow) · Revives are free.

## 7. Throwables and gadgets
Frag (130 dmg ≤3 m, falls to 0 at 8 m, fuse 3.5 s) · Smoke (12 s) · Flash (3 s blind, 5 s bodycam whiteout) · **Signal Jammer** (25 m, 20 s: enemy bodycam glitch, disables enemy drones and ping) · **Decoy Beacon** (30 s fake footsteps/gunfire audio) · **Signal Flare Gun** (single use extraction) · **Bike Crate token** (drops a bike at your feet) · Keycards/keys (`key_*`, open base doors and vaults) · post-launch **Recon Drone**.

## 8. Valuables (sell to Broker)
Copper spool 300 · Circuit board 1,200 · Solar cell 2,500 · Medical scanner 8,000 · Vault ledger 25,000 · Gold bar 40,000 · **Halo Core** 60,000–250,000 (boss/cache only).

## 9. Data format
Items are JSON in `Client/Assets/Data/Items/*.json`, mirrored to the server; a content hash is checked in the connection handshake. Example:
```json
{ "id":"wpn_kudu_kr7","class":"ar","ammo":"amo_762x39","damage":34,"rpm":600,"mag":30,"reload_s":2.5,
  "fall_start_m":85,"recoil":7,"weight_kg":4.0,"value":14000,"tier":2,"slots":["muzzle","optic","underbarrel","magazine","stock","laser"],
  "recoil_pattern":"rp_kudu_v1","prefab":"Weapons/wpn_kudu_kr7","hitscan":true }
```
Validation script must reject unknown fields, missing ids and out-of-range values in CI.
