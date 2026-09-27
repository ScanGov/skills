---
name: feature-announcement
description: How to reflect a ScanGov dashboard feature on scangov.com (ScanGov/scangov-com repo) — the features listing (_data/features.json), and a news post when one is warranted. Use when asked to "add to com", "update com", "add to scangov.com", "update scangov.com", or to announce/update a feature on the marketing site. Companion to the content-style skill for prose voice, and report-format for data/report posts.
---

# Feature announcement (scangov.com)

Reference for reflecting a shipped or changed ScanGov feature on scangov.com. General prose rules are in the `content-style` skill — this covers structure and where things go. For a data-journalism "report" post, see `report-format` instead.

---

## Features listing vs. news post — most updates are listing-only

Every dashboard capability belongs on the features listing. A news post is the exception, not the default: dashboard features like Pulse, Tasklist, Scan management, and Status bar have a features.json entry and **no** news post — the listing is a standing reference, not an announcement. A news post is for a public self-serve tool with its own URL (good-bot-scan, the readability scanner, the RFP builder) or a launch big enough to be its own story. A UI redesign or incremental improvement to an existing dashboard feature is listing-only: update the entry's `items`, don't write a post about it.

If unsure which this is, check `content/news/` for precedent on similarly-scoped past changes before drafting a post.

---

## Before adding anything

Check `_data/features.json` for an entry that already covers this capability — an update to an existing feature extends its `items` rather than needing a new entry. If a news post is warranted (see above), also check `content/news/` for a prior post on the same feature that should be updated instead of superseded by a new post.

---

## Features listing

`_data/features.json` is a flat array. `content/features.html` renders it as cards on `/features/`, and `content/feature.html` paginates it into a detail page per entry at `/features/{{ feature.slug }}/`. No template changes are needed to add an entry.

Each entry:
- `title`
- `slug` — used in the detail page URL
- `description` — one sentence, ~15-20 words, used on the card and as the detail-page lead. See "Voice" below.
- `icon` — a Font Awesome solid class name (no `fa-` prefix duplication)
- `items` — 2-4 short bullet strings (`{ title }`), the capability's specifics
- Optional: `video` (YouTube ID), `docsPath` (link to the relevant docs.scangov.org page), `workExample`

There is no `job` field — it was removed (along with its `<p class="lead mb-4">` in `content/feature.html`) as a redundant second blurb alongside `description`. Don't reintroduce it.

---

## Voice: lead with user benefit, not implementation

A feature's `description` (features listing) and a news post's frontmatter `description` should say what the reader or their site's visitors get out of the feature — not name the UI mechanism that delivers it. "Status badges," "a status guide," "one-click," and similar interface details belong in the body copy or `items`, not the lead.

- Weak (names the UI): "Broken link tracking gets simpler status badges and a status guide explaining what each one means."
- Strong (names the benefit): "Spot broken links on your site at a glance, so you can fix them before your visitors run into them."

Also check `_data/features.json` for a theme word before reusing it in a new `description` — a second feature claiming the same word (e.g., "trust") reads as filler, not a fresh reason to care.

---

## News post (only when warranted — see above)

- Lives at `content/news/YYYY-MM-DD-slug.md`.
- **Never set a plain `permalink:`.** `content/news/news.11tydata.json` sets the default permalink via `eleventyComputed.permalink`, slugified from `title`. A later title edit silently moves a plain `permalink:` page's URL. Only set `eleventyComputed.permalink` yourself when the URL needs to be pinned against future title edits (e.g. a report's working title is likely to change before publish).
- Frontmatter: `draft`, `date`, `author`, `title`, `description` (see "Voice" above), `topics` (array), optional `ogImage`/`ogImageAlt`, `video`, `modified`.
- Structure for a feature-launch post (not a report — see `report-format` for that structure): intro paragraph, then sections like `## What it does`, `## How it works`, and a closing CTA. Use `content/news/2026-09-08-good-bot-scan.md`, `content/news/2025-10-27-scangov-readability-scanner.md`, and `content/news/2026-09-20-broken-link-checker.md` as templates. `## How it works` is a `-` list unless the reader must do the steps in order (see `content-style`'s `<ol>` rule) — a list of what the feature does isn't a sequence.
- Link to two places, not just one: the feature's own docs.scangov.org page — the one that explains the whole feature, not a page that only partially covers it (e.g. `/broken-links/`, not `/status/`, for a feature whose badge glossary lives on `/status/` but whose own docs page is `/broken-links/`) — and its `/features/{{slug}}/` page on scangov-com. Don't re-explain detail that belongs on either page.
- Place the docs-page reference after `## How it works`, not inside `## What it does`. `## What it does` stays focused on what the reader gets; the docs link is a "want more detail" pointer that belongs once the reader already knows how the feature works, e.g. "The broken links docs explain how it works." as its own line after the `## How it works` list.
- The Share/actions block is automatic for any `isPost` page via `_includes/jumbotron.html` + `_includes/actions.html` — never hand-write it.

---

## Images

Optional. Two conventions in use: `public/assets/img/pages/` (feature-launch posts) and `public/assets/img/posts/` (report/announcement social cards). Reference from frontmatter as `ogImage: pages/<name>.png` (path relative to `assets/img/`), kebab-case filenames. A post can skip an image entirely — several feature-launch posts do.

---

## Workflow

1. Identify what shipped — check the current conversation, or `git log`/`git diff` in the feature's own repo (e.g. `my.scangov.com`), for what to reflect.
2. Add or update the features.json entry.
3. Decide whether a news post is warranted (see above) — default to no.
4. If warranted, draft the news post per the structure above, following `content-style` for voice, and link to both the docs page and the features page (see "News post" above) — check the docs page actually exists (see `docs-content`) rather than assuming one does.
5. Leave the diff for review — don't commit unless asked.
