# Handoff

Working state for the next session. Read this first, then CHANGELOG.md, then check `upload/`.
Keep this current: update it whenever you finish work or leave something unfinished. Unlike the changelog, this file is rewritten in place — it reflects *now*, not history.

_Last updated: 2026-07-02_

## Current state
- Site live at https://reports.enveraconsulting.com/
- Homepage (`index.html`) **redesigned to match the main Envera site**: full site header (logo from `/assets/`, nav with Capabilities dropdown, green "Get in Touch" CTA), archive-style hero, restyled report cards (site design tokens — Fraunces/DM Sans, brand greens, card hover-lift), and full dark site footer (brand blurb, columns, LinkedIn, address, legal — matches live site: brand + Capabilities + Contact, empty 4th slot, no newsletter). Still a self-contained single file; nav/footer links point absolutely to `enveraconsulting.com`. Report-card class names (`.report-card`, `.badge`, `.reports-count`, etc.) preserved so the CLAUDE.md card template still applies verbatim.
- Contact: replica of the site's split contact modal is inlined (opens on any "Get in Touch"/`/contact` click via a small inline script). No backend on this subdomain, so the form hands off to the visitor's email client via `mailto:hello@enveraconsulting.com` (prefilled subject/body) rather than a server POST.
- 1 report published: `45z-tallow-analysis` (SAF 1.00 revision; card date July 2026).
- `assets/` at repo root now wired into the homepage: `envera-logo.png` (header), `envera-logo-white.png` (footer), `og-default.jpg` (OG/Twitter meta).

## In `upload/` (not yet placed)
- `upload/web-templates/` and `upload/static/` — a **copy of the main enveraconsulting.com site source** (Flask templates + CSS/JS/images), dropped in as design reference for the redesign above. Not report content; these are staging-only reference material and are not served. Decide whether to delete them now that the redesign is done (they're currently untracked per `git status`).

## Pending my confirmation
- Redesign committed? **No** — changes are staged in the working tree, not yet committed or pushed. Awaiting go-ahead to commit + push (push publishes live).

## Open questions
- Mobile nav: the redesign faithfully replicates the main site, which **hides the nav menu below 900px** (logo remains, links home) — there is no hamburger. On mobile the "Get in Touch" CTA is therefore not reachable from the nav (footer link still opens the modal). If a working mobile menu is wanted, add a toggle + small JS.
- Contact modal uses a `mailto:` handoff (no backend). If a true no-email-client submission is wanted later, wire the form to a form service (Formspree/Basin) or a serverless endpoint.
- Clean up `upload/web-templates/` and `upload/static/` reference dumps once no longer needed?

## Next steps
- Verify the redesigned homepage in a browser, then commit (`index.html`, `CHANGELOG.md`, `HANDOFF.md` together) and push once confirmed.
