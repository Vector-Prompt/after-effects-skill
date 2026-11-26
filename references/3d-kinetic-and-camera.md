# 3D Kinetic Typography, Camera Rigging & SaaS Styles

## 1. 3D Kinetic Typography & Connected Corner-Spine Architecture (Motion Nations `UahA-6QYfC0`)
- **Zero-Plugin Editorial Palette**: Clean off-white (`#F0F0F0` / `#F4F3EE`) solid backdrop + ultra-bold **Bricolage Grotesque `headline800`** (`#1C1C1E` charcoal + `#10C820` / `#10B981` green accent words) + `motionBlur = true` (`shutterAngle = 180`). (Do **not** apply `QCA_Finishing` on light backgrounds.)
- **3D Track-Matte Opening Reveals (`TrackMatteType.ALPHA`)**:
  - For opening words (`animating`, `text`), create 3D `matteShape` rectangles sized via `sourceRectAtTime(0, false)`, parent both `textLayer` and `matteShape` to `Kinetic_Stage_Null`, and call `textLayer.setTrackMatte(matteShape, TrackMatteType.ALPHA)`.
- **Connected 90° 3D Corner-Spine Sculpture (All Words Stay Visible)**:
  - Build a continuous 3D architectural corner where **Left Wall** (`Y Rotation = 0°`, `RIGHT_JUSTIFY`, right-edge anchored `[r.left + r.width, r.top, 0]`) and **Right Wall** (`Y Rotation = -90°`, `LEFT_JUSTIFY`, left-edge anchored `[r.left, r.top, 0]`) meet seamlessly at a shared vertical corner seam (`[0, y, 0]` in Null space).
  - Do **NOT** trim/hide previous words—animate `Kinetic_Stage_Null` `Y Rotation` (`0° -> +42° -> +84° -> +38° -> +44°`), `X Rotation` (`±12°`), `Position` (panning down the staircase), and `Scale` (`82% -> 122%`) with `88%` Easy Ease (`KeyframeEase(0, 88)`) so both perpendicular walls appear simultaneously in dramatic 3D perspective.
  - Reveal each new word on its wall with `Scale` `[0,0,0] -> [100,100,100]` anchored at the corner seam.

## 2. Pro 3D Camera Rigging (Camera 3D Toolkit Pro `Eekm_Hf2wuU` / `g8NsT2VleXE`)
- **Exact 100% Framing Math (Zero Cropping)**: Read `zoom = cam.property("ADBE Camera Options Group").property("ADBE Camera Zoom").value` (~2666.67px for 50mm on 1920x1080). Place `cam` at `[960, 540, -zoom]` and parent to a 3D Null at `[960, 540, 0]`.
- **Overlapping Nulls Chain**: Chain `Cam -> Null_1 -> Null_2 -> Null_3` and overlap keyframe windows so camera motion never stops dead between targets.
- **Z-Parallax + Scale Compensation + Full-Bleed Orbit Guard**: Spread 3D layers in `z` and set `Scale = (zoom + z) / zoom * 100` so initial framing stays pixel-aligned. Whenever a background layer sits in 3D space (`z > 0`) under an orbiting/pulling camera, author the BG Pre-Comp at **`3840×2160`** (`Scale >= 125%`), cap camera orbit (`±10° Y`, `±5° X`), and place a `threeDLayer = false` gradient fallback (`BG_Safe_Base_2D`) at layer bottom so tilted corners never expose raw black edges.
- **Cinematic Auto-Focus DOF**: `Depth of Field = true`, `Aperture = 90–130`, `Blur Level = 120`, and keyframe `Focus Distance`.

