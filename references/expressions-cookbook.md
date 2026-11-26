# After Effects Expressions Cookbook (Learned from Ukramedia Playlist `PL1gEYuY5m72bYvZS7iNRXDWtMT2-2-5bI`)

Drop-in, JavaScript-engine-safe (`thisLayer`, `if/else` complete) expressions for SaaS UI, 3D rigs, and data-driven motion graphics.

## 1. Zero-Keyframe Fade In & Out (`inPoint` / `outPoint`)
Automatically fades any layer in and out based on its timeline trim (`inPoint` / `outPoint`):
```javascript
var fade = 0.30; // seconds
ease(time, inPoint, inPoint + fade, 0, 100) - ease(time, outPoint - fade, outPoint, 0, 100);
```

## 2. Auto-Fit Text / Layer Inside Max Width (`sourceRectAtTime`)
Prevents dynamic or MOGRT text from overflowing a pill, badge, or card (`maxW` in px):
```javascript
var maxW = 420;
var w = sourceRectAtTime(time, false).width;
var s = (w > maxW && w > 0) ? (maxW / w) * 100 : 100;
[s, s];
```

## 3. Maintain Child Scale When Parent Scales
Keep a child badge/label at constant visual size even when its parent Null zooms:
```javascript
var p = parent.transform.scale;
[value[0] * 100 / p[0], value[1] * 100 / p[1]];
```

## 4. Dynamic `createPath()` Connector Line Between Moving Layers
Apply to a Shape Layer's **Path** property so the line stays glued to moving/floating UI nodes:
```javascript
var a = thisComp.layer("Node_A");
var b = thisComp.layer("Node_B");
var p1 = fromComp(a.toComp(a.anchorPoint));
var p2 = fromComp(b.toComp(b.anchorPoint));
createPath([[p1[0], p1[1]], [p2[0], p2[1]]], [], [], false); // 2D slice required even for 3D layers!
```

## 5. Lock Gradient Ramp (`ADBE Ramp`) to Moving/Resizing Layer
Apply to **Start of Ramp** and **End of Ramp** so gradients move with the card:
```javascript
// Start of Ramp:
var r = sourceRectAtTime(time, false);
toComp([r.left, r.top]);

// End of Ramp:
var r = sourceRectAtTime(time, false);
toComp([r.left + r.width, r.top + r.height]);
```

## 6. Universal Comma Formatter for Rolling Counters
Works for any digit count (4, 5, 6, 7+ digits) on `Source Text`:
```javascript
var val = Math.round(linear(time, 0.5, 2.5, 0, 128400));
"$" + val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
```

## 7. Marker-Triggered Text Switching
Change `Source Text` automatically whenever the playhead passes a Layer Marker whose comment holds the new string:
```javascript
var m = thisLayer.marker;
if (m.numKeys > 0) {
    var k = m.nearestKey(time).index;
    if (m.key(k).time > time) k--;
    (k > 0 && m.key(k).comment !== "") ? m.key(k).comment : value;
} else {
    value;
}
```

## 8. 3D Carousel Positioning (`N` Cards in a Ring)
Arrange `N` 3D cards evenly around a `radius` circle driven by a Master Null's `Y Rotation` or `Slider`:
```javascript
var idx = 0; // 0..N-1
var total = 6;
var radius = 620;
var baseAngle = degreesToRadians((idx * 360 / total) + time * 18);
[960 + Math.cos(baseAngle) * radius, 540, Math.sin(baseAngle) * radius];
```

## 9. Voiceover / Audio-Reactive Bar Height
Drive Shape `Rect Size` height from `Convert Audio to Keyframes` (`Audio Amplitude`):
```javascript
var amp = thisComp.layer("Audio Amplitude").effect("Both Channels")("Slider");
var h = linear(amp, 0, 40, 16, 180);
[value[0], h];
```

## 10. Auto-Contrast Text Color (`sampleImage`)
Automatically switch text between dark and white based on background luminance behind it:
```javascript
var bg = thisComp.layer("BG_Layer");
var lum = rgbToHsl(bg.sampleImage(toComp(anchorPoint), [20, 20]))[2];
lum > 0.55 ? [0.06, 0.07, 0.1, 1] : [0.97, 0.98, 0.99, 1];
```

## 11. Squash & Stretch Pop (Cinecom — Area-Preserving Icon/Badge Elasticity)
Apply to `Scale` on icons, badges, or bullet points for lively elastic distortion on `inPoint`:
```javascript
var maxDev = 13, spd = 30, decay = 4.0;
var t = time - inPoint;
var x = scale[0] + maxDev * Math.sin(spd * t) / Math.exp(decay * t);
var y = scale[0] * scale[1] / x;
[x, y];
```

## 12. Motion Tail / Index-Offset Follow-Through (Cinecom)
Apply to `Position` on stacked duplicate layers so each copy trails `layer(1)` by `delay` frames:
```javascript
var delay = 5; // frames per layer
var d = delay * thisComp.frameDuration * (index - 1);
thisComp.layer(1).position.valueAtTime(time - d);
```

