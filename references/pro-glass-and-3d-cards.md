# Pro Glass & 3D Card Finishing Architecture

Synthesized from 9 After Effects tutorials (`XIO_Dv6_Ujk`, `bj28UpR_5Wc`, `qobkYjVL8Og`, `_nEXlqlzZt4`, `lpGg1evgmk4`, `NqYdA2KXkd4`, `DDrt1DQyiKU`, `tYjqtArZtds`, `cPBRUdTOEZ0`).

## 1. Why Cards Look "Finished" (The 4-Layer Glass Card Stack)
Never ship a flat single-layer rounded rectangle with a uniform 1.5px stroke. Always use `addGlassCardEffect()` (or build this exact 4-layer stack):
1. **Shape-Adjustment Frosted Blur (`Zuyns tYjqtArZtds` & `Zammit qobkYjVL8Og`)**:
   - Create a rounded-rectangle Shape Layer (`card_FrostBlur`) linked via expression to the card's `size` & `roundness`, set **`layer.adjustmentLayer = true`**, and apply `ADBE Box Blur2` (`Radius: 28–35px`, `Repeat Edge Pixels = true`).
   - Because it is a Shape Layer set to Adjustment Layer, AE blurs **only inside the rounded corners**—no extra Track Matte needed, and it works in both 2D and 3D!
2. **Translucent Depth Fill (`10–28%` Opacity + `ADBE 4ColorGradient`)**:
   - Keep the card surface translucent (`10–28%` on light studio backgrounds, `22–38%` on dark backgrounds) with a corner-locked `ADBE 4ColorGradient` so the frosted backdrop visibly refracts through the card.
3. **Specular Rim + Rotating `CC Light Sweep` (`Emmanuel XIO_Dv6_Ujk` & `Motion Nations NqYdA2KXkd4`)**:
   - Place a dedicated stroke-only layer (`card_SpecularRim`, `2px` bright white `50–75%` opacity) on top, with `CC Light Sweep` whose **`Direction` rotates smoothly (`time * 55`)** (or `Bevel & Emboss` Layer Style at `Fill Opacity = 0%`) so a crisp metallic highlight glides along the beveled border.
4. **Soft Multi-Layer Drop Shadow (`Zammit DDrt1DQyiKU`)**:
   - Use wide, low-opacity `ADBE Drop Shadow` (`Opacity: 25–35%`, `Distance: 20–35`, `Softness: 130–250`) or dual colored shadows so cards float naturally with ambient depth.

## 2. Textured Glass & Rotating Gradient Strokes (`Stephan Zammit DDrt1DQyiKU` / `Motion Nations NqYdA2KXkd4`)
- **Textured Glass Refraction**: Add `CC Vector Blur` (`Type: Direction Fading`, `Amount: 80`, `Revolutions: 0.8`) + `Curves` + `CC Composite` (`Stencil Alpha`) inside a dark/colored card for organic brushed-glass refraction.
- **Rotating Tapered Rim Glint (`NqYdA2KXkd4` & `DDrt1DQyiKU`)**:
  - Method A (`NqYdA2KXkd4`): Duplicate a stroked card, enable **`Stroke Taper` (`End Length: 100%`, Round Cap)**, shorten **`Trim Paths End` (`25–35%`)**, and animate **`Trim Paths Offset`** with stacked `Glow` (`44px`, `200px`, `800px` in `Add` mode) for a laser-like light racing around the border.
  - Method B (`DDrt1DQyiKU`): Stack a tapered white stroke over a black stroke -> `Fast Box Blur` (`half of stroke width`) -> `CC Composite (Stencil Alpha)` -> `Levels (Alpha squeeze)` -> `Tritone` (Cyan / Coral / Blue) -> `Trim Paths Offset` so a multi-color gradient stroke actually **rotates along the path**.

