# Apple UI & SaaS Glass Motion Patterns

## 1. Apple UI Morphing Button & 3D Gyro Face ID (BentoMotion `iqhcdoWrdZM`)
- **Soft Studio Spotlight Backdrop**: Cool light-gray solid (`#E8EBF0`) + centered white `Ellipse` blurred (`ADBE Box Blur2` ~`260px`).
- **Slider-Driven Morphing Pill (`X`/`Y` Sliders + Staggered Bounce)**:
  - Add `"X"` and `"Y"` `Slider Control` effects to `box` and link `ADBE Vector Rect Size` via `[effect("X")("Slider"), effect("Y")("Slider")]`.
  - Start as circle (`140x140`, `Roundness: 70`), expand to pill (`520x136`) with a **3-frame stagger** between `X` and `Y` and `bounceExpr(0.05, 2.4, 7.0)`.
- **3-Tier Null Rig (`Zoom_Null` -> `Click_Null` -> `Main_Null` -> `Text_Scale_Null`)**:
  - `Main_Null`: `0% -> 100%` entrance scale with `bounceExpr(0.045, 2.5, 12.0)`.
  - `Text_Scale_Null`: button label scale (`decay = 10`) + `ADBE Box Blur2` (`28 -> 0` on reveal, `0 -> 28` + `Opacity 100 -> 0` on press).
  - `Click_Null`: 4-keyframe press squish (`100 -> 86 -> 104 -> 100`) triggering the blue pill (`#007AFF`) to morph `X`/`Y` sliders, `Roundness`, `Fill Color`, and `ADBE Drop Shadow` color into a dark Face ID squircle (`260x260`, `#111318`).
- **Set Matte Shine Sweep**: Parent a blurred white `Ellipse` (`light`) to `box` and apply `ADBE Set Matte3` pointing to `box` so the sweep clips cleanly inside the morphing pill.
- **Pre-Comped Face ID (`1000x1000`, Classic 3D)**:
  - Stage 1: 4 `Trim Paths` corner brackets + rounded vector eyes/nose/smile (`ADBE Vector Stroke Line Cap = 2`), trimming out (`outPoint`) right as Stage 2 begins.
  - Stage 2: Dual 3D `Ellipse` stroke rings rotating `+360° X` / `-360° Y` (offset mid-keys on Ring 2) + wide-stroke (`44px`) duplicates with `CC Force Motion Blur` (`ADBE Force Motion Blur`).
  - Stage 3: Rounded vector Checkmark (`✓`) draw-in via `Trim Paths` (`0% -> 100%`) + slow `Zoom_Null` push-in (`100% -> 108%`).

## 2. Apple Stacked Frosted-Glass Notifications (Zuyns `tYjqtArZtds`)
- **Light Studio + 3D Perspective Grid Floor**: Off-white/cool-gray gradient (`#F2F4FA -> #DCE2EF`) + 3D-rotated (`X Rotation = 78°`) hairline grid floor with horizon fade.
- **True Frosted-Glass Pill Stack**: Translucent white pill (`58%` fill, `85%` 1.5px rim stroke, `ADBE Drop Shadow`) + Adjustment Layer behind each pill with `ADBE Box Blur2` (`28px`, Repeat Edge Pixels) and `setTrackMatte(pillShape, TrackMatteType.ALPHA)`.
- **Reactive Stack Cascade**: Parent each card to a master Null with Bounce; when Card 2 arrives, Card 1 slides down (`+88px`), scales (`100% -> 92%`), and dims (`100% -> 88%`).

## 3. Apple App Store Card, Reflection & Face ID Scan (Orlando `ccwvClDGf4c`)
- **Lavender Studio + Sonar Rings**: Radial lavender-to-white gradient (`#C9BDF2 -> #F5F2FC`) + 3–4 expanding concentric white stroke rings (`10–25%` opacity).
- **Floor Reflection Trick**: Duplicate card Pre-Comp beneath main card, flip vertical scale `[100, -100]`, apply `ADBE Gaussian Blur 2` (`24px`), and mask with a feathered top-to-bottom opacity fade (`28% -> 0%`).
- **Micro-Scan to Green**: Shift inner Face ID facial group `[-8, 2] -> [8, -1] -> [0, 0]`, then transition strokes to `#1CE868` with `PEDG2` Deep Glow 2 + bouncy scale pulse.