## 13. Fixed-Amplitude Rest Bounce (Velocity-Independent)
Use when Easy Ease keyframes have low incoming velocity or you want a guaranteed uniform bounce on `Scale` (`value + [n, n]`) after the final keyframe:
```javascript
var amp = 50, freq = 2, decay = 5, n = 0;
if (numKeys > 0 && time > key(numKeys).time) {
    var t = time - key(numKeys).time;
    n = Math.sin(freq * t * Math.PI * 2) * amp * Math.exp(-decay * t);
}
value + [n, n];
```

---

## AEExpressions.com Full Reference Catalog (`aeexpressions.com`)

### 14. Exponential Scale (Maps `0–100%` Keyframes to True Exponential Zoom)
```javascript
var s = Math.exp(transform.scale[0] / 21.667905) - 1;
[s, s];
```

### 15. Auto Fade In & Out with 2 Markers
```javascript
if (marker.numKeys < 2) { value; } else {
    var fInS = inPoint, fInE = marker.key(1).time;
    var fOutS = marker.key(2).time, fOutE = outPoint;
    if (time < fInS) { 0; }
    else if (time <= fInE) { linear(time, fInS, fInE, 0, 100); }
    else if (time <= fOutS) { 100; }
    else if (time <= fOutE) { linear(time, fOutS, fOutE, 100, 0); }
    else { 0; }
}
```

### 16. Single-Layer Typewriter with Blinking Cursor (`Source Text`)
```javascript
var txt = value, speed = 10, cursor = "|", blink = true, keepBlinking = true;
var charsShown = Math.floor((time - inPoint) * speed);
var typed = txt.substr(0, charsShown);
var blinkingState = Math.floor(time * 2) % 2 == 0;
var cursorVisible = blink ? blinkingState : (charsShown < txt.length);
var showCursor = keepBlinking ? cursorVisible : (charsShown < txt.length ? cursorVisible : false);
typed + (showCursor ? cursor : "");
```

### 17. Seamless Looping Wiggle (`Wiggle Loop`)
```javascript
var wps = 2, amp = 40, loopTime = 3;
var cur = time % loopTime;
var start = wiggle(wps, amp, 1, 0.5, cur);
var end = wiggle(wps, amp, 1, 0.5, cur - loopTime);
linear(cur, 0, loopTime, start, end);
```

### 18. 1D Wiggle (`X`, `Y`, or `Z` Only)
```javascript
var w = wiggle(3, 50);
[w[0], value[1]]; // X-only (use [value[0], w[1]] for Y, or [value[0], value[1], w[2]] for Z)
```

### 19. Stop-Motion / Posterized Wiggle & Pulse Opacity
```javascript
// Stop-motion choppy motion (7 fps)
posterizeTime(7); wiggle(7, 50);

// Smooth pulsing opacity (30% <-> 80%)
var wave = Math.abs(Math.sin(time * 1 * Math.PI));
30 + wave * (80 - 30);
```

### 20. Time-Windowed Expression (`startTime` to `stopTime`)
```javascript
var startTime = 2, stopTime = 5;
(time >= startTime && time <= stopTime) ? wiggle(5, 50) : value;
```

### 21. Stepped Random Scale (`seedRandom` every 3 frames) & Flicker
```javascript
// Random uniform scale every 3 frames
var frame = Math.round(time / thisComp.frameDuration);
seedRandom(Math.floor(frame / 3), true);
var s = random(50, 150);
[s, s];

// Rhythmic Opacity Flicker (10-frame period)
var n = Math.sin(time / thisComp.frameDuration * (1 / 10) * Math.PI);
n <= 0 ? 0 : value;
```

### 22. Digital Clock (`HH:MM:SS`), Loops & Dropdown Menu Switch
```javascript
// Digital Clock (Source Text)
var t = time, h = Math.floor(t/3600), m = Math.floor((t%3600)/60), s = Math.floor(t%60);
(h<10?"0"+h:h) + ":" + (m<10?"0"+m:m) + ":" + (s<10?"0"+s:s);

// Loop Modes: loopOut("cycle") | loopOut("pingpong") | loopOut("offset") | loopOut("continue") | loopIn("cycle")

// Dropdown Menu Control
var ctrl = effect("Dropdown Menu Control")("Menu").value;
if (ctrl == 1) { [100, 100]; } else if (ctrl == 2) { [500, 200]; } else { [300, 500]; }
```

---

## Motion Design School Expressions Library (`tools.motiondesign.school/expressions`)

### 23. Elastic Bounce for Text Animators (Expression Selector `Amount`)
```javascript
var freq = 2, decay = 4, delay = 0.05, dur = 0.12;
var myDelay = (textIndex - 1) * delay;
var t = time - (inPoint + myDelay);
var startVal = 100, endVal = 0;
if (t < dur) {
    linear(t, 0, dur, startVal, endVal);
} else {
    var amp = (endVal - startVal) / dur;
    var w = freq * Math.PI * 2;
    endVal + amp * (Math.sin(t * w) / Math.exp(decay * t) / w);
}
```

