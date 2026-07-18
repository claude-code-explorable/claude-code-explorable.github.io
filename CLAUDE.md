# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**"Claude Code, Explorable"** — a standalone, book-style static site of interactive explorables about how Claude Code works. It is a **separate repo** from the broader `gen-ai-explorables` collection (which it is a part of, and links back to), with its own git history and GitHub Pages deployment.

There is **no build step, no package manager, no test framework, and no dependencies**. To develop: open `index.html` in a browser, or (preferred) serve the root so the reader can theme embedded chapters:

```bash
python -m http.server 8000   # then visit http://localhost:8000/
```

`.nojekyll` is present so GitHub Pages serves the files as-is. Deploy: Settings → Pages → `main` branch, root folder. Also works as-is on Netlify/Vercel/Cloudflare Pages (static, no build command).

## Structure

- `index.html` — the **reader shell**: a sidebar table of contents, a cover, and a reader that embeds each chapter as a live `<iframe>`. It handles navigation, theming, and the cover/chapter layout — it does **not** contain chapter content.
- `NN-slug/index.html` — one fully self-contained explorable per numbered directory. Each is standalone: inline `<style>`, inline `<script>`, no external assets or network calls.

Current chapters:
- `04-under-the-hood-of-claude-code` — the real query loop: context assembly, the five-layer compaction pipeline, the deny-first permission gate, subagent isolation.
- `05-the-files-you-own` — everything under `.claude/` as a markdown file with frontmatter; which parts are advice the model can ignore and which are gates it never sees.

Chapters keep their original two-digit numbers (04, 05) from the parent `gen-ai-explorables` collection, but are presented to the reader as chapter 1, 2, … via the `num` field (see below).

## Adding a chapter

Two steps — no other change needed:

1. Create `NN-slug/index.html` following the explorable pattern below (copy an existing chapter as a scaffold).
2. Append one entry to the `CHAPTERS` array near the top of the `index.html` IIFE. Each entry is:

   ```js
   {
     id:"06", num:3, slug:"06-your-slug",
     title:"Chapter title",
     tag:"Category · subcategory", mech:"verb · verb · verb",
     g1:"#hexcolor", g2:"#hexcolor",   // cover gradient stops
     blurb:"1–3 sentence description shown on the cover card."
   }
   ```

   - `id` is the two-digit folder prefix (must match `slug`); `num` is the reader-facing chapter number.
   - `tag`/`mech` mirror the `gen-ai-explorables` card metadata (taxonomy tag · interaction verbs).
   - `g1`/`g2` are the cover-tile gradient colors.

Also add the chapter to the `README.md` chapter list.

## The explorable pattern

Every chapter follows the same self-imposed conventions — match them when adding or editing one:

- **Single file, zero dependencies.** All CSS and JS inline. No fetch, no CDN, no fonts/images over the network. The "no build, no network" property is the point.
- **Data-driven.** Content lives in a `DATA` array/object at the top of an IIFE (`(function(){ "use strict"; ... })()`), separated from rendering logic. To change what a chapter shows, edit its data, not the render code. Preserve any citation/reference (`ref`/`refT`) provenance — the explainers are grounded in named sources.
- **Interaction chosen from the shape of the idea.** Timeline scrubber, filterable map, draggable simulator, etc. — the interaction should embody the concept.
- **Theming.** Dark is the default. Support both schemes via `@media (prefers-color-scheme: dark)` plus `:root[data-theme="dark"]` / `:root[data-theme="light"]` overrides (CSS custom properties for all colors). A theme toggle flips `data-theme` on the root element and must win over the media query. The reader shell passes/persists theme to embedded chapters — keep chapters theme-aware.
- **Accessibility.** Honor `@media (prefers-reduced-motion: reduce)` (kill transitions/animations), keep keyboard focus working, use semantic/labeled controls.
- Plain ES5-style vanilla JS (`var`, IIFE) — no framework, no modules, no transpilation.

## Grounding

Chapter 1 is grounded in *"Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems"* and the official *How Claude Code works* documentation; the model's internals are left deliberately opaque. Keep new chapters similarly grounded in named sources.