## 4. Apple Liquid Glass UI & Vibrant SaaS Panels (MDMZ `Czh7wbRtv2Y` / Neuron FX `fJJ6YhaUWB8` / Editwithtrix `POffjDZbdlQ` / `GLWmMmlMcno`)
- **3-Layer Native Optical Glass (`MDMZ` + `GLWmMmlMcno` Apple-Ad Upgrade)**:
  1. `Refraction` Map Layer: Duplicate rounded card (`Roundness: 80`), `Fill = Black`, `Inner Glow` (`White`, `Blend: Normal`, `Opacity: 100%`, `Size: 40px`) — or Pre-Comp `Refraction_Map_Pre` (`Black` core + `40px` blurred white inner bevel clipped to card via `ALPHA` matte for 100% deterministic ExtendScript execution).
  2. `Liquid Glass` Shape-Adjustment Layer (`adjustmentLayer = true`): Exact 5-effect optical stack (`GLWmMmlMcno`):
     - **`ADBE Brightness & Contrast 2`**: `Brightness: 50`, `Contrast: -100` (milky lifted frosted floor)
     - **`ADBE Gaussian Blur 2`**: `Blurriness: 48–60` (`Repeat Edge Pixels: 1`)
     - **`ADBE Noise2`**: `Amount: 5%` (`Use Color Noise: 0`)
     - **`ADBE Geometry2` (`Transform`)**: `Scale Height` (`ADBE Geometry2-0003` = `120`) + `Scale Width` (`ADBE Geometry2-0004` = `120`) for `1.2x` optical magnification *(CRITICAL: `-0003` is `Scale Height`, NOT a Uniform Scale checkbox — setting `-0003 = 1` shrinks the glass output to 1% height!)*
     - **`ADBE Displacement Map`**: `Layer = Refraction`, `Effects & Masks`, `H/V Max Displacement = 30–50`, `Luminance`, `Wrap Pixels`
  3. `Stroke` Specular Bevel Rim: Duplicate card (`Fill: None`, `Stroke: White 1.5–2px`, `Round Cap = 2`) on top. *(Keep Apple dark-violet ribbons ultra-smooth using oversized Solids `2400x1600` + `ADBE Mask Feather` — never raw Shape Layer `Gaussian Blur` which clips at unexpanded bounding-box seams, and never jagged `Turbulent Displace`; tune crystal contrast lightly e.g. `+18 Bright / -28 Contrast`, `Blur 32px` instead of chalky `-100` blowout over light stripes, and omit heavy QCA chromatic fringe on crisp white UI text.)*
- **Ultra-Soft `300px` Silk Glassmorphism Card (`tWjSO__gjfQ`)** *(clean, fast UI card over glowing aurora blobs)*:
  1. **Glowing Gradient Blob Backdrop**: Deep radial cobalt-navy BG (`#050C7A`) + oversized **`1100×1100` Solids + `ADBE Ramp` + heavily feathered `Ellipse` Masks (`95–180px` feather)** with unique Lissajous drift paths, phase offsets, and scale breathing — **never** unblurred hard circles spinning in uniform `sin/cos` synchronization (which looks like a linked wheel), and **never** fractal `Turbulent Displace` (which creates ragged splatters)!
  2. **`Glass_Card_Pre` (Self-Contained `STENCIL_ALPHA` Slab) + Full-Bleed 3D Camera Stage**: Nest the `300px` Silk-Blur + `4–5%` milk tint + `2px` hairline rim + sweeping `CC Light Sweep` inside `Glass_Card_Pre` with a `BlendingMode.STENCIL_ALPHA` cutout, then animate it in `Anim_PreComp` (`Z = 0`) over a **2D full-bleed `BG_Safe_Base_2D`** (`threeDLayer = false`) + **4K `3840×2160` `BG_Aurora_3D_Plane` (`Scale >= 125%`, `Z = +360`)** with calibrated `Main_3D_Camera` orbit (`±10° Y`, `±5° X`, DOF locked) and soft drop-shadowed `Z = -38` front typography (fast unified fade `<= 0.25s`, never dragging partial per-letter opacity over glowing streaks).
