# UI design system (tokens + components)
> Read after: 01_BRAND, 05_ART/01 · Used in: Phase 1 (theme), Phase 5 (all screens) · Implementation: `05_UI_TOOLKIT_IMPLEMENTATION_A11Y_L10N.md`

## 1. Principles
1. **Diegetic first:** the screen is a bodycam feed; UI elements borrow OSD language sparingly (brackets, REC dot, timestamps, mono digits). 2. **Glanceable:** in raids, information is 1 glance, 1 tap. 3. **Thumb-first:** landscape only, ≥48 dp (≈9 mm) targets. 4. **Fair and honest:** no dark patterns. 5. **Lean:** few draw calls, no heavy blur.

## 2. Tokens (USS variables)
| Token | Value |
|---|---|
| `--bg` | `#0D1117` |
| `--surface-1 / -2 / -3` | `#141A22 / #1C242E / #26303C` |
| `--border` | `#2E3946` |
| `--text` / `--text-dim` | `#ECE7DB` / `#9AA3AD` |
| `--brand` (Laterite) | `#C8461F` |
| `--accent-gold` | `#F2B33D` |
| `--signal` (Cyan) | `#35E0D2` |
| `--rec` | `#FF2E3A` |
| `--ok` / `--warn` / `--danger` | `#3DBB6D / #F2B33D / #FF2E3A` |
| Rarity | grey `#9AA0A6` · green `#3DBB6D` · blue `#3B82F6` · purple `#A855F7` · gold `#F2B33D` |
Rules: saturated colours only for function (rarity, alerts, brand CTA). Backgrounds stay near-neutral. Contrast ≥4.5:1 for text, ≥3:1 for icons.

**Typography** (reference 1920×1080): Display 56/60 · H1 40/44 · H2 30/34 · H3 24/28 · Body 20/26 · Caption 16/20 · HUD numbers 36 mono. Fonts: **Chakra Petch** (headings, buttons), **Barlow** (body), **JetBrains Mono** (OSD, numbers). Sentence case for UI copy; OSD in caps mono. Fallback: Noto Sans families per language.
**Spacing:** 4/8/12/16/24/32/48. **Radii:** 4/8; feature cards use **chamfered corners** (9-slice sprite) with tiny corner brackets. **Elevation:** flat layers + 1 px borders, no blurred shadows.
**Icons:** SVG, 24/32/48 px grids, 2 px stroke, angular terminals. **Motion:** 120/200/320 ms, ease-out-cubic; press scale 0.97; screen transition = 160 ms "signal tune" (1-frame chroma slice + slide 24 px); reduce-motion setting disables everything except fades.

## 3. Components
Button (primary/secondary/ghost/danger/store) · toggle · slider · segmented control · tab bar · card (with brackets variant) · list row · **item slot** (rarity frame, count, durability bar, value chip) · currency chip (Scrip, Cred) · progress bars (HP, stamina, XP, hold-ring) · badge/dot · modal · bottom sheet · toast · tooltip · input field · reward tile · countdown ring · loading skeletons · empty state.
States: default, pressed, disabled, selected, focused (controllers/keyboard), loading, error. No hover-only affordances.

## 4. Layout
Landscape only, reference 1920×1080, aspect 16:9 to 21:9 and tablets 4:3. 12-column grid, 24 px gutters, content max 1680 px centered on ultra-wide. Safe-area insets applied to root; Android 16 edge-to-edge respected. HUD elements anchor to corners with per-element offsets from the layout editor.

## 5. Lens motifs (use on <10% of components)
Viewfinder corner brackets on hero cards, blinking REC dot for live states (matchmaking, recording), timestamp microcopy in headers (`KST-4471 // 21:33:07`), scanline wipe on reveals. Avoid full-screen noise on menus.

## 6. Copy and tone
Short, active, warm. Example: "Extract to keep it." not "You must successfully extract in order to retain your loot." Numbers formatted with locale; currencies with icons.

## 7. Do / don't
Do: strong hierarchy, generous spacing, one accent per screen. Don't: purple-on-white gradients, glassmorphism, emoji as UI, ALL-CAPS body text, stacked accent colours.

## 8. Admin panel
Uses the same tokens but calmer: dense tables, mono numerals, cyan only for live signals, laterite for primary actions; see `08_BACKEND_SERVER_ADMIN/04`.
