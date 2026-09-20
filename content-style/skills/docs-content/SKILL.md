---
name: docs-content
description: How to add or update a page on docs.scangov.org (ScanGov/docs repo) when a ScanGov feature ships or changes. Use when asked to "add to docs", "update docs", or to document a new/changed feature on the docs site. Covers content file structure, frontmatter schema, categories, and sidenav placement. Companion to the content-style skill for prose voice.
---

# Docs content

Reference for adding or updating pages on docs.scangov.org (the `docs` repo). General prose rules are in the `content-style` skill — this covers structure and where things go.

---

## Before adding anything

Search `content/*.md` for a page that already covers the topic (grep the feature name and adjacent concepts). docs.scangov.org pages accumulate related content over time — a `/status/` page that started as an HTTP status-code glossary later grew a URL-sync-state section. Extending an existing page with a new `##` section is almost always the right move over creating a new one. A new page is for a genuinely new topic with no existing home.

---

## File structure

- All top-level pages are flat markdown files in `content/*.md` — the filename is the URL slug (`content/impact.md` → `/impact/`). No subdirectories.
- Shared config lives in `content/content.11tydata.json` (`layout: layouts/docs.html`, `tags: pages`, `isDoc: true`) — don't set `layout` per file.
- `title` and `description` frontmatter render automatically as the page's `<h1>` and lead paragraph. Body content starts at `##` (h2), not `#`.
- `toc.html` builds the on-page nav from `h2`/`h3` only — anything meant to appear there needs a real heading, not bold text.

---

## Frontmatter schema

Required — enforced by `scripts/generate-content-json.mjs`, which fails CI if missing:
- `title`
- `description`
- `icon` — a Font Awesome class string, e.g. `"fa-solid fa-check-circle"`

Optional:
- `date`, `modified`
- `category` — only pages with a `category` get exported to `_data/contentexport.json` for cross-repo use by my.scangov.com. Categories in use: `product`, `getting-started`, `account`, `legal`, `standards-reference`. There's no generic "reference" category; `standards-reference` is the closest fit for a standards/attribute-style page.
- `keywords`
- `topics` — a list, conventionally just `- ScanGov`
- `scangov: true` — toggles the `scangov.html` include
- `imgOg`, `imgAlt`, `imgCaption`
- `videos` — a list of `{ id, title }`

---

## Navigation

The sidenav is hand-maintained, not generated, in `_data/sidenav.json`, grouped into sections ("Get started", "Scanning & data", "Scoring", "Standards & guidance", "About & policies"). A new page needs its own entry in the right section. Extending an existing page needs no nav change.

---

## Workflow

1. Identify what shipped — check the current conversation, or `git log`/`git diff` in the feature's own repo (e.g. `my.scangov.com`), for what the docs need to explain.
2. Search for an existing page to extend (see "Before adding anything").
3. Extend or create the content file per the structure and frontmatter above.
4. Add a sidenav entry only if this is a genuinely new page.
5. Follow the `content-style` skill for voice, headings, and terminology.
6. Leave the diff for review — don't commit unless asked.
