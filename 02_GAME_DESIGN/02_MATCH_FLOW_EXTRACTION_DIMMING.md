# Match flow: deploy, timeline, Dimming, extraction, death, settlement
> Read after: 01_CONCEPT_BIBLE · Used in: Phase 2 (server loop), Phase 3 (slice), Phase 7

## 1. Before the raid
Deploy screen → choose map/mode → choose kit (Standard Kit is free) → **Gear Value** computed → bracket (Rookie <50k · Standard 50k–249,999 · High Stakes ≥250k; matchmaking also uses a rolling **Threat Rating** = recent extracted value) → optional **insurance** (max 2 items, fee 8% of value, item returns 45 min after loss) → queue → **Dropship lobby** (30 s).
Server locks the carried items into an "in-raid" manifest (see backend docs). Server crash refunds the manifest; loot is not kept.

## 2. Timeline — Map A "The Basin" (25:00, 48 players)
| Time | Event |
|---|---|
| 00:00 | Dropship lobby: inventory, ping practice, squad ready check |
| 00:30 | Drop begins; dropship flies a random line across the map (≈65 m/s). Jump any time; **auto-eject at 02:10** |
| 03:00 | **Signal Cache #1** (airdrop, high-tier loot) announced 60 s before |
| 04:00 | **Elite Wave 1**: elite squads awaken at 2 random bases |
| 06:00 | **Signal Flare** enabled |
| 08:00 | **4 extraction points open** (drawn from pool of 12) |
| 09:00 | Dimming warning; **10:00 Phase 1** begins |
| 12:00 | **Elite Wave 2** + **Signal Cache #2** |
| 14:00 | Dimming Phase 2 |
| 18:00 | Extraction **re-roll**: 2 close, 2 new open (radio announces) |
| 20:00 | Dimming Phase 3 |
| 22:00 | **Last Bird** location revealed |
| 24:00 | Last Bird lands (departs 24:30) |
| 25:00 | Raid ends: anyone left is **MIA** (loses carried gear) |
Other maps use the same skeleton scaled (B/C 20:00, D 15:00, Skirmish 12:00: multiply times by length/25 and round to 5 s).

## 3. The Dimming (hazard front)
Irregular circle (noise-perturbed edge shader) with a **safe centre** chosen within 700 m of the map centre, revealed at 09:00.
| Phase | Radius end (Map A) | Time to close | Damage/s outside |
|---|---|---|---|
| 1 | 2600 m | 3:00 | 1 |
| 2 | 1500 m | 3:00 | 3 |
| 3 | 700 m | 2:30 | 6 |
| 4 | 150 m | 2:30 | 10 |
Inside the Dimming: bodycam **signal degradation** (tearing, RGB split, dropped frames), audio hiss, minimap jitter (not off), and from Phase 2 **Halo Drones** spawn. Never fully black the screen (readability).

## 4. Extraction
| Type | Rule |
|---|---|
| **Fixed point** | 20 s hold inside radius (8 m); enemies inside radius pause the timer; siren audible 300 m; squads extract together or individually |
| **Signal Flare** | Fire flare outdoors after 06:00 → helicopter in 60 s, 20 s window, rotor audible 600 m; not usable in Dimming or under roofs |
| **Send-Off Ramp** (2 per map) | Ride a bike over the Perimeter Canal gap; success = extract + **Style bonus** +5%…+15% loot value by trick quality; failure = fall damage + bike loss (no extraction) |
| **Rail Pod** (Map A/D) | Restore power at a substation objective, then 30 s boarding |
| **Boat** (Map A/C) | Harbor boats, 25 s boarding, slower but hidden from land |
| **Last Bird** | Cargo plane, 90 s window, contested mega-fight by design |
Extraction is denied if carrying weight >60 kg or while downed. Extraction rewards and stats screen appear instantly; replay cut is generated after.

## 5. Death and loss
- **Downed** at 0 HP: 45 s bleed-out, crawl, pistol only; squadmate **revive** takes 6 s (interruptible).
- **Death:** a **Body Bag** container drops with carried items (not Safe Case, not insured items). Squadmates can loot it; any items they extract return to the owner at settlement.
- **Squad wipe:** all members lose carried gear.
- **Disconnect:** 60 s **Signal Lost** hold (prone, 50% damage reduction, cannot extract); rejoin allowed for 90 s.

## 6. Settlement
Server signs a settlement (items in/out, kills, damage, extraction) → backend validates conservation rules → **profit ledger** (value extracted − value lost − fees), XP, Rep, Feed Rep, contract progress → **Reel** (highlights) → Safehouse. Full protocol in `08_BACKEND_SERVER_ADMIN/02`.

## 7. Acceptance
Timeline events fire at correct ticks under load · Dimming damage is server-side · extraction cannot be triggered by client claims · settlement is idempotent · reconnect restores state within 2 s.
