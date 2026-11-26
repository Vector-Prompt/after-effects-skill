# After Effects AI Agent Skill (`after-effects`)

> **Automated Adobe After Effects ExtendScript (ES3) motion-graphics skill for AI coding agents** (Antigravity, Claude Code, Cursor, Gemini CLI, etc.) — maintained by **[Vector & Prompt](https://github.com/Vector-Prompt)**.

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)

## Overview

This skill teaches AI agents how to write **production-ready, error-free ExtendScript (`.jsx`)** for **Adobe After Effects 2023–2026+** (`60fps`, strict ECMAScript 3), complete with:

- **Voiceover-Ready Marker PreComps** (`Anim_PreComp` inside `Main_Voiceover_Sync` with draggable Layer Markers)
- **Symmetric Kinetic Animate-In & Animate-Out** (`addKineticOutro()` with pre-expression `valueAtTime`)
- **Asymmetric Speed-Graph Curves & Physics Expressions** (`setSmoothCurve()`, `bounceExpr()`, Squash & Stretch, sinusoidal float, `createPath()` dynamic connectors, `sourceRectAtTime` auto-fit)
- **Pro Glassmorphism, Liquid Glass & Apple UI Motion Patterns** (4-layer frosted glass cards, rotating specular rims, Dynamic Island morphing, 3D hover lift)
- **3D Camera Rigging & Kinetic Typography** (exact `z = -zoom` frustum math, `3840x2160` 4K background orbit guard, 90° 3D corner-spine typography, brutalist kinetic typography)
- **Zero-Crash ES3 Guardrails** (strict `var` / no reserved words / `safeSetExpr()` + `auditCompExpressions()` validation)

---

## Repository Structure

```text
after-effects-skill/
├── SKILL.md                                   # Main AI Agent Skill definition (v2.0.0)
├── NOTICE.md                                  # Open-source license & study attributions
├── LICENSE                                    # Apache-2.0 License
├── README.md                                  # Setup & usage guide
├── references/
│   ├── channel-standards.md                   # ES3 strict rules, QA, typography & architecture
│   ├── pro-glass-and-3d-cards.md              # Frosted glass, 3D carousels & specular rims
│   ├── apple-ui-patterns.md                   # Liquid glass, Dynamic Island, Face ID & SaaS UI
│   ├── 3d-kinetic-and-camera.md               # 3D camera frustum math & kinetic typography
│   └── expressions-cookbook.md                # 30+ battle-tested AE expressions
├── helpers/
│   └── _channel_helpers.jsxinc                # Shared ES3 ExtendScript helper library
└── templates/
    └── AE_ANIMATION_PROMPT_TEMPLATE.txt       # Ready-to-use prompt template for screenshots
```

---

## Quick Installation

### 1. Install the Skill
Copy `SKILL.md`, `NOTICE.md`, and the `references/` folder into your project's (or global) skills directory:

- **Project-level (Antigravity / universal `.agents`):**
  ```text
  <your-project>/.agents/skills/after-effects/
  ├── SKILL.md
  ├── NOTICE.md
  └── references/
  ```
- **Global (Antigravity):**
  `~/.gemini/antigravity/skills/after-effects/`

### 2. Install the Shared ES3 Helper Library
Copy `helpers/_channel_helpers.jsxinc` into your After Effects scripts output folder:
```text
<your-project>/ae_templates/_channel_helpers.jsxinc
```
Generated scripts inside `ae_templates/<Category>/` automatically include it via `//@include "../_channel_helpers.jsxinc"`.

### 3. Generate an Animation
1. Attach a UI/design screenshot or describe the motion you want.
2. Optionally fill in `templates/AE_ANIMATION_PROMPT_TEMPLATE.txt`.
3. Ask your AI agent to generate the `.jsx` script using the `after-effects` skill.
4. In Adobe After Effects: enable **Settings > Scripting & Expressions > Allow Scripts to Write Files and Access Network**, then run via **File > Scripts > Run Script File...**.

---

## License & Attribution

- Licensed under the **Apache License 2.0** — see [LICENSE](LICENSE).
- Originally based on [`terminal-skills/after-effects`](https://terminalskills.io/skills/after-effects) (Apache-2.0) and extended with custom ExtendScript architecture, 3D camera math, and motion-design rules by **Vector & Prompt**.
- See [NOTICE.md](NOTICE.md) for full open-source repository attributions (`Apache-2.0` / `MIT`) and creator study references.

> **Disclaimer:** Adobe After Effects is commercial software owned by Adobe Inc. This repository is an independent open-source workflow and scripting skill and is not affiliated with or endorsed by Adobe Inc.