### 24. 3D Hide Backface & Flip Backface (`toCompVec`)
Prevents 3D cards or 3D kinetic text from rendering mirrored when rotated past 90° away from camera:
```javascript
// Hide Backface (apply to Opacity):
toCompVec([0, 0, 1])[2] > 0 ? value : 0;

// Flip Backface (apply to Scale):
toCompVec([0, 0, 1])[2] > 0 ? value : [-value[0], value[1], value[2]];
```

### 25. 3D Distance Fade from Active Camera (`Opacity`)
```javascript
var startFade = 500, endFade = 1500;
var C = thisComp.activeCamera.toWorld([0, 0, 0]);
var d = length(C, toWorld(anchorPoint));
linear(d, startFade, endFade, 100, 0);
```

### 26. Squash/Wobble Pop at Every Layer Marker (`Scale`)
```javascript
var n = 0, t = 0;
if (marker.numKeys > 0) {
    n = marker.nearestKey(time).index;
    if (marker.key(n).time > time) n--;
}
if (n > 0) t = time - marker.key(n).time;
var amp = 15, freq = 5, decay = 3.0;
var scaleFact = (100 + amp * Math.sin(freq * 2 * Math.PI * t) / Math.exp(decay * t)) / 100;
[value[0] * scaleFact, value[1] / scaleFact];
```

### 27. Re-Trigger Keyframed Animation at Every Marker
```javascript
var n = marker.nearestKey(time).index;
if (time < marker.key(n).time) n--;
if (n == 0) n = 1;
var t = time - marker.key(n).time;
thisProperty.valueAtTime(t);
```

### 28. Parent Position Delay (`framesToTime`), Noise Drift & Index Stack
```javascript
// Parent Position Delay (5 frames behind parent):
var delay = 5;
parent.fromComp(toComp(anchorPoint, time - framesToTime(delay)));

// Smooth Organic Noise Drift (apply to Position/Rotation):
noise((time + 0 * thisComp.frameDuration) * 2) * 60;

// Index Stack Offset (120px vertical list):
value + [0, 120 * (index - 1)];
```

### 29. Locale-Safe `CONTROLS` Rig & Layer Control Pattern (`IMT / aedev`)
```javascript
// Locale-independent effect parameter access (use index (1) or matchName, avoids translated "Slider"/"Color" names!)
var ctrl = thisComp.layer("CONTROLS");
var spd  = ctrl.effect("Speed")(1);     // Slider Control value
var tint = ctrl.effect("Accent")(1);    // [r, g, b, a] Color Control
var refL = ctrl.effect("Target")(1);    // Layer Control returns a Layer object in expressions (even though set via integer index in ES3!)
refL.transform.position;
```

### 30. Pre-Expression vs Post-Expression Value Read (ExtendScript `addKineticOutro` Safety)
```javascript
// Pass true for 2nd arg to read raw keyframed/static value (ignores wiggle/hover expressions!)
var rawRestPos = posProp.valueAtTime(tOutStart, true);
var computedPos = posProp.valueAtTime(tOutStart, false);
```

### 31. Safe Expression Setter & Error Audit (ES3 ExtendScript)
```javascript
// Expression errors NEVER throw in ExtendScript! Always force eval and check expressionError:
if (prop.canSetExpression) {
    prop.expression = exprStr;
    prop.valueAtTime(0, false); // force evaluation
    if (prop.expressionError !== "") {
        $.writeln("Expression error: " + prop.expressionError);
    }
}
```

### 32. `Expression Selector` Bounce-In by Word/Char Index (`8pPk9c7eSM4`)
Apply to **`ADBE Text Expressible Selector` > `Amount` (`ADBE Text Expressible Amount`)** with Animator `Position = [0, 120, 0]`, `Opacity = 0`, optional `Fill Color = Accent`, and `Based On = Words (3)` (`ADBE Text Range Type2`). Handles 1D/2D/3D `Amount` cleanly:
```javascript
// Dimension-safe damped cosine word bounce-in
var delay = 0.08;
var freq = 2;
var decay = 5;
var t = time - inPoint - (textIndex - 1) * delay;
var amt = 100;
if (t >= 0) {
    amt = (Math.cos(freq * t * 2 * Math.PI) / Math.exp(decay * t)) * 100;
}
if (value.length == 3) { [amt, amt, amt]; } else if (value.length == 2) { [amt, amt]; } else { amt; }
```

### 33. Zero-Keyframe `Source Text` Typewriter + Cursor Blink (`8pPk9c7eSM4`)
Apply directly to `Source Text` (`ADBE Text Document`) — preserves styled text content via `value.toString()`, zero keyframes needed:
```javascript
var charsPerSecond = 14;
var cursorBlinksPerSecond = 2;
var cursorSymbol = "|";
var fullText = value.toString();
var elapsedTime = Math.max(time - inPoint, 0);
var visibleChars = Math.min(Math.floor(elapsedTime * charsPerSecond), fullText.length);
var showCursor = (Math.floor(time * cursorBlinksPerSecond) % 2 === 0);
var outStr = fullText.substr(0, visibleChars);
if (visibleChars < fullText.length || showCursor) { outStr = outStr + cursorSymbol; }
outStr;
```
