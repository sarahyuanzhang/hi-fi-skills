# hi-fi skills

Claude skills for enriching UI-building prompts with visual inspiration references.

- **`/hi-fi-me`** — takes a UI prompt, finds relevant references in your repo, returns it annotated with concrete visual references
- **`/hi-fi-extract`** — takes a URL, analyzes it, and adds a structured entry to the shared repo
- **`/hi-fi-me show`** — opens a local HTML gallery of your current repo
- **`/hi-fi-me sync`** — (optional) pulls from a running hi-fi-me server instance

The shared inspiration catalog lives in `hi-fi-me/REPO.md`. Everyone's additions are visible to everyone once pushed to this repo.

---

## Setup

**Requires:** [Claude Code](https://claude.ai/code) installed.

```bash
# 1. Clone this repo
git clone https://github.com/YOUR-ORG/hi-fi-skills.git ~/hi-fi-skills

# 2. Symlink skill folders into Claude's skills directory
ln -sf ~/hi-fi-skills/hi-fi-me ~/.claude/skills/hi-fi-me
ln -sf ~/hi-fi-skills/hi-fi-extract ~/.claude/skills/hi-fi-extract
```

That's it. Open Claude Code and try `/hi-fi-me show` to verify the gallery works.

---

## Adding inspiration

```bash
# In Claude Code — single URL:
/hi-fi-extract https://codepen.io/some-cool-demo

# Or multiple URLs at once:
/hi-fi-extract https://codepen.io/demo1 https://observablehq.com/@d3/example https://shadertoy.com/view/abc

# Claude proposes entries one-by-one, writes all confirmed ones in a single commit, and pushes automatically.
```

## Getting teammates' additions

```bash
cd ~/hi-fi-skills && git pull
```

---

## Using in Claude.ai

No Claude Code? Use the skill in a [Claude.ai Project](https://claude.ai/projects) instead.

1. Open your Project → **Settings → Custom Instructions**
2. Paste the contents of [`hi-fi-me/SKILL.claude-ai.md`](hi-fi-me/SKILL.claude-ai.md)
3. Replace `YOUR_GITHUB_USERNAME` with the GitHub username hosting this repo
4. Say: "hi-fi-me this prompt: [your prompt]"

REPO.md is fetched live from GitHub on every run — no `git pull` needed by anyone.

> Extraction (`/hi-fi-extract`) still requires Claude Code. Gallery view (`/hi-fi-me show`) also requires Claude Code.

---

## Usage

```
/hi-fi-me I want to build a visualization that looks like a swarm of bees
/hi-fi-extract https://observablehq.com/@d3/force-directed-graph
/hi-fi-me show
```

---

## Notes

- `REPO.md` is committed — it's the shared catalog everyone reads from and writes to
- `CONFIG.md` is gitignored — it's personal (only needed if you use a shared hi-fi-me server)
- The skill works fully offline — no server required for prompt enhancement