- **Dynamic Island Morph**: Circle (`[88, 88], R:44`) -> Search Pill (`[520, 88], R:44`) -> Expanded Card (`[780, 440], R:28`) via `ADBE Vector Rect Size` (`0.45s–0.75s`, `88%` Easy Ease).
- **UI Entrance Scale Rule**: Never pop UI cards from `0% -> 100%`; use subtle `94% -> 100%` uniform scale + `Opacity` + `TrackMatteType.ALPHA` (except intentional bounce-pop pills).
- **3D Background Frustum & Inner-Glass Alignment Rule**: Whenever placing a background plane in 3D (`Z > 0`) under an orbiting/dolly 3D camera, NEVER use a `1920×1080` layer at `<= 150%` alone (tilted/zoomed corners expose black gaps). Always place a `threeDLayer = false` gradient fallback (`BG_Safe_Base_2D`) at the bottom, author the 3D BG precomp at **`3840×2160`** with `Scale >= 125%`, keep `card3D` visible from `t = 0` (`Opacity >= 40% -> 100%`), AND link the glass precomp's inner BG copy (`Aurora_Behind_Card` / `BG_Source_Inside_Glass`) to `BG_Plane.scale * (camHoldZ / (camHoldZ + bgZ))` (~`0.815`, ~`102%`) instead of leaving it shrunk at `54%` so the frosted glass edge blends seamlessly with the 3D background.

## 5. iMessage Glass Input Bar & Higgsfield Cursor (Bart_VFX `gr-znnBnhpk` / Mojtaba Sorbi `QGS0sZmyAks`)
- **Pre-Comped 3D Chat Container**: Nest 2D chat UI in `1240x760` Pre-Comp, then tilt in 3D (`Y: -7°`, `X: 3°`) over deep teal fluid mesh (`4-Color Gradient + Motion Tile Mirror + Turbulent Displace`).
- **Typewriter + Blinking Pipe**: `ADBE Text Percent Start` (`0 -> 100%`) + `Math.round(time * 2.5) % 2 * 100` on pipe `|` Opacity + send button squish (`100 -> 82 -> 100`) + vector tail speech bubble bounce.
- **Tip-Anchored Cursor**: Anchor at exact pointer tip (`[-22, -32]`), curved `setSpatialTangentsAtKey()` arcs, velocity banking tilt (`Math.max(-14, Math.min(14, transform.position.velocity[0] * 0.012))`), and expanding click ripple rings.

