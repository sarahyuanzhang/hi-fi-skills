# hi-fi Inspiration Repo

This file is the canonical inspiration catalog for the `/hi-fi` skill. Each entry is a visual or technical reference that gets matched against UI-building prompts to provide concrete, grounded suggestions.

**Do not edit manually during a skill run.** Use `/hi-fi-extract <url>` to add new items, or `/hi-fi sync` to pull from a hi-fi-me server instance.

---

## Format reference

Each item uses this structure:

```
---
id: <uuid-or-slug>
title: <concise reference name>
source_url: <url or omit if none>
thumbnail_url: <blob URL for image preview>
categories: [<one or more of: data-viz, illustration, animation, page-layout, UI-pattern>]
tags: [<8-15 freeform lowercase tags>]
added: <YYYY-MM-DD>
---

**Description:** <2-3 sentences. What it is, how it works visually/technically.>

**Glossary:**
- *term*: definition useful to an LLM agent reading this as context
- *term*: definition

**Comments:** *(optional — omit section if empty)*
- [note on when/how to use this reference, e.g. "good for onboarding flows with progressive disclosure"]

---
```

Items are separated by `---` horizontal rules.

## Items

---
id: c8f0c87c-6872-4433-bb50-afd364329daa
title: Procedural Noise Texture Generation Tutorial
source_url: https://thebookofshaders.com/11/
thumbnail_url: https://pvyjytrh7lmsz0vn.private.blob.vercel-storage.com/inspo-1777643811676.png
categories: [data-viz, illustration, animation]
tags: [single-column-layout, editorial-layout, monochrome-palette, smooth-interpolation, procedural-texture, organic-animation, continuous-signal, noise-function, time-series, anomaly-highlight, real-time-updates, shader-rendering, data-density-low, gradient-transition]
added: 2026-05-01
---

**Description:** This page from 'The Book of Shaders' demonstrates how to generate smooth, organic-looking noise using value noise techniques, covering 1D and 2D interpolation between random values using smoothstep and cubic curves. For Datadog product design, procedural noise concepts could inform smooth metric anomaly visualizations, organic-feeling background textures for dashboards, or animated transitions in time-series charts that avoid jarring linear interpolation artifacts.

**Glossary:**
- *Perlin noise*: 
- *value noise*: 
- *smoothstep*: 
- *GLSL*: 
- *fract*: 
- *cubic interpolation*: 
- *rand()*: 
- *2D noise*: 
- *bilinear interpolation*: 
- *procedural generation*: 

---

---
id: 796ffd3b-b8a3-4e47-9a61-2cd0e55f2848
title: GLSL Shaping Functions for Data Curves
source_url: https://thebookofshaders.com/05/
thumbnail_url: https://pvyjytrh7lmsz0vn.private.blob.vercel-storage.com/inspo-1777643801005.png
categories: [data-viz, illustration, animation]
tags: [threshold-line, smoothstep-interpolation, gradient-mapping, normalized-value-space, curve-shaping, monochrome-palette, single-axis-plot, anomaly-highlight, power-function-curve, shader-canvas, data-density-low, real-time-updates, inline-code-edit, easing-function]
added: 2026-05-01
---

**Description:** This page demonstrates how mathematical shaping functions (pow, step, smoothstep) can transform linear values into expressive curves using GLSL shaders, visualized as gradient-to-line mappings in a normalized 0.0–1.0 space. For Datadog product design, these techniques directly apply to animating metric transitions, rendering smooth threshold indicators on time-series charts, and creating visually meaningful easing curves for anomaly highlights or alert state changes.

**Glossary:**
- *GLSL*: 
- *smoothstep*: 
- *step function*: 
- *linear interpolation*: 
- *pow()*: 
- *fragment shader*: 
- *normalized coordinate space*: 
- *hardware-accelerated functions*: 
- *vec3 constructor*: 
- *shaping functions*: 

---