## 3. SaaS Comparison, Framer & Explainer Styles
- **ShareAE Dark SaaS Comparison**: Deep Obsidian `#09090b`, `64x64` `Repeater` tech grid (`35%`), ambient `Gaussian Blur 240` glow, staggered (`160ms`) rounded cards with `ADBE Drop Shadow`, winner focus (`104%` + `PEDG2` Trim Paths laser border + subtle `CC Light Sweep`).
- **Zakaria HQ / ElevenLabs Explainer**: Deep Void `#06070b`, `ADBE Box Blur2` (`180px`) aura orbs, 2.5D tilted dashboard (`X: 28° -> 0°`), rolling counter expression, morphing prompt box + typewriter (`ADBE Text Percent Start`), exposure flash transitions (`ADBE Exposure2`).
- **Tom Lulofs / Framer & Lovable AI**: Animated radial-masked `64x64` grid + `wiggle(0.35, 110)` gradient blobs, hairline Bento guides + `+` ticks, blinking pipe prompt bar, continuous sinusoidal 3D card float (`Math.sin(time * 0.9) * 5`) + elliptical orbital feature badges.
- **Simply Put — Minimal 3-Scene SaaS Product Demo (`x18mEAYUIGk`)**:
  - **Scene 1 (Hero Emergence)**: Minimal high-contrast UI hierarchy, emerging logo/badge + headline, Null-driven zoom control, and deliberate `85–88%` Graph Editor easing (`F9`).
  - **Scene 2 (UI Shift & Feature Flow)**: Smooth Position/Scale handoffs between dashboard panels with generous negative space and zero visual clutter.
  - **Scene 3 (Final Null Zoom-Out & Polish)**: Master Null cinematic zoom-out reveal with `motionBlur = true` (`shutterAngle = 180`) and cohesive transition timing across all 3 scenes.
- **Tark — Viral Short-Form & Shattering Growth Graph (`ZRRQFWT17B8`)**:
  - **Stylized `12fps` + Edge Vignette Blur**: Adjustment layers with `ADBE Posterize Time` (`12`), `ADBE Noise2` (`16%`), and an inverted rounded-rectangle masked `ADBE Box Blur2` (`15px`, `360px` feather) for cinematic edge falloff.
  - **Corner-Anchored Circle Wipe**: White `Ellipse` anchored at top-right (`Scale 57% -> 350%`) with `ADBE Ramp` (`Blend With Original 100% -> 0%`).
  - **Rough-Edge Brush Underline**: Curved stroke (`8px`) + `ADBE Roughen Edges` (`Rusty`, `Border: 1`) + `Trim Paths` (`0 -> 100%`).
  - **Viral Growth Graph + Shattering Milestone Nodes (`1K → 5K → 10K`)**: Dark graph container + dashed reference line (`Dash: 25`) + glowing blue curved trend line (`Trim Paths` + `PEDG2` Deep Glow 2). On each milestone hit, trigger `ADBE Shatter` (`Glass`, `Repetitions: 200`) + 10-frame impact camera shake (`wiggle`) while `Camera_Null` tracks along the curve.

## 4. Core Motion & Sound Sync Principles
- **Beat-Locked Rhythm**: Align major cuts and card staggers to clean rhythmic intervals (`0.5s` / `1.0s` / `2.0s` grid) and add timeline cues via `comp.markerProperty.setValueAtTime(t, new MarkerValue("Beat_1"))`.
- **Clean Masked Text Reveals**: Place a `matteShape` covering the text's final bounding box, call `textLayer.setTrackMatte(matteShape, TrackMatteType.ALPHA)`, and slide `textLayer` up from below the baseline with `88%` Easy Ease.
- **Responsive Auto-Sizing Text Box**: `Rect Size` expression: `var r = thisComp.layer("TEXT").sourceRectAtTime(time, false); [r.width + 48, r.height + 28];`.
- **Vector Pre-Comp Sharpness**: Enable `collapseTransformation = true` on 2D vector Pre-Comps that scale above 100%.

## 5. Viral Talking-Head / Reels 5-Scene Editing System (`Qbntd8yud0o`)
When editing viral 9:16 talking-head Reels & kinetic explainers with dark spotlight depth:
- **3-Tier Glowing Gradient Typography**:
  - **Main Phrase**: Clean sans (`White -> Silver Gray` `ADBE Ramp` / `4ColorGradient`) + subtle `Drop Shadow` + `CC Light Sweep` (sweeping left-to-right, then resting back at center) + `PEDG2` (`Exposure: 0.1`, `Radius: 700` [or `180–250` on icons], Chromatic Aberration subtle).
  - **Red Alert Emphasis ("WAFFLERS", "PHONE")**: Bold slanted/display (`Red #FF2020 -> Orange #FF851B` gradient + Dark Red `Drop Shadow` + `CC Light Sweep` + `PEDG2`).
  - **Electric Blue Accent**: `Cyan #1EA8FF -> Royal Blue #0052FF` gradient + `CC Light Sweep` + `PEDG2`.
