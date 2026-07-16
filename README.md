# Code's Roadmap

A companion learning site for [Creative Tech Studio](https://creativetechstudio.org). Code's Roadmap teaches people to write code and work with real developer tools — practical, command-oriented paths aimed at anyone actually building and shipping things (not exclusively kids, despite CTS's youth focus).

It's a static site: plain HTML, one shared stylesheet, and vanilla JavaScript. No backend, no build step, no framework. (A fuller app may come later; for now it's HTML pages that link to each other.)

## What's here

```
roadmap-app/
├── index.html                       # Branded landing page
├── assets/
│   ├── styles.css                   # The single shared stylesheet + design tokens
│   ├── ctslogo.png                  # Full CTS logo
│   ├── cts-mark-logo.png            # Logo mark (nav)
│   ├── design-system.html           # Living design doc (open in a browser)
│   ├── template-roadmap.html        # Starter for a new roadmap (trail) page
│   └── template-guide-roadmap.html  # Starter for a new reference guide page
└── pages/
    ├── cloudflare-app.html          # Roadmap (trail type)
    ├── cloudflare-guide.html        # Reference guide type
    └── git-github-guide.html        # Reference guide type
```

## Page types

The site has three kinds of pages, all sharing `assets/styles.css`.

**Homepage** (`index.html`) — a landing page with search (press `/` to focus, `Esc` to clear), difficulty filters (All / Beginner / Intermediate), and roadmaps grouped into categories: **Web Basics**, **Python & Logic**, and **Ship It**. Each roadmap is a card linking to its own page. Search and filters narrow together (AND), empty categories collapse, and zero matches shows a "no results" state.

**Roadmaps** (the *trail* type — e.g. `pages/cloudflare-app.html`) — one start-to-finish vertical trail of numbered waypoints. Each stop expands to reveal the exact commands and an explanation, with a copy button on every command. Checkboxes mark steps cleared, a progress bar tracks completion, and state is saved in the browser via `localStorage` (each page uses a unique `PROGRESS_KEY`) so progress persists across visits.

**Reference guides** (the *guide* type — e.g. `pages/git-github-guide.html`, `pages/cloudflare-guide.html`) — a sticky sidebar of topics (auto-grouped into sections, collapsing to a dropdown on mobile) plus a reading panel, with a terminal switcher (macOS/Linux · Git Bash · PowerShell · CMD) that rewrites shell commands to match the reader's terminal. These are task-based lookups ("push a project", "fix a rejected push") you jump around in, not start-to-finish paths. Content lives in a `TOPICS` array; each topic can have key-points callouts, command blocks, comparison tables, and info/warn callouts.

## Templates

A matched pair of starter files lives in `assets/`. Copy one into `pages/`, edit the `EDIT:`-marked spots, and fill in its content array — the layout, styling, and behavior come pre-wired.

| Template | Use it for | Ships with |
|----------|-----------|------------|
| `template-roadmap.html` | A start-to-finish **trail** with a single correct order and a finish line | Progress bar, checkable waypoints, `localStorage` persistence, copy buttons; a `STEPS` array with one example of each step type (`local` / `cloud` / `manual` / `done`) |
| `template-guide-roadmap.html` | A **reference** you jump around in, by topic | Auto-grouping sidebar (+ mobile dropdown), terminal switcher with the `SHELL` substitution system, deep-linking, copy-to-clipboard; a `TOPICS` array demonstrating key-points, command blocks, tables, and callouts |

**Which one?** The quick test: if someone would start at the top and work down once, it's a **roadmap**; if they'd come back repeatedly to look up one thing, it's a **guide**. Pick by the shape of the content, not the topic — the same subject could be either.

**`assets/design-system.html`** is the living design doc — open it in a browser. It documents the palette, type scale, spacing, components, layout patterns for all three page shapes, voice/copy rules, do/don'ts, and a "Building a New Page" section that walks through choosing a template with a checklist for each.

## Design system

Everything shares one CTS-branded look, with tokens defined at the top of `assets/styles.css` (see `assets/design-system.html` for the full reference):

- **Palette** — a near-black background (`--void #0A1220`) with orange (`--orange #FF6B2C`) and cyan (`--cyan #2FE6E6`) accents pulled from the CTS logo. Never introduce a third accent — every moment is orange, cyan, or neutral ink.
- **Type** — Chakra Petch (display/titles), Inter (body), JetBrains Mono (code, labels, counts). Loaded from Google Fonts; no fourth face.

## Running locally

Pages reference assets by absolute path (`/assets/…`, `/index.html`), so serve from the project root rather than opening files directly — a `file://` open will break styling and navigation:

```bash
# from roadmap-app/
python -m http.server 8000
# then open http://localhost:8000
```

## Adding a new page

1. **Pick a template** by content shape (see the table above) and copy it from `assets/` into `pages/`, renamed (e.g. `python-guide.html`).
2. Edit every `EDIT:`-marked spot (tab title, headings, intro).
3. Replace the sample content array with your own — `STEPS` for a roadmap, `TOPICS` for a guide — and delete the samples.
4. **Roadmap only:** give it a unique `PROGRESS_KEY`, or it will share saved progress with another page.
5. **Guide only:** give each topic a `group` so the sidebar auto-sorts; keep cross-terminal commands as plain `{ code: '…' }` and only add a `SHELL` entry for genuine native-shell differences.
6. Add a linking card on `index.html` in the right category grid — set `data-diff` (`beginner` / `intermediate`) for a roadmap, or tag it **Guide** for a guide. Match the card title to the page title word for word.
