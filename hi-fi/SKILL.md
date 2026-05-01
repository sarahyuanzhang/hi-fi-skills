---
name: hi-fi
description: Enhances a UI-building prompt with visual inspiration references from your hi-fi inspiration repo. Invoke when the user says "hi-fi my prompt", "hi-fi this", "enhance this prompt with inspiration", "add visual references", "/hi-fi", or provides a UI design prompt and wants to enrich it with concrete references. Also trigger when the user is about to send a prompt to an AI agent for building UI and wants to inject relevant visual/code references first. The skill reads from a local REPO.md file and returns a numbered list of suggestions plus the full annotated prompt. Also handles subcommands: "/hi-fi show" (open gallery in browser), "/hi-fi sync" (pull from hi-fi-me server into REPO.md).
---

# Hi-fi Prompt Enhancer

This skill pipes a UI-building prompt through your hi-fi inspiration repo and returns it annotated with concrete visual references — making prompts for AI agents more specific and grounded in real design patterns.

## Subcommands

Before doing anything else, check if a subcommand is being invoked:

- **`/hi-fi show`** (also: "show me my inspo", "show the repo", "hi-fi gallery") → go to **[Show Gallery](#show-gallery)**
- **`/hi-fi sync`** (also: "sync from server", "pull from hi-fi-me") → go to **[Sync from Server](#sync-from-server)**
- Anything else → continue with the prompt enhancement workflow below

---

## Prompt Enhancement Workflow

### 1. Get the prompt

Use the prompt text provided inline with the invocation. If invoked with no text, ask:
> "What's the prompt you want to hi-fi?"

Wait for a response before proceeding.

### 2. Clarify vague prompts

Before reading the repo, assess whether the prompt has enough specificity to match against real visual references. A prompt is too vague if it lacks at least two of: a surface type (page, chart, panel, animation), a visual quality (style, motion, density, palette), or a functional context (what data, what interaction, what product area).

If the prompt is vague, ask 2-3 targeted follow-up questions. Examples:
- "What surface is this for — a full page, a chart, a side panel, a widget?"
- "Is this for Datadog specifically, or a standalone visualization?"
- "What data is being shown — time series, events, topology, something else?"
- "Any visual style references — dense/minimal, dark/light, animated/static?"
- "What's the interaction model — explore, monitor, configure?"

Keep questions short and scannable. Wait for the answer, then incorporate it into the prompt before proceeding.

If the prompt is specific enough, skip this step.

### 3. Check for updates

Run:
```bash
cd ~/hi-fi-skills 2>/dev/null && git fetch origin --quiet 2>/dev/null && git status -sb 2>/dev/null
```

If the output contains `[behind` (e.g. `## main...origin/main [behind 2]`), show this note:
> ℹ️ Your hi-fi repo has new items from teammates. Run `cd ~/hi-fi-skills && git pull` to get them.

Then continue regardless — don't block. If git isn't configured or the command fails, skip silently.

### 4. Read the repo

Read the file at `~/.claude/skills/hi-fi/REPO.md`.

- If the file is missing or the Items section is empty: report and stop.
  > "Your hi-fi repo is empty. Run `/hi-fi-extract <url>` to add items, or `/hi-fi sync` to pull from the hi-fi-me server (if it's running)."

### 5. Match items to the prompt

From the REPO.md entries, select the 3–5 most relevant items for the prompt. Consider:
- **Description** — does the visual/technical approach match what the prompt is asking for?
- **Glossary terms** — do the defined terms help explain the technique the prompt needs?
- **Tags and categories** — do they align with the surface type, motion style, or data type in the prompt?
- **Comments** — does the user's note explicitly call out a use case that matches the current prompt? A comment that directly mentions the prompt's context (e.g. "good for onboarding" when the prompt is about onboarding) should boost this item's relevance above tag/description-only matches.

For each selected item, identify the specific phrase in the original prompt it best annotates. Rewrite the prompt with inline annotations:
- If item has `source_url`: `[see: Title](source_url)`
- If no `source_url` but has glossary: `[see: Title — term1, term2, ...]` (first 3 glossary terms)
- If neither: `[see: Title]`

### 6. Check if web search fallback is needed

If you found fewer than 2 good matches from the repo, proceed to step 7. Otherwise skip to step 8.

### 7. Web search fallback

Extract 2-4 key visual/technical concepts from the prompt (e.g. "swarm animation", "particle simulation", "flocking behavior"). Build a search query constraining to high-quality reference sites:

```
<key concept> (site:codepen.io OR site:tympanus.net OR site:observablehq.com OR site:shadertoy.com OR site:awwwards.com OR site:css-tricks.com OR site:d3-graph-gallery.com OR site:bl.ocks.org OR site:animista.net OR site:lottiefiles.com)
```

Run 1–2 searches using the WebSearch tool. Discard results that are:
- Blog posts about the technique rather than live demos/examples
- Generic tutorials without a visual output
- Paywalled or login-gated

Keep only results linking directly to a live demo, interactive example, or high-quality visual reference. Aim for 2–3 web references maximum.

### 8. Present results

See **Output Format** below.

### 9. Error fallback

If REPO.md can't be read for any reason, report the error and stop. Don't fabricate suggestions.

---

## Output Format

Present results exactly like this:

---

## Hi-fi suggestions

*(Gallery matches — omit this section header if there are none)*

1. **"[originalPhrase]"** → [itemTitle](sourceUrl)
   [1-2 sentences on why this reference is relevant]

2. **"[originalPhrase]"** → [itemTitle — term1, term2]
   *(no sourceUrl — glossary terms shown inline)*
   [1-2 sentences on relevance]

*(repeat for all matched items)*

## Web references
*(only present if web search fallback ran — omit entirely if repo returned 2+ results)*

- **[Page title]** — [url]
  [1 sentence on why this is relevant]

*(repeat for each web reference, max 3)*

> Want to save any of these? Run `/hi-fi-extract <url>` to extract and add it to your repo.

## Enhanced prompt

```
[enhanced prompt with inline annotations]
```

---

The enhanced prompt has inline annotations as `[see: Title](url)` markdown links or `[see: Title — term1, term2]`. Paste it as-is. Keep it in a fenced code block for easy copying.

Web references are **not** injected into the enhanced prompt automatically — present them separately to decide whether to paste them in manually.

---

## Show Gallery

Read `~/.claude/skills/hi-fi/REPO.md` and generate a self-contained HTML file at `/tmp/hi-fi-gallery.html`.

**Style:**
- Background: `#000000`, text: `#ffffff`
- Font: `font-family: monospace` throughout — titles, descriptions, tags, everything
- No colors beyond black and white (links can be `#ffffff` underlined)
- Minimal, like a printed reference sheet

**Layout:**
- 3-column grid, no search, no filter controls
- Each card:
  - Thumbnail image (full width of card, aspect-ratio ~16/10, `object-fit: cover`)
  - Title (bold, mono, larger)
  - Description (first 2 sentences, truncated)
  - Tags as plain inline text: `#tag1 #tag2 #tag3`
  - Source link: `↗ source_url` as a plain `<a>` link (omit if no source_url)
- Cards separated by a thin `1px solid #333` border
- Modest padding, tight layout

After writing the file, run:
```bash
open /tmp/hi-fi-gallery.html
```

Report: "Opened gallery in your browser — N items."

---

## Sync from Server

*(Optional — only needed if you run hi-fi-me locally or have a deployed instance)*

Pull items from a hi-fi-me server into REPO.md.

1. Check the server:
   ```bash
   curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/api/export
   ```
   If not 200, report that the server isn't running and stop.

2. Fetch all items:
   ```bash
   curl -s http://localhost:3000/api/export
   ```
   This returns REPO.md-formatted entries as plain text.

3. Read the current `~/.claude/skills/hi-fi/REPO.md`. Parse existing item ids from frontmatter.

4. For each item from the server:
   - If its `id` already exists in REPO.md: update the title, description, categories, tags, thumbnail_url — but **preserve** any existing `**Glossary:**` section for that entry (glossary was hand-enriched, don't overwrite it)
   - If its `id` is new: append it as a new entry

5. Write the merged result back to REPO.md.

6. Report: "Synced N items (X new, Y updated) to REPO.md."