## 3. True 3D Cards, Parallax Windows & Carousels (`_nEXlqlzZt4`, `qobkYjVL8Og`, `bj28UpR_5Wc`, `lpGg1evgmk4`)
- **3D Window Parallax (`PremiumBeat _nEXlqlzZt4`)**:
  - Put inner 3D elements (icons, floating particles, charts) inside an `Inner_Window_Comp` at `Z = +100, +200, +300`, clip it to a rounded window (`TrackMatteType.ALPHA`), and enable **`collapseTransformation = true`** so tilting the 3D card reveals deep interior parallax!
  - Scale `card_back` to `99%` behind the front rim to prevent 3D Z-fighting/moiré edges.
- **3D Extruded Glass Thickness (`Emmanuel` & `Zammit`)**:
  - In `Advanced 3D` (or Z-stack in Classic 3D), set `Bevel Style: Convex`, `Bevel Depth: 1–4`, `Extrusion Depth: 5–15`, `Specular Shininess: 85–90%`, lit by soft `Point Lights` (`Radius: 500`, `Falloff: 2000`) positioned to graze the card edges.
- **3D Carousel + Reflection (`SonduckFilm bj28UpR_5Wc` & `The Poser lpGg1evgmk4`)**:
  - Place `N` Pre-Comped 3D cards at `Z = -500`, rotate the center Null by `360 / N` degrees (`60°` for 6 cards, `120°` for 3 cards) per card, and animate `Y Rotation` with `loopOut("offset")`.
  - Duplicate the carousel Null + cards as a Reflection Pre-Comp (`X/Z Rotation = 180°`, `Compound Blur` / `Gaussian Blur`, `Linear Wipe` fade) beneath the floor.
- **Interactive 3D Hover Effector (`Zammit qobkYjVL8Og`)**:
  - Use a `Pointer_Effector_Null` and drive each card's Z offset via `linear(length(toWorld(anchorPoint), effector.toWorld([0,0,0])), 0, 260, -45, 0)` so whichever card the cursor passes over lifts forward in 3D space!

## 4. 3D Floating UI Carousel & Tracked Stage Rig (`The Motion Designers cPBRUdTOEZ0`)
When creating floating 3D UI carousels (either tracked over live footage via `Tracker > Track Motion -> Tracking_Information_Null` or over a floating kinetic stage):
1. **High-Res Portrait Card Pre-Comps (`1080x1920`, scaled `50%`)**:
   - Build each card inside its own `Card_N_PreComp` (`1080x1920` or matching aspect) clipped with an inner rounded-rectangle Shape `TrackMatteType.ALPHA` matte and 2D `addGlassCardEffect()` finishing, then place all `N` pre-comps in the main 3D comp at `50%` scale so typography and glass edges remain razor-sharp even near the lens.
2. **Daisy-Chain `Z = +500` Cylindrical Rig (`setup3DCarouselRing()`)**:
   - Enable `threeDLayer = true` on `Rotation_Controller_Null` and all `N` card layers.
   - Set `Rotation_Controller_Null` Position to `[centerX, centerY, +500]` (leaving cards at `[centerX, centerY, 0]` so the pivot sits `500px` inside the ring).
   - Step-parent cards one by one: parent `Card_1`, add `360 / N` degrees (`60°` for 6 cards) to `Rotation_Controller` `Y Rotation`, parent `Card_2`, add `+60°`, ..., and parent the final card `Card_N` **without** rotating further so it starts centered in front.
   - Parent `Rotation_Controller_Null` to `Tracking_Information_Null` (tracked footage null or organically hovering 3D stage null).
3. **Gesture-Synced Whip Rotation + 5-Frame Staggered Fan-Up + Motion Blur**:
   - Animate `Rotation_Controller` `Y Rotation` in `360 / N` (`60°`) whip steps synced to pointer/hand gestures or VO markers, with Easy Ease + strong Speed Graph (`85%+` influence) and an initial spin-in from frame 0.
   - Stagger each card's rising pop-in (`Position Y` slide-up + `Opacity 0 -> 100%`) by **`5` frames** (`5 * comp.frameDuration`) so the carousel fans up in a crisp wave.
   - Enable **`comp.motionBlur = true`** and **`layer.motionBlur = true`** on every 3D card layer and card pre-comp for realistic rotational smear.