- **Background Spotlight Vignette (`Adjustment Layer`)**: Place over raw footage (`Gaussian Blur: 30`, `Brightness: -100`, `PEDG2: 0.01`) clipped by an **Inverted, Feathered Circle Mask** so edges darken/blur while the speaker and glowing UI stay spotlighted.
- **Pre-Comp Adjustment Glow for Multi-Icon Groups**: Never apply `PEDG2` separately onto small ungrouped icons if their bounding box clips glow; place icons + shared `Adjustment Layer` (`PEDG2 Exposure: 0.1`) inside a Pre-Comp (and pre-comp 2D effect text before mixing into `Advanced 3D` comps so 2D blurs/glows don't vanish!).
- **Scene Transitions & Diagrams**:
  - **Scene 1 (Expanding Pill + Roto Sandwich)**: Expand glass pill width along `X Size` (`0 -> W`, `2px` gray stroke) + center line (`Scale X: 0 -> 100%` anchored center) + tint icons + pop a glowing User Card *behind* the cutout layer.
  - **Scene 2 (B-Roll Split Crop)**: Radial dark-blue background (`#23334B -> #060A14`) + rounded split frames + inverted feathered rect edge-glow vignette (`Blur: 40`, `PEDG2: 0.1`).
  - **Scene 3 (`ADBE Shatter` Zero-G Glass Fracture)**: Pre-comp the hero title + 3D object, apply `ADBE Shatter` (`View: Rendered`, `Pattern: Glass`, `Repetitions: ~5.7`, `Extrusion Depth: 0.20`, `Force 1/2 Strength: 3`, `Gravity: 0`, **`Viscosity: 1.0`**) so glass cracks hang stiffly in mid-air.
  - **Scene 4 (Top-Down Studio Schematic)**: Overhead Camera + Key Light sliding in over radial dark-navy ground, **Repeater/Dashed `Trim Paths`** sightlines, translucent Light Cone (`Triangle` shape + heavily feathered `ADBE Linear Wipe 100% -> 0%`), all unified with `PEDG2` (`Exposure: 0.1`, `Chromatic Aberration: 1% Red & Blue`).
  - **Scene 5 (`Transform` Zoom Camera + SFX Markers)**: Top **`ADBE Geometry2` (`Transform`)** Adjustment Layer for punchy Reels zoom-ins/outs paired with 3-tier sound markers (`Whoosh` on zooms, `UI Click / Gear / Glass Shatter / Shutter` on graphic beats, plus musical bed).

## 6. Brutalist Editorial Kinetic Typography System (`pldJPbHAWfQ`)
Pure monochrome (`#FFFFFF` / `#000000`) + vivid Red (`#FF1A1A`) typography motion-graphics techniques requiring zero paid plugins:
- **Per-Character `Ramp Up` Cascade + Outline Echoes**:
  - Add Text Animator (`ADBE Text Position 3D` offset `+110px` **AND `ADBE Text Opacity = 0`** so displaced letters stay invisible until swept!), set Range Selector Shape to **`Ramp Up` (`2`)**, **`Ease High: 25%`**, **`Ease Low: 100%`**, and key `Offset` `-100% -> +100%`.
  - Keep the 2 hollow **Outline Echo Copies** (`TextDocument.applyFill = false`, `applyStroke = true`, `strokeWidth: 2`) hidden until center resolves, then expand them cleanly outward from `[center]` (`±148px` with asymmetric `90/16` curve) rather than rendering static outline stacks on frame 0.
  - For geometric pointed Eye cutouts, always keep `[0, 0]` tangents on the left/right tips and use a true color-highlighted `'o'` glyph with cumulative `sourceRectAtTime` kerning before zooming into circles.
- **`ADBE Tint` Split-Screen Inversion Wipe**:
  - Place a comp-sized `Adjustment Layer` with **`ADBE Tint`** (`Map Black To White [1,1,1,1]`, `Map White To Black [0,0,0,1]`, `Amount: 100%`) and animate its `Position X` (or `Scale`) across the frame to invert everything beneath (text, outlines, grids, circles) black <-> white in real time. *(Disable before red-accent scenes so red stays vivid!)*
- **Red-Pivot `90°` L-Corner Snap**:
  - Pivot suffix text strictly at its **lower-left typographic baseline `[r.left, 0]`**, and **shift its horizontal `Position X` right by `+prefix.height + gap` while at `-90°`** (since `-90°` rotates upward ink `y < 0` leftward into `-X`), then animate `X -> joinX + gap` simultaneously as `Rotation` snaps `-90° -> 0°`.
- **Circular Grid Expansion + Cascading Upward Word Wall**:
  - Reveal a thin grid inside an expanding black circle (`Scale 0 -> 130%`) while stacking hero words (`Dynamic -> Limitless -> Electric`): each new word slides up as previous words shrink and step upward.
- **Per-Letter Opposing Spatial-Arc Converge**:
  - Measure per-character X centers using **cumulative prefix widths (`word.substring(0, i+1)`)** to preserve true font kerning, set **vertical anchor to baseline `y = 0`** (never bounding-box center `r.top + r.height/2` which scrambles `'p'`/`'d'`/`'t'` baselines!), use clean geometric Swiss sans for hollow stroke echoes (`HelveticaNeue` / `Montserrat` / `Inter`, avoiding ink-trap counter overlaps), and converge with `540°` Motion Blur.

## 7. 5 SaaS Style Text Animations System (`8pPk9c7eSM4`)
Modern high-craft SaaS product reveal typography mixing clean geometric sans + italic cyan-blue highlight words over deep indigo/violet/cobalt/amber 4-Color Gradient backdrops (`0` paid plugins):
- **Typography Hierarchy Rule**: Pair clean upright display text (`Bricolage Grotesque` / `SF Pro Display` `Bold`/`SemiBold`) with **italic emphasis + bright accent tint** (`#38BDF8` sky-cyan or `#FF6B1A` amber) on the focal phrase (`"for Editors"`, `"built for SaaS"`), rather than styling every word identically.
- **01 — Native `Ramp Up` Clean Reveal (Words or Characters)**:
  - Add Text Animator (`ADBE Text Position 3D = [0, 100, 0]`, `ADBE Text Opacity = 0`).
  - Set Range Selector `Shape = Ramp Up (2)`, `Ease Low = 100%`, `Based On` (`ADBE Text Range Type2`) = `Words (3)` or `Characters (1)`, and animate `Offset: -100% -> +100%` over `~1.0s`. *(Pure fade variant: keep `Position = [0, 0, 0]`.)*
- **02 — Per-Character 3D Z-Depth + Text Blur Reveal (`CINEMATIC`)**:
  - Center-align text, enable **`threeDLayer = true`** AND **`textLayer.threeDPerChar = true`** (`Enable Per-Character 3D`).
  - Inside Text Animator add `Position = [0, 0, -5000]`, `Opacity = 0%`, and **`ADBE Text Blur`** (`[35, 35]`).
  - Set Range Selector `Shape = Ramp Up (2)`, `Ease Low = 100%`, `Offset: -100% -> +100%` so individual letters fly forward out of deep Z-space and sharpen into focus.
- **03 — Hero Scale Character Sweep + Synced Gaussian Blur (`Build`)**:
  - Scale headline large and apply `01` (`Based On = Characters`, `Ramp Up`) while sweeping Layer `Position` horizontally + **`ADBE Gaussian Blur 2`** (`Blurriness: 100 -> 0` with `Repeat Edge Pixels = 0`) over `12–18` frames with strong asymmetric settle (`90/16` Speed Graph).
- **04 — Zero-Keyframe `Expression Selector` Bounce + Optional Color Tint**:
  - Remove default `Range Selector 1`, add **`ADBE Text Expressible Selector`**, set `Position = [0, 120, 0]`, `Opacity = 0%`, optional **`ADBE Text Fill Color`** (accent cyan/blue so words flash color as they enter and settle white), set `Based On = Words (3)`, and drive `ADBE Text Expressible Amount` with damped cosine decay (see `expressions-cookbook.md` §32).
- **05 — Zero-Keyframe `Source Text` Typewriter + Blinking Pipe Cursor**:
  - Zero animators, zero keyframes: drive `ADBE Text Document` directly with `charsPerSecond`, `cursorBlinksPerSecond`, and `elapsedTime = Math.max(time - inPoint, 0)` (see `expressions-cookbook.md` §33).
