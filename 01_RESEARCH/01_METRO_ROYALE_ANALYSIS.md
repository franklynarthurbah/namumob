# PUBG Mobile "Metro Royale" — analysis for Namulinda
> Read after: 00_START_HERE/01 · Used in: Phases 0–3 (design intent) · Sources: `03_FUSION_DESIGN_NAMULINDA.md` §6
> Reference videos (titles/descriptions only were readable): "Metro Royale Mode EXPLAINED!! (Talent Points, Metro Cash, etc)" (Nov 2020) · "Metro Royale Advanced Tips – Use the Environment" (Feb 2025) · "Enter the Metro Royale!" (Nov 2020).

## 1. What the mode is
A PvPvE **scavenge → fight → evacuate** mode separate from classic battle royale. A squad deploys with gear chosen from a persistent stash, loots a map within a time limit (roughly 20–30 minutes depending on variant), fights AI enemies, bosses and other squads, and must reach an extraction point to keep what it carries. Dying loses carried loot except protected items. Winning is measured in **profit**, not last-alive.

## 2. Mechanics catalogue → Namulinda mapping
| # | Observed in Metro Royale | Namulinda adaptation |
|---|---|---|
| 1 | Pre-raid loadout from stash; everything carried is at risk | Same. Free **Standard Kit** each raid (no fear of zero gear). **Gear Value** shown before deploy |
| 2 | Lock/password box: protected items always return | **Safe Case** (2 slots, upgradeable to 6 via Safehouse, never purchasable) |
| 3 | Squadmates can recover a dead teammate's marked gear; it returns to the owner on extraction | **Body Bag** container + squad-return rule (owner gets item back if the recoverer extracts) |
| 4 | Several extraction points, only some active per match; timers; camped by players | Pool of 12, 4 active per match; 20 s hold timer, contested pause; **Send-Off Ramps** (stunt extraction) |
| 5 | Evacuation Flare Gun calls a helicopter to your position (about 1–3 min depending on variant) | **Signal Flare** consumable: 60 s arrival, loud rotor, not usable under roofs/in Dimming |
| 6 | AI factions, elite/boss waves at set times (community: about 4 and 12 min marks) | Concession Guards, Scrap Crews, Halo Remnants; **elite waves 04:00 and 12:00**; one boss per base |
| 7 | Radiation zones, dynamic weather, subway/underground routes (random layouts) | **The Dimming** front, weather, **Underline** tunnels with power puzzles |
| 8 | Black Market: sell loot, buy gear; Metro Cash | 4 **Traders**, **Scrip**, buy/sell spread |
| 9 | Talent points: permanent growth (survival, sprint, capacity...) | **Aptitudes**: small capped bonuses, earned only |
| 10 | Quality tiers, armor plates, backpack upgrades, special weapon traits | Rarity tiers, armor classes with durability, backpack tiers |
| 11 | Matchmaking by gear value (Basic vs Advanced map) | **Brackets** by Gear Value + recent-extraction "Threat Rating" (anti-sandbagging) |
| 12 | Missions/quests give XP, cash, upgrade items | **Contracts** board in Safehouse |
| 13 | Survival Drop variant: 20 min, Super Airdrop ~6 min, only 3–4 fixed extractions | **Skirmish** (12 min, 24 players) |
| 14 | Motorcycles/cars for rotation and extraction, smoke cover | **Stunt bikes** and vehicles as first-class traversal |
| 15 | Small lobbies in recent seasons (community reports 8–16 players [VERIFY]) | Namulinda targets 48 on Map A. Player count is a server parameter; validate in Gate 2 and beta |
| 16 | Honor/Fame progression that does not use backpack space | **Feed Rep** |
| 17 | Custom rooms | Private raids (post-launch) |

## 3. Why it works (design lessons to preserve)
- Tension = **risk × time**. Every minute adds loot value and threat. Encounter density rises after mid-match.
- **Value is legible:** every item shows a value; the post-raid screen is a profit ledger.
- PvE is a *lower-variance income* with real danger; PvP creates spikes. Both must pay.
- Multiple exits reduce single-point camping but still create ambush geometry: reward scouting.
- Persistent stash + meta upgrades give a reason to return after a loss.

## 4. Pain points to fix
- **Sandbagging/rat play** (minimal gear, camping extractions): bracket by carried value + recent extracted value; add flare/bike exits; make extraction zones defensible only briefly (hold timer, audio cue, no-camp radius rules).
- **Power sold for money:** forbidden (D-13). Aptitudes and Safehouse upgrades are earned only.
- **Complexity for newcomers:** three guided low-risk raids, tooltips, "Standard Kit" safety net.
- **Server cost of AI:** AI budget and LOD in `02_GAME_DESIGN/07`.

## 5. Do NOT copy
Names and terms (Metro Royale, Metro Cash, Talent, Undercover Pass, Lock Box, faction names), maps, bosses, UI art, item designs, skins, audio, voice lines.

## 6. Owner video-notes template
| Video | Timestamp | Observation | Idea for Namulinda |
|---|---|---|---|
| (add) | | | |
