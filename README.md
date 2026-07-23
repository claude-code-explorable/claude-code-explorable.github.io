# Claude Code, Explorable

An interactive field guide to **how Claude Code works** — and the files you own. Each chapter is a self-contained, interactive explorable you can operate, not just read.

**Chapters**
1. **Under the hood of Claude Code** — trace one turn of the real query loop: context assembly, the five-layer compaction pipeline, the deny-first permission gate, subagent isolation.
2. **The files you own in Claude Code** — everything under `.claude/` is a markdown file with frontmatter. Walk a real project tree and learn which parts are advice the model can ignore and which are gates it never sees.
3. **Writing the files you own** — knowing which files exist is half of it; the craft is writing them so Claude actually follows them. Take a real, over-stuffed starter `CLAUDE.md` and route every line to its right home (keep, cut, rule, skill, hook, local) while context cost and adherence move in opposite directions.

## Run locally

No build step, no dependencies. Either open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
# then visit http://localhost:8000/
```

Serving (rather than `file://`) is recommended so the book can theme the embedded chapters.

## Deploy for free (GitHub Pages)

1. Create a new GitHub repository and push this folder to it.
2. In **Settings → Pages**, set the source to the `main` branch, root folder.
3. Your book goes live at `https://<username>.github.io/<repo>/`.

`.nojekyll` is included so Pages serves the files as-is. It also works as-is on Netlify, Vercel, or Cloudflare Pages (static, no build command).

## How it's built

One self-contained `index.html` (sidebar table of contents, cover, and a reader that embeds each chapter as a live `<iframe>`), plus the explorables in their own numbered folders. Everything is inline HTML/CSS/vanilla JS — no framework, no network calls. To add a chapter, drop its folder in and append one entry to the `CHAPTERS` array in `index.html`.

Chapters are grounded in *"Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems"* (arXiv 2604.14228) and the official Claude Code documentation; the model's internals are left deliberately opaque.

By [Bhaskarjit Sarmah](https://bhaskarjitsarmah.github.io). Part of the [Gen AI Explorables](https://bhaskarjitsarmah.github.io/gen-ai-explorables/) collection.