---
id: 6d35f881-5e81-4b8d-affa-726fa4cb7213
title: GLSL Shader Tile Pattern Generation
source_url: https://thebookofshaders.com/09/
thumbnail_url: https://pvyjytrh7lmsz0vn.private.blob.vercel-storage.com/inspo-1777643791320.png
categories: [data-viz, illustration, animation, page-layout]
tags: [procedural-pattern, tile-grid, fragment-shader, coordinate-transformation, repeating-texture, offset-rows, monochrome-palette, modulo-grid, cell-based-layout, heatmap-grid, matrix-transform, real-time-rendering, data-density-high, animated-pattern]
added: 2026-05-01
---

**Description:** This page demonstrates how fragment shaders use fract() and mod() functions to create repeating tile patterns and offset grids (like brick walls) by manipulating normalized coordinate spaces. In Datadog product design, these techniques could apply to background texture generation for dashboards, animated heatmap cell rendering, or procedural pattern overlays for status indicators and severity visualizations.

**Glossary:**
- *GLSL*: 
- *fract()*: 
- *mod()*: 
- *fragment shader*: 
- *normalized coordinate space*: 
- *tiling*: 
- *UV mapping*: 
- *step()*: 
- *affine transformation*: 
- *domain repetition*: 

---

---
id: 76cc34ba-a769-45bc-8aae-f10024726792
title: Sidebar Navigation with Hierarchical Content Structure
source_url: https://natureofcode.com/autonomous-agents/#path-following
thumbnail_url: https://pvyjytrh7lmsz0vn.private.blob.vercel-storage.com/inspo-1777643781404.png
categories: [page-layout]
tags: [sidebar-navigation, hierarchical-data, sticky-toc, split-pane, long-form-content, nested-menu, chapter-navigation, documentation-layout, breadcrumb-hierarchy, content-anchoring, monochrome-palette, data-density-high, fixed-sidebar, multi-section]
added: 2026-05-01
---

**Description:** This page demonstrates a documentation layout with a collapsible sidebar table of contents alongside long-form technical content, using structured hierarchy to guide readers through complex multi-chapter material. For Datadog product design, this pattern applies directly to documentation portals, runbook layouts, and knowledge base UIs where hierarchical navigation must remain accessible while reading dense technical content.

**Glossary:**
- *table of contents*: 
- *hierarchical navigation*: 
- *anchor links*: 
- *collapsible tree*: 
- *content sectioning*: 
- *progressive disclosure*: 
- *document outline*: 
- *nested list structure*: 
- *sidebar layout*: 
- *information architecture*:

---

---
id: order-vs-chaos-dot-interpolation
title: Order vs. Chaos — Interactive Dot Interpolation
source_url: http://www.generative-gestaltung.de/2/sketches/?02_M/M_1_2_01
thumbnail_url:
categories: [illustration, animation, UI-pattern]
tags: [p5js, lerp, interactive, mouse-driven, circle-layout, random-seed, order-chaos, generative, canvas, particles, deterministic-random, smooth-transition, seeded-random]
added: 2026-05-01
---

**Description:** A p5.js sketch that interpolates 150 dots between a fully random scatter and a perfect circle arrangement, with mouse X position controlling the blend in real time. Uses `lerp()` to smoothly mix two coordinate systems — random (chaos) and evenly-angularly-spaced circular (order) — creating a live slider between entropy and geometry. Click to reshuffle the random seed while preserving the interpolation fader position.

**Glossary:**
- *lerp*: linear interpolation — blends two values by a factor t ∈ [0,1]; used here to mix x/y positions between random and circular targets
- *randomSeed*: a fixed integer passed to p5's random() to make it produce the same sequence every frame, ensuring the "random" layout is stable until clicked
- *faderX*: mouse X normalized to 0–1, used as the t parameter in lerp() — left is pure chaos, right is pure circle
- *angular distribution*: equal spacing of 360/count degrees per dot, placing them evenly around a circle of radius 300
- *order-chaos duality*: a compositional technique where a single continuous parameter moves between structured and entropic arrangements

---
