---
name: hi-fi-extract
description: Extracts structured inspiration data from a URL and adds it to your hi-fi REPO.md. Invoke when the user says "/hi-fi-extract <url>", "extract this URL", "add this to my hi-fi repo", "add to hi-fi repo", or when the hi-fi skill offers to save a web fallback result. The skill fetches the page, analyzes it with Claude, shows a proposed REPO.md entry for review, then writes it on confirmation.
---

# hi-fi Extract

This skill takes a URL, analyzes the page, and produces a structured entry for your hi-fi inspiration repo (`~/.claude/skills/hi-fi/REPO.md`).

## Workflow

### 1. Get the URL

Use the URL provided inline with `/hi-fi-extract`. If no URL was given, ask:
> "What URL do you want to extract?"

### 2. Fetch the page

Use the **WebFetch** tool to retrieve the page content. Get as much text as possible — code, description, title tags, any README or about text on the page.

If the page returns an error or requires login, report:
> "I couldn't fetch that page (it may require a login or is otherwise inaccessible). You can still add it manually — paste the page title and a brief description and I'll format the entry."

### 3. Screenshot (optional)

If the **browser MCP** (`mcp__Claude_in_Chrome__navigate`) is available:
1. Navigate to the URL
2. Take a screenshot with `mcp__Claude_in_Chrome__computer`

Use this to enrich the visual description. If the browser MCP isn't available, skip silently — the text content is usually enough.

### 4. Extract metadata

From the page text (and screenshot if available), extract the following:

**`title`** — A concise, descriptive name for this reference. Not the full page title — just a short identifying phrase (e.g. "Boids Flocking Simulation", not "p5.js — Boids by Daniel Shiffman").

**`description`** — 2–3 sentences. Cover:
- What it is (visual output, subject matter)
- How it works technically (algorithm, data structure, rendering approach)
- What makes it distinct or interesting as a reference

Write as if explaining to an LLM agent that will use this description to decide whether this reference is relevant to a UI prompt. Be specific and technical.

**`categories`** — One or more from this fixed list only:
- `data-viz` — charts, graphs, visualizations of data
- `illustration` — static or decorative graphics, icons, generative art
- `animation` — motion, transitions, time-based effects
- `page-layout` — structure, composition, typographic layout, navigation patterns
- `UI-pattern` — interactive components, forms, controls, micro-interactions

Assign the most accurate category. Multiple categories allowed if genuinely applicable.

**`tags`** — 8–15 freeform lowercase tags. Cover:
- Technical technique (e.g. `canvas`, `webgl`, `svg`, `css-grid`)
- Visual style (e.g. `minimal`, `dense`, `particle`, `fluid`, `geometric`)
- Interaction model (e.g. `hover`, `drag`, `real-time`, `static`)
- Data characteristics (e.g. `time-series`, `hierarchical`, `network`, `geospatial`)
- Any relevant domain (e.g. `d3`, `three-js`, `p5`, `glsl`)

**`glossary`** — 4–8 key technical terms from this specific reference that an LLM would need to understand in order to replicate or apply the technique. For each term, write a tight 1-sentence definition. Focus on terms that are:
- Domain-specific (not general programming terms)
- Actually used in this reference (not generic)
- Useful for disambiguating or directing an AI agent

Example format:
```
- *boids*: autonomous agents that simulate flocking through local per-agent rules only
- *Reynolds rules*: Craig Reynolds' 1987 model — separation (avoid crowding), alignment (match heading), cohesion (move toward center)
- *steering force*: a velocity adjustment vector applied each frame to nudge an agent toward a target behavior
```

### 5. Show the proposed entry

Display the full REPO.md block in a fenced code block for review:

````
```
---
id: <generate a short slug from the title, e.g. "boids-flocking-simulation">
title: <title>
source_url: <url>
thumbnail_url:
categories: [<categories>]
tags: [<tags>]
added: <today's date YYYY-MM-DD>
---

**Description:** <description>

**Glossary:**
<glossary bullet list>

---
```
````

Then ask:
> "Does this look right? Say 'save it' to add to your repo, or tell me what to change."

Note: `thumbnail_url` is intentionally left blank. If you already have a thumbnail URL, paste it in before confirming.

### 6. Write to REPO.md on confirmation

When confirmed (e.g. "save it", "looks good", "yes", "ship it"):

1. Read `~/.claude/skills/hi-fi/REPO.md`
2. Find the `## Items` section (the last section in the file)
3. Append the new entry after the existing items, before the final comment if any
4. Write the file back

Report: "Added **[title]** to your hi-fi repo. You now have N items."

Then auto-commit and push:
```bash
cd ~/hi-fi-skills && git add hi-fi/REPO.md && git commit -m "add: [title]" && git push 2>&1
```

- If it succeeds: report "Pushed to the shared repo ✓"
- If the push fails (no remote, auth error, etc.): report "Saved locally. To share: `cd ~/hi-fi-skills && git push`"
- If `~/hi-fi-skills` isn't a git repo: skip silently

---

## Notes

- The slug (used as `id`) should be kebab-case, derived from the title, lowercase, max ~5 words. If the same slug already exists in REPO.md, append `-2`, `-3`, etc.
- Don't fabricate glossary terms. If the page is thin on technical content, 2–3 terms is fine.
- If the URL is a CodePen, Observable notebook, or similar live demo, prioritize understanding the *technique* (what algorithm or visual approach is being demonstrated) over the *framework* (don't just say "uses p5.js").