## 6. Viral Clean Light/Blue SaaS Style (Tark `EicGPYk7cFc`)
- **4-Color Gradient + Noisy Blue Glow Backdrop**: Solid BG with `ADBE 4ColorGradient` (2 points White, 2 points Bright Blue pushed outside frame edges) + blurred light-blue `Ellipse` (`Gaussian Blur 300`, Repeat Edge Pixels OFF) + `Noise 12%` (Use Color Noise OFF). Outer UI container uses light gray fill, `5px` white stroke, and ultra-soft `Drop Shadow` (`Opacity 10%`, `Distance 1`, `Softness 230`). (Do **not** apply `QCA_Finishing` on light backgrounds.)
- **Interactive Toggle Switch Cascade**: Rounded pill tracks (`Gray` OFF ↔ `High-Contrast Blue` ON) + white circular thumbs (`Drop Shadow` `12%`, `Distance 2`, `Softness 27`), animated in staggered beats with **10-frame micro-holds** and `85%` Speed Graph easing.
- **Masked Vertical Word-Roller**: Anchor word on the left + vertical word stack (`+120px` Y spacing) parented to a `Roller_Null` inside a Pre-Comp (`-120px` Y per step, `10-frame` slide + `5-frame` hold, `80%` Easy Ease + Motion Blur), clipped to a single-line matte (`TrackMatteType.ALPHA`), with `ADBE Ramp` (Navy → Bright Blue) on the text.
- **Fanned 5-Card Depth Stack + DOF**: Center hero card (`100%`, `0°`, `Gaussian Blur 12 -> 0` focus-in) flanked by mid cards (`90%`, `±10°`) and far cards (`70–80%`, `±20°`, static `Gaussian Blur 8` DOF), plus a `Trim Paths` strike-through price line and popping discount badge.
- **Null-Driven Node-Graph Flowchart**: Connected UI cards linked by tapered `Trim Paths` lines (`Stroke Taper` + `Trim Paths End 0 -> 100%`), fading/sharpening in (`Gaussian Blur 40 -> 0` + `Opacity 0 -> 100%`) as a master `Camera_Null` pans and zooms (`200% -> 100%`) between nodes.

## 7. Multi-Stop Glowing Edge UI Button & Inverted-Matte Bloom (`usna8R3DepE`)
Zero-paid-plugin trendy glowing SaaS button / pill edge reveal:
- **Orbiting Rainbow Gradient Rim + Tapered `Trim Paths` Whip**:
  - Draw a rounded pill (`ADBE Vector Shape - Rect`, `Roundness: height / 2`) with a crisp **`7px` Stroke** and **`ADBE Vector Stroke Line Cap = 1` (`Butt Cap`)** + subtle **`ADBE Vector Stroke Taper`** (`Start/End Length: 18–22%`, `Start/End Width: 0%`) — **never** `Round Cap` on inverted-matte Trim paths (round caps create ugly circular hook blobs past the matte!).
  - Add `ADBE Vector Filter - Trim`, key **`End: 0% -> 100%`** + **`Offset: -150° -> +60°`** over `~1.7s` with asymmetric Speed Graph (`setSmoothCurve(prop, 88, 18)`), and color with orbiting **`ADBE 4ColorGradient`** stops mixing high-luminance Emerald Green, Amber-Orange, Electric Cyan alongside lower-luminance Deep Cobalt so glow blooms in organic localized color clouds rather than a uniform ring.
- **Anti-Blowout 4x Stacked Native `ADBE Glo2` Cascade**:
  - Stack 4 progressive **`ADBE Glo2`** effects (`Radius: 12 -> 42 -> 110 -> 220`, `Threshold: 55 -> 48 -> 42 -> 35%`), **strictly keeping `Glow Intensity` restrained (`0.42 -> 0.36 -> 0.30 -> 0.24`)** and **`Composite Original = 2` (`Behind`)**. *(High `1.0+` intensities stacked 4x clip all RGB channels to blown-out white!)*
- **`Alpha Inverted` Core Occlusion (`TrackMatteType.ALPHA_INVERTED`)**:
  - Duplicate the exact-size pill (`[PILL_W, PILL_H, PILL_R]`) as a solid fill-only matte (`Stroke OFF`, `Fill ON`, no glow/trim) and attach it via **`strokeLayer.setTrackMatte(matteLayer, TrackMatteType.ALPHA_INVERTED)`** over a dark `#06070B` pill backing (do **not** place a dull static grey wireframe ring over the unlit card!).
- **Dual-Layer Staggered Text Color-Flash Wipe**:
  - Create two identical `"Edge Glow"` text layers (`inPoint ~0.55s`) with a sweeping **`ADBE Mask Feather` (`~155px`)** ellipse mask wiping across the pill: magenta/orange tint layer `4–5` frames ahead of the top `#FFFFFF` layer for a luminous colored leading edge.
