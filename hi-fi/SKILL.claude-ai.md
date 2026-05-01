---
name: hi-fi
description: Enhances a UI-building prompt with visual inspiration references from the shared hi-fi inspiration repo. Invoke when the user says "hi-fi my prompt", "hi-fi this", "enhance this prompt with inspiration", "add visual references", "/hi-fi", or provides a UI design prompt and wants to enrich it with concrete references. Also trigger when the user is about to send a prompt to an AI agent for building UI and wants to inject relevant visual/code references first.
---

> **Setup:** Replace `YOUR_GITHUB_USERNAME` in step 3 below with the GitHub username where the
> hi-fi-skills repo is hosted. Do this once before pasting these instructions into your Project.
>
> Note: Gallery view (`/hi-fi show`) and sync (`/hi-fi sync`) require Claude Code. To extract
> and add new items to the repo, use `/hi-fi-extract` in Claude Code (terminal).

# Hi-fi Prompt Enhancer

This skill pipes a UI-building prompt through the shared hi-fi inspiration repo and returns it annotated with concrete visual references — making prompts for AI agents more specific and grounded in real design patterns.

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

### 3. Fetch the repo

Use the **WebFetch** tool to retrieve:
```
https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/hi-fi-skills/main/hi-fi/REPO.md
```

This always returns the latest version of the catalog — no local setup or `git pull` needed.

- If the fetch fails (404, network error): report the error and stop.
  > "Couldn't fetch the hi-fi repo from GitHub. Check that the URL in your Project instructions has the correct GitHub username."
- If the Items section is empty: report and stop.
  > "The hi-fi repo is empty. Add items using `/hi-fi-extract` in Claude Code."

### 4. Match items to the prompt

From the REPO.md entries, select the 3–5 most relevant items for the prompt. Consider:
- **Description** — does the visual/technical approach match what the prompt is asking for?
- **Glossary terms** — do the defined terms help explain the technique the prompt needs?
- **Tags and categories** — do they align with the surface type, motion style, or data type in the prompt?
- **Comments** — does the user's note explicitly call out a use case that matches the current prompt? A comment that directly mentions the prompt's context (e.g. "good for onboarding" when the prompt is about onboarding) should boost this item's relevance above tag/description-only matches.

For each selected item, identify the specific phrase in the original prompt it best annotates. Rewrite the prompt with inline annotations:
- If item has `source_url`: `[see: Title](source_url)`
- If no `source_url` but has glossary: `[see: Title — term1, term2, ...]` (first 3 glossary terms)
- If neither: `[see: Title]`

### 5. Check if web search fallback is needed

If you found fewer than 2 good matches from the repo, proceed to step 6. Otherwise skip to step 7.

### 6. Web search fallback

Extract 2-4 key visual/technical concepts from the prompt (e.g. "swarm animation", "particle simulation", "flocking behavior"). Build a search query constraining to high-quality reference sites:

```
<key concept> (site:codepen.io OR site:tympanus.net OR site:observablehq.com OR site:shadertoy.com OR site:awwwards.com OR site:css-tricks.com OR site:d3-graph-gallery.com OR site:bl.ocks.org OR site:animista.net OR site:lottiefiles.com)
```

Run 1–2 searches using the WebSearch tool. Discard results that are:
- Blog posts about the technique rather than live demos/examples
- Generic tutorials without a visual output
- Paywalled or login-gated

Keep only results linking directly to a live demo, interactive example, or high-quality visual reference. Aim for 2–3 web references maximum.

### 7. Present results

See **Output Format** below.

### 8. Error fallback

If the repo can't be fetched for any reason, report the error and stop. Don't fabricate suggestions.

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

> Want to save any of these? Run `/hi-fi-extract <url>` in Claude Code to add it to the shared repo.

## Enhanced prompt

```
[enhanced prompt with inline annotations]
```

---

The enhanced prompt has inline annotations as `[see: Title](url)` markdown links or `[see: Title — term1, term2]`. Paste it as-is. Keep it in a fenced code block for easy copying.

Web references are **not** injected into the enhanced prompt automatically — present them separately to decide whether to paste them in manually.
