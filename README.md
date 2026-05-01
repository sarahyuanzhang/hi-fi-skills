# hi-fi skills

Claude skills for enriching UI-building prompts with visual inspiration references.

- **`/hi-fi`** — takes a UI prompt, finds relevant references in your repo, returns it annotated with concrete visual references
- **`/hi-fi-extract`** — takes a URL, analyzes it, and adds a structured entry to the shared repo
- **`/hi-fi show`** — opens a local HTML gallery of your current repo
- **`/hi-fi sync`** — (optional) pulls from a running hi-fi-me server instance

The shared inspiration catalog lives in `hi-fi/REPO.md`. Everyone's additions are visible to everyone once pushed to this repo.

---

## Setup

**Requires:** [Claude Code](https://claude.ai/code) installed.

```bash
# 1. Clone this repo
git clone https://github.com/YOUR-ORG/hi-fi-skills.git ~/hi-fi-skills

# 2. Symlink skill folders into Claude's skills directory
ln -sf ~/hi-fi-skills/hi-fi ~/.claude/skills/hi-fi
ln -sf ~/hi-fi-skills/hi-fi-extract ~/.claude/skills/hi-fi-extract
```

That's it. Open Claude Code and try `/hi-fi show` to verify the gallery works.

---

## Adding inspiration

```bash
# In Claude Code:
/hi-fi-extract https://codepen.io/some-cool-demo

# Claude will analyze the page, propose an entry, and write it to REPO.md on confirmation.
# Then share it:
cd ~/hi-fi-skills
git add hi-fi/REPO.md
git commit -m "add: Title of the thing"
git push
```

## Getting teammates' additions

```bash
cd ~/hi-fi-skills && git pull
```

---

## Usage

```
/hi-fi I want to build a visualization that looks like a swarm of bees
/hi-fi-extract https://observablehq.com/@d3/force-directed-graph
/hi-fi show
```

---

## Notes

- `REPO.md` is committed — it's the shared catalog everyone reads from and writes to
- `CONFIG.md` is gitignored — it's personal (only needed if you use a shared hi-fi-me server)
- The skill works fully offline — no server required for prompt enhancement
