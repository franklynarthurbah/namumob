# Operators (player characters), AI models, customization
> Read after: 01_ART_DIRECTION · Used in: Phase 1 (placeholder), Phase 5 (customization UI), Phase 6 (full sets)

## 1. Base bodies and heads
Two base bodies (**Body-M**, **Body-F**) on one shared Humanoid skeleton (≈52 bones + sockets). **8 head sculpts per body**, **12 skin tones** (deep to light, tuned shading so dark skin keeps highlights and readable features under low light), **8 eye colours**, **20 hairstyles** (afro, twist-out, box braids, locs, cornrows, short crop, fade, bald, buzz, bun, ponytail, headwrap, headscarf, and more), **6 facial-hair styles**, 3 build presets + 3 blend-shape sliders (shoulder width, waist, height ±4%).
Diversity rule: dignified, non-caricatured, culturally aware; run a sensitivity review before launch.

## 2. Slots
Head (hair/hat/helmet) · Face (mask/goggles) · Torso (shirt/jacket) · Vest/armor · Legs · Boots · Gloves · Backpack · Rig · Armband · **Kestrel bodycam frame (cosmetic)** · Weapon skin · Bike skin · Parachute trail · Emote · Safehouse theme · Nameplate/banner. Gear in slots follows the gameplay item (armor look = armor class); cosmetic *skins* override look only.

## 3. Budgets
| LOD | Tris (whole character incl. gear) | Range |
|---|---|---|
| LOD0 | 12k | 0–25 m |
| LOD1 | 6k | 25–60 m |
| LOD2 | 2.5k | 60–120 m |
| LOD3 | 1k (impostor card on tier L) | >120 m |
Draw calls per character ≤3: pre-combine clothing meshes into **one skinned mesh** at load (or ship baked outfit meshes) with 1 clothing atlas (1024²) + skin/hair materials. ≤20 characters at LOD0/1 on tier M. Textures: body 1024², extras 512².
**First-person arms** (arms+gloves, separate rig) ≤10k tris, share skin/glove choice.

## 4. Colour/pattern customization
Mask texture channels: R primary, G secondary, B accent; palettes from `01_ART` §2 plus 24 extra hues; camouflage patterns as tiling textures. **Redaction:** vertex-colour R marks the face region for the mosaic shader.

## 5. Rarity and availability
Standard (free), Refined, Prime, Mythic. Free unlocks via levels/contracts; purchasable via Cred; Feed Pass rewards. **No gameplay difference** (see `09_MONETIZATION_IAP`).

## 6. AI character sets
Scrap Crew (4 variants), Concession Guard (4), Halo Remnant (drone, sentry, warden) + 2 elite variants per faction + 6 unique boss models for Map A (bosses B/C/D added with maps). Built from the same kit with faction palettes; bosses have bespoke silhouettes (see `02_GAME_DESIGN/07`).

## 7. Animation compatibility
Humanoid avatar for all characters; AI share the player animation set; boss-specific clips (3–6 each). Server never runs animators for players (stance-based hitboxes).

## 8. Data definitions
`operators.json` (base body/head/skin/hair ids), `cosmetics.json` (id, slot, rarity, mesh/material ref, price Cred, unlock rule, tags). Item IDs `cos_<slot>_<name>`. The client asks the server for the equipped loadout; the server replicates only cosmetic IDs (uint16 each) in the spawn message.

## 9. Acceptance
Character creator produces valid combos (no clipping across 200 random builds, automated) · 20 LOD0/1 characters within frame budget on tier M · redaction toggles at runtime · dark-skin tones pass the low-light readability test (screenshots reviewed).
