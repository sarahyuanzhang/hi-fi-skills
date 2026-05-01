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

---
```

Items are separated by `---` horizontal rules.

## Items

*(empty — run `/hi-fi sync` to pull from the server, or `/hi-fi-extract <url>` to add a new item)*
