# Game modes, social features, live-ops events
> Read after: 02_MATCH_FLOW · Used in: Phase 5 (lobby), Phase 11 (extra modes), live-ops docs (08/06)

## 1. Modes
| Mode | Players | Length | Gear risk | Maps | Notes |
|---|---|---|---|---|---|
| **The Run — Standard** | 48/32 | 25/20 min | yes | A, B, C | flagship |
| **The Run — Skirmish** | 24 | 12 min | yes | A (smaller circle), B, C | fast Metro-Survival-Drop-style |
| **Underline** | 16 | 15 min | yes | D | CQB, darkness, IR bodycam |
| **Bodycam Arena** | 6v6 | 5–8 min | none | arenas derived from bases | TDM, Hardpoint, Gun Game, Wingman (2v2). Separate XP and cosmetics only |
| **Freewheel** | 1–8 | free | none | Stunt Park | time trials, trick score, cosmetics/Feed Rep only |
| **Tutorial raids** | 1–4 | 8 min | none | Slice | three guided PvE raids |
Rankings: extraction modes rank by **season profit + survival rating**; Arena by Elo-style MMR. Ranked Arena disables aim assist.

## 2. Party and squads
Party up to 4 (invite by code/friend). Squad fill option with matchmaking. Roles are soft (scout, rider, medic, anchor); UI shows squad status: HP, bleed, weapon, ammo, distance. Squad gear return rule from Body Bags.

## 3. Communication
Ping wheel (enemy, loot, go, wait, extract, danger) with distance/direction · quick chat presets (localized) · text chat (lobby only, filter + report) · **voice** (Phase 6): push-to-talk default, per-player mute, report, proximity off (squad only). Toxicity: report button on post-raid screen; admin moderation queue with chat logs and replay slice.

## 4. Social
Friends (code + platform friends optional), recent squad list, clans (post-launch), leaderboards (season profit, best Hype, longest ride), Reels sharing (system share sheet). Streamer mode hides names and chat.

## 5. Live-ops events (calendar driven from admin panel)
- **Weekly rotation:** featured map bonus, contract set, trader specials (Scrip only).
- **Seasonal event (every 4 weeks):** limited contracts, cosmetic track, special world dressing, temporary modifier (e.g. **Blackout Night**: darker map + IR bodycam; **Double Elites**).
- **Community goals:** server-wide extraction of N Halo Cores unlocks a cosmetic for everyone.
- **Maintenance windows** announced 24 h ahead via in-game banner and push.

## 6. Ranked/anti-abuse
Placement runs (5); Threat Rating prevents smurfing at Rookie; team-killing penalties (Scrip fine, matchmaking penalty); AFK detection in Arena.

## 7. Acceptance
Modes selectable from data (`modes.json`) without client update when only parameters change · party invites work across platforms · Freewheel needs no backend economy access.
