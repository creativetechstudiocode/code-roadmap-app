# CLAUDE.md

Guidance for Claude Code (and humans) working in this repo.

## What this is

**Code's Roadmap** — a static companion learning site for Creative Tech Studio (creativetechstudio.org) that teaches coding and developer tooling through command-oriented walkthroughs. It is intentionally simple: plain HTML, one shared stylesheet, and vanilla JavaScript. **No backend, no build step, no framework, no dependencies.** A fuller app may be built later, but today it's HTML pages linking to each other — don't add tooling unless asked.

## Layout

```
index.html                         Homepage (landing, search, filters, category grids)
assets/
  styles.css                       The ONLY stylesheet — design tokens + all shared styles
  ctslogo.png, cts-mark-logo.png   Logos
  design-system.html               Living design doc — source of truth for look & rules
  template-roadmap.html            Starter for a trail page
  template-guide-roadmap.html      Starter for a reference-guide page
pages/                             All non-home pages live here
  cloudflare-app.html              Roadmap (trail)
  cloudflare-guide.html            Reference guide
  git-github-guide.html            Reference guide
```

## The three page types

- **Homepage** (`index.html`) — hero + search + difficulty filters + category card grids. Cards link to pages. Search (`/` focuses, `Esc` clears) and filters combine with AND.
- **Roadmap / trail** (from `template-roadmap.html`) — one ordered, top-to-bottom path. Numbered waypoints expand to show commands; checkboxes + progress bar; progress saved in `localStorage` under a per-page `PROGRESS_KEY`. Content is a `STEPS` array.
- **Reference guide** (from `template-guide-roadmap.html`) — sidebar of topics (auto-grouped; mobile dropdown) + reading panel + terminal switcher (macOS/Linux · Git Bash · PowerShell · CMD). Look-up tool, no progress tracking. Content is a `TOPICS` array; a `SHELL` table holds per-terminal command variants.

**Roadmap vs guide:** start-at-top-and-work-down-once → roadmap; come-back-to-look-up-one-thing → guide. Choose by content shape, not topic.

## Conventions (follow these)

- **Absolute asset paths.** Pages live in `/pages/` but the site is served from root, so link assets and nav as `/assets/styles.css`, `/assets/ctslogo.png`, `/index.html`. Relative paths break from `/pages/`.
- **Serve from the project root** to view (`python -m http.server 8000`). Opening a file via `file://` breaks styling and links.
- **One stylesheet.** Put shared styling in `assets/styles.css`. Page-specific CSS (e.g. the guide sidebar layout) goes in a `<style>` block in that page's `<head>` — but reuse the design tokens.
- **Use the design tokens**, never hard-coded colors. They're the `:root` block in `styles.css` and are documented in `design-system.html`.
- **Two accents only.** Every UI moment is orange (`--orange`), cyan (`--cyan`), or neutral ink. Never add a third accent color.
- **Three fonts only.** Chakra Petch (display/titles), Inter (body), JetBrains Mono (code, labels, counts). No fourth face.
- **Content lives in JS arrays**, not hand-written HTML. Edit `STEPS` / `TOPICS`; the render functions build the DOM. User-visible strings pass through `escapeHtml`, except fields explicitly documented as allowing inline HTML (topic/step `body`, `intro`, `keypoints`, `callout.html`).

## Adding a page

1. Copy the matching template from `assets/` into `pages/`, renamed.
2. Edit every `EDIT:`-marked spot (title, headings, intro).
3. Replace the sample `STEPS`/`TOPICS` with real content; delete the samples.
4. Roadmap: set a **unique** `PROGRESS_KEY` (a shared key overwrites another page's progress).
5. Guide: give each topic a `group` (sidebar auto-sorts by first-seen order); keep same-on-every-terminal commands as `{ code: '…' }` and only add a `SHELL` entry for genuine native-shell differences.
6. Add a linking card on `index.html` in the right category — `data-diff` (`beginner`/`intermediate`) for a roadmap, a **Guide** tag for a guide. Card title must match the page title exactly.

## Keep these in sync when changing structure

`design-system.html` (the "Building a New Page" section and Layout Patterns table), this file, and `README.md` all describe the templates and page types. If you rename a template, change the asset-path convention, or add a page type, update all three.

## Voice

Plain, warm, active. Titles describe the outcome ("Build Your First Webpage"), not the tech. Card descriptions are one sentence. Buttons name the action ("Start"). Numbers over adjectives ("6 steps", not "quick and easy").
