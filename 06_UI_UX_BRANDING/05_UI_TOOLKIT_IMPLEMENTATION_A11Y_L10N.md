# UI Toolkit implementation, accessibility, localization
> Read after: 02–04 in this folder · Used in: Phase 1 (theme + HUD skeleton), Phase 5 (screens)

## 1. Structure
```
Client/Assets/UI/
  Theme/        Tokens.uss (variables), Typography.uss, Themes/{default,highcontrast,cb_*}.uss
  Components/   Button.uxml, ItemSlot.uxml, CurrencyChip.uxml, ... + C# custom controls
  Screens/      Lobby, Deploy, Loadout, Store, ...
  HUD/          HudRoot.uxml, controls (Stick, LookZone, FireButton, ...)
  Icons/        SVG + generated sprite fallbacks
  Fonts/        Chakra Petch, Barlow, JetBrains Mono (OFL) + Noto fallbacks
  PanelSettings/ HUD, Menus, Modals, Toasts
```
Naming: BEM-like `.nl-btn`, `.nl-btn--primary`, `.nl-card__title`. All colours/spacings via USS variables from the token table.

## 2. Panels and scaling
`PanelSettings`: Scale With Screen Size, reference 1920×1080, screen match 0.5 [VERIFY best value with device tests]; sort order HUD 0 · OSD 5 · Menus 10 · Modals 20 · Toasts 30. Multiple `UIDocument`s, not one giant tree.

## 3. Architecture
MVVM: `ViewModel` (plain C#, observable properties) ↔ UXML via Unity runtime data binding [VERIFY 6.3 API] or a thin manual binder. `ScreenRouter` (push/pop, transitions, Android back). Services (Inventory, Store, Party, Match) are injected; screens never call HTTP. Sim → HUD via a `HudViewModel` updated from events, not polled.

## 4. Performance rules
No `Update` polling; use `schedule.Execute` sparingly. `ListView` virtualization with fixed item heights for stash/store/lists. Pool feed/toast/damage-arc elements. Minimise transparent overdraw and nested masks; avoid `overflow:hidden` on huge trees. `usageHints` (DynamicTransform/GroupTransform) for animated elements. Atlas sprites; SVG icons imported as vector images — **profile on tier L; if costly, pre-rasterize to sprite atlases at build**. Pre-warm dynamic font glyphs at splash to avoid hitches. Shader Graph UI effects (glitch, sweep) only where the design system allows, tier H first. Menu CPU budget 0.8 ms/frame tier M.

## 5. Safe areas and edge-to-edge
Android 16 (API 36) enforces edge-to-edge. A `SafeAreaController` reads `Screen.safeArea` and cutouts each orientation change and pads the root; test on notched devices, foldables and gesture navigation. HUD anchors relative to safe area corners.

## 6. HUD input
HUD controls are custom `VisualElement`s using pointer events with **pointer capture per pointer ID**; multi-touch verified on 5+ simultaneous touches; look zone uses raw delta with a smoothing setting; no allocations in handlers. Menus use standard UITK controls. Predictive back gesture handled by the router.

## 7. Accessibility
Text scale 80–130% via `--font-scale`; colour-blind palettes (deuteranopia/protanopia/tritanopia) via theme USS + shape/icon rarities; high-contrast theme; reduce motion; adjustable hold times; one-handed layouts; haptics toggle; subtitle size/background; screen-reader labels on interactive controls where Unity 6.3 supports it [VERIFY]; minimum touch target 48 dp; never colour-only signals.

## 8. Localization
Unity Localization package: string tables + smart strings for plurals; launch EN, then FR, PT, ES, SW, AR (RTL: **verify** UI Toolkit RTL/text-shaping support, plan plugin/workaround), others later. No text baked in textures; +40% length headroom; number/date/currency formatting via culture; pseudo-localization test build; font fallbacks with Noto families per script; voice lines localized via addressable audio tables.

## 9. Tests
UITK test helpers for navigation; screenshot tests at 16:9, 19.5:9, 21:9, 4:3; monkey test (random taps 10 min, no exceptions, no allocations spikes); accessibility snapshot tests (scale 130%, high contrast); localization overflow test.

## 10. Delivery checklist
Tokens only (no hard-coded colours) · every screen has loading/empty/error states · every button has audio + haptic hooks · all strings localized keys · UI CPU budgets met on tier L and M · back navigation verified.
