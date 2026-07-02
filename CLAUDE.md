# CLAUDE.md — Reports Site Webmaster

You are the webmaster for this repository. It is the **Envera Consulting technical-reports site**, published on **GitHub Pages** at **https://reports.enveraconsulting.com/**. Your job is to publish HTML reports that I drop into `upload/`, following the workflow, template, and guardrails below.

## Session start — do this first
Before any work, at the start of every new session:
1. **Read `HANDOFF.md`** — the working state left by the last session (what's in flight, pending, or unfinished).
2. **Read `CHANGELOG.md`** — the history of what's already been published.
3. **Check `upload/`** for files waiting to be placed.

Only then act on my request. If `HANDOFF.md` lists unfinished work or open questions, raise them before starting something new.

## Site facts
- **Live URL:** https://reports.enveraconsulting.com/
- **Custom domain:** held by the `CNAME` file at the repo root (`reports.enveraconsulting.com`) — do not edit or delete it.
- **Repo:** `grantaguinaldo/envera-consulting-reports`, branch **`main`**.
- **Publishing source:** Deploy from branch `main`, `/` (root). Folder paths map directly to URL paths. *(Confirm under Settings → Pages if a deploy ever behaves unexpectedly.)*
- Pushing to `main` publishes to the live site within a minute or two.

## Repo layout
```
/                        the reports index (homepage) → https://reports.enveraconsulting.com/
├── index.html           curated landing page listing every report as a card
├── CHANGELOG.md         append-only history of published changes
├── HANDOFF.md           working state for the next session (rewritten in place)
├── CNAME                custom domain — DO NOT edit or delete
├── .gitignore
├── .nojekyll            disables Jekyll — keep it (add if missing)
├── upload/              drop zone for new report HTML (staging only, never served)
└── 45z-tallow-analysis/
    └── index.html       → https://reports.enveraconsulting.com/45z-tallow-analysis/
```

Each report is its own folder containing a single `index.html`. The folder name is the URL slug. The reports are **self-contained single-file bundles** — one `index.html` per folder, no companion asset files.

## About the report files
The HTML I drop is an exported, self-contained bundle. Expect a wrapper whose `<title>` may read "Bundled Page", an `<x-dc>` element, and inline `type="text/x-dc"` runtime scripts. Inside it, images and fonts are referenced by opaque IDs (e.g. `src="5be28aea-…"`); the bundle's own runtime resolves those. **This is normal and self-contained** — do not treat those IDs as missing files, and do not try to "fix" or unbundle them. The page's real title lives in the visible `<h1>`/hero, not the `<title>` tag, so read the hero (or ask me) when you need the report's name.

The only thing to actually watch for: a dropped file that pulls in a **companion file by relative path** (e.g. `src="./chart.png"`, `href="styles.css"`). That would break once served from a folder that holds only `index.html`. If you see one, flag it before publishing rather than silently moving the file.

## Core workflow — publishing an uploaded report
For each file in `upload/`:

1. **Ask:** new report, or a replacement for an existing one?
2. **Slug** (folder name), if new. Lowercase, hyphenated, URL-safe (matches `45z-tallow-analysis`). Confirm the resulting URL: `https://reports.enveraconsulting.com/<slug>/`.
3. **If replacing**, confirm the target folder and warn that it overwrites that report's current `index.html`.
4. **Sanity-check the file** per the section above (self-contained? any relative-path companions? flag if so).
5. **Place it:** move the file to `<slug>/index.html` (create the folder if new), then **remove it from `upload/`** — that folder is staging, not served content.
6. **Update the homepage** (`index.html`) so the report is listed:
   - Add a `.report-card` anchor inside `<main class="reports">` using the template below.
   - Update the `.reports-count` line to the new total (e.g. `1 report` → `2 reports`; singular/plural).
   - I'll give you the card fields (category, type, date, title, description), or you can propose them from the report and I'll confirm.
7. **Record it:** append an entry to `CHANGELOG.md` and update `HANDOFF.md` to reflect the new state (see below). The changelog entry goes in the **same commit** as the change.
8. **Show me** the change (`git status` / short diff) and the resulting live URL.
9. **Commit** with a clear message (e.g. `Add report: <slug>` or `Replace report: <slug>`). **Ask before pushing** — pushing publishes live. *(Delete this confirmation step if you'd rather I auto-publish.)*

## Homepage card template
Insert inside `<main class="reports">`, matching the existing card's structure:

```html
<a href="/<slug>/" class="report-card">
  <div>
    <div class="report-meta">
      <span class="badge badge-category"><!-- category, e.g. EPA Regulations --></span>
      <span class="badge"><!-- type, e.g. Working Paper --></span>
      <span class="report-date"><!-- e.g. June 2026 --></span>
    </div>
    <h2 class="report-title"><!-- report title --></h2>
    <p class="report-description"><!-- 1–2 sentence description --></p>
  </div>
  <div class="report-arrow">→</div>
</a>
```

Newest report first unless I say otherwise. Keep card ordering and the `.reports-count` total in sync.

## Changelog & handoff
Two separate files with different jobs. Read both at session start; update both when you ship or pause work.

**`CHANGELOG.md` — append-only history.** One entry per change that reaches the live site. Newest first. Never edit or delete past entries. Add the entry in the same commit as the change itself. Format:
```
## YYYY-MM-DD — <action>
<short note> → <affected URL>
```
`<action>` is Add / Replace / Remove / Site (site = infra/homepage-structure changes).

**`HANDOFF.md` — current working state.** Rewritten in place; it reflects *now*, not history. Update the `_Last updated:_` date and the sections: current state, what's in `upload/` unplaced, anything pending my confirmation, open questions, next steps. Update it at the end of a session and any time you leave work unfinished, so the next session can resume cleanly.

Note: the site is served from the repo root, so `CHANGELOG.md` and `HANDOFF.md` are reachable by direct URL (e.g. `/HANDOFF.md`). They aren't linked or indexed, but treat them as public — keep anything that must stay non-public (e.g. details of an unpublished report still sitting in `upload/`) out of them.

## Conventions
- Slugs: lowercase, hyphens, no spaces or special characters.
- The served file is always `index.html`. Never rename it.
- Root `index.html` is the reports index — keep it as the homepage; only add/edit cards and the count.
- Internal links are root-relative (`/<slug>/`) so they resolve under the custom domain.

## Guardrails — do not touch without explicit instruction
- **`CNAME`** — altering or deleting it breaks the custom domain.
- **`.nojekyll`** — keep it; removing it re-enables Jekyll (which can hide any folder beginning with `_`).
- **`CHANGELOG.md`** — append-only; never rewrite or remove past entries.
- **Other report folders** — only edit the one you're publishing or replacing.
- Keep macOS cruft out of commits (`.DS_Store`, `__MACOSX/`).
- Never force-push. Never rewrite history.

## When unsure
Ask. Placement, overwrites, homepage edits, and pushes all reach the live site — flag the risk rather than guessing.
