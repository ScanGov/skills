---
name: report-format
description: Structure and tooling for ScanGov's data-journalism "report" posts (original R&D, findings, charts). Use when writing, reviewing, or scaffolding a new report-style post on scangov-com, or when building a chart for one. Companion to the content-style skill — this covers report structure and the Chart.js system, not general prose rules.
---

# Report format

Reference for ScanGov's recurring R&D/data posts (e.g. "One in 10 county
websites blocks good bots"). General prose rules still apply — see the
`content-style` skill. This skill covers what's specific to reports:
required structure, and the shared charting system.

---

## Starting a new report

Run `node scripts/new-report.js "Working title"` from the `scangov-com`
repo root. It scaffolds a new file in `content/news/` with:
- `isReport: true` in frontmatter, so tooling can find it
- an `eleventyComputed.permalink` fixed at creation time — **this matters**:
  scangov-com derives news URLs from `title` by default, so a later title
  edit silently moves the page and orphans the old URL. A report's working
  title is especially likely to change before publish, so pin the URL from
  the start.
- the heading skeleton below, pre-filled

---

## Required structure

1. Lead paragraph — no heading. What was tested, and why.
2. `## Why this matters` — right after the lead paragraph, before "What we
   scanned," always present. Ties the finding to real impact up front,
   before the reader sees any numbers.
3. `## What we scanned` — the scope: what was tested, how much of it, and
   when. Stat-led bullets (see `content-style`'s percentage-first rule)
   where a real count applies. This is where scope/timing facts live —
   don't duplicate them in "About the data" below.
4. `## What we learned` — bulleted TL;DR with the key numbers, written so
   a reader who stops here still has the point.
5. Topic/analysis sections — flexible names and count, but each one:
   sentence case, no quote marks in the heading (see `content-style`).
6. `## Who's responsible` — the audience: who should act on or care about
   this finding. A plain bulleted `<ul>` (text only, no icons) from
   `_includes/who-list.html`, pulling role names from `_data/who.json` so
   the list is one shared source of truth instead of retyped per report.
   Each role links to its `/who-we-serve/{slug}/` audience page when the
   `who.json` entry has a `slug`, so a reader can go straight from "this
   applies to you" to the page that makes the case for that audience:
   ```njk
   {% set whoRoles = [
     "Executives",
     "Digital service teams",
     "Third-party vendors",
     "Government leaders",
     "IT staff",
     "Website managers",
     "Hosting providers"
   ] %}
   {% include "who-list.html" %}
   ```
   Role titles must match `who.json` exactly (case-sensitive) — a typo
   silently drops that role rather than erroring. Pick whichever subset of
   `who.json`'s roles are actually relevant to this report; not every
   report needs all of them.
7. `## About the data` — *how* the data was measured, not what/when (that's
   "What we scanned"). Internal shape, in order:
   - Methodology bullets — short, one to two sentences each, uniform
     length. Don't restate a fact already given elsewhere on the page (a
     chart's own caption/note, "What we scanned") — link to this section
     instead of repeating it.
   - `### Corrections` — only when this report is amending an **earlier
     published version of itself** (numbers changed after publish, a
     methodology bug found post-publish, etc.). Narrative, not bulleted
     facts: what was wrong, when it was fixed, how it affects the numbers
     shown. Readers checking "was this corrected" shouldn't have to scan
     past methodology bullets to find it.
     A bug found and fixed *while preparing* the report, before it ever
     published, is not a correction — that's just methodology, and goes
     as a bullet in the list above instead (see the county-bot post's
     "What we got wrong, and fixed" bullet for the pattern: same content,
     but framed as how the data was produced, not as a public correction).
   - `### Raw data` — its own subsection, not a bolded inline line buried
     in the bullets above it. State what one row represents ("One row per
     county:"), then bullet the actual CSV column names — each as `` `code` ``
     followed by a short, plain description of that field, not a narrative
     sentence trying to summarize every field in one breath. Then a
     download button:
     `<a href="/data/FILE.csv" class="btn btn-outline-primary btn-sm">Download CSV</a>`.
     This is the one deliberate exception to "no `.btn` in report body
     copy" (see "What to do next" below) — a raw-data download is a
     single, unambiguous action, unlike the more persuasive call to action
     that section makes.
8. `## What to do next` — the report's call to action. No lead-in
   sentence — go straight to a short, concrete checklist of what the
   reader should actually do, with a plain text link to the relevant tool
   worked into its first item (e.g. "Run the [good bot
   scan](/tools/good-bot-scan/) on your domain."). Text only — no
   `.btn`-styled link/button in report body copy (contrast with "Raw
   data" above, which does use one deliberately).
9. `## About ScanGov` — always last. Same text every report, don't
   rewrite it per post:
   > ScanGov monitors websites for [AI-readiness](/botability/),
   > [accessibility](/accessibility/), [security](/security/), and
   > [usability](/usability/), and helps teams build better digital
   > experiences. [Learn more about ScanGov](/about).

content-lint warns if a file with `isReport: true` is missing `## What we
scanned`, `## What we learned`, `## Why this matters`, `## Who's
responsible`, `## About the data`, `## What to do next`, or `## About
ScanGov`.

---

## Charts: use the shared Chart.js system, not hand-built SVG

Every report chart uses one include —
`_includes/report-chart.html` — backed by vendored Chart.js
(`public/js/chartjs-vendor.js`, `public/js/report-charts-init.js`). Do not
hand-write inline `<svg>` bar charts or a per-post `<style>` block — that
was the old approach (see the county-bot post's git history) and it's what
this system replaces: chart numbers and the accessible data-table numbers
were typed twice, with no shared source, so they could silently drift out
of sync.

**Match `my.scangov.com`'s Pulse feature.** Pulse (`src/shared/
pulsesection.mjs` + `public/js/scorechart.js`) already uses Chart.js
elsewhere in the org — this system mirrors it rather than inventing its
own conventions:
- The vendored `chartjs-vendor.js` is the same file Pulse uses
  (`my.scangov.com/public/js/chart.min.js`, Chart.js v4.5.1, the full
  official UMD build — not a custom trimmed build), for one consistent
  Chart.js version/plugin-set across the ScanGov ecosystem.
- The legend is Chart.js's own built-in Legend plugin (`position:
  'bottom'`), not hand-built HTML — same as Pulse. A custom
  `generateLabels` maps the chart's `muted`/non-muted per-bar colors to
  legend entries, since that's a per-data-point distinction Chart.js's
  default per-dataset legend doesn't handle on its own.
- Axis tick/grid color read from the same `--bs-secondary-color` /
  `--bs-border-color` custom properties Pulse reads, via
  `getComputedStyle(document.documentElement)`.
- Chart.js has no built-in "attribution" concept (nothing like a mapping
  library's attribution control) — see Attribution below.

**Usage**, per chart:
```njk
{% set chartCaption = "What happens when ScanGovBot asks for a county homepage" %}
{% set chartSubcaption = "2,858 U.S. county websites, September 2026" %}
{% set chartValueLabel = "Counties" %}
{% set chartShareTotal = 2858 %}
{% set chartRows = [
  { label: "Lets the bot in", value: 2435, muted: true },
  { label: "Turns away the basic request only", value: 100 }
] %}
{% include "report-chart.html" %}
```

- `chartRows` is the single source of truth — the chart and its accessible
  data table both render from it, so they can't disagree.
- The data table is **not** a Chart.js feature — Chart.js only draws to
  the canvas. The table is plain HTML this include builds itself from
  `chartRows`, entirely separate from Chart.js.
- The table sits inside a **Bootstrap accordion** (`.accordion` /
  `.accordion-button` / `.accordion-collapse`, `data-bs-toggle="collapse"`)
  — not native `<details>/<summary>`. This uses the Collapse component
  already vendored sitewide (`bootstrap-trimmed.min.js`, loaded on every
  page via `_includes/js.html`), so it adds no new JS. See
  `components/public/templates/accordion.html` for the reference markup.
  The button/collapse ID pair is derived from `chartCaption` via
  Eleventy's built-in `slugify` filter — give two charts on the same page
  distinct captions, or their IDs collide.
- The table uses `class="table table-sm table-bordered small"` — the
  "Dense data" pattern in `components/public/templates/tables.html`, not
  just `table table-sm`.
- `muted: true` on a row renders it in the muted/green color instead of the
  main red bar color (e.g. "not a block" categories, as opposed to "a
  block" — see Colors below).
- `chartShareTotal` is optional — set it when several charts on the same
  page are each a subset of one larger population and should all show
  "share of N" against that same N, not each chart's own row sum (this is
  exactly the county-bot post's situation: three charts, all sharing 2,858
  as the denominator).
- `chartMainLabel` / `chartMutedLabel` are optional — set both on a chart
  that mixes `muted: true` and non-muted rows (e.g.
  `chartMainLabel = "Blocks the bot"`, `chartMutedLabel = "Doesn't
  block"`) for a two-entry legend. Set only `chartMainLabel` on a
  single-color chart for a one-entry legend. This is Chart.js's built-in
  Legend plugin, positioned at the bottom of the chart — not hand-built
  HTML (see Colors below for why prose should reference these labels, not
  the colors).
- **If a page has more than one chart, re-set every variable before each
  `{% include %}` call** — including optional ones you're not using this
  time, e.g. `{% set chartMainLabel = "" %}` on a chart with no muted rows.
  Don't rely on this file's own trailing resets to clear a value the
  calling template never re-sets itself; that's what caused a legend to
  leak onto later charts the first time this was built.
- Add both `<script src="/js/chartjs-vendor.js"></script>` and
  `<script src="/js/report-charts-init.js"></script>` once, anywhere in the
  post — page-specific scripts go directly in the content file on
  scangov-com, not the shared layout (matches how `readability.html` loads
  `readit.js`).
- Requires the `numberFormat` Nunjucks filter (already registered in
  `eleventy.config.js`) for the data table's number formatting.

**Bars are always ordered most to least.** The include sorts `chartRows` by
`value` descending automatically — you don't need to pre-sort the data you
pass in, and both the chart and its accessible data table use the same
sorted order (so they can't disagree on ordering either). If a chart
genuinely needs a different order (e.g. a fixed narrative sequence instead
of magnitude), that's not supported yet — flag it as a real need before
working around it.

**Colors:** bold red (`--report-chart-bar`) for the main/flagged category,
bold green (`--report-chart-muted`) for everything else — a deliberate red
= bad / green = fine contrast, not a light/dark pair. Both are CSS custom
properties on `:root` in `scangov.css` (must be `:root`, not scoped to
`.report-chart` — `report-charts-init.js` reads them via
`getComputedStyle(document.documentElement)`, which only sees properties
set on an ancestor of `<html>`; scoping them to `.report-chart` made them
invisible to the script, which silently fell back to old hardcoded
blue/gray defaults the first time this was built). Read once at chart-init
time — a live theme toggle after the chart has rendered won't recolor it,
known limitation, not yet fixed.

**Don't reference chart colors in prose.** Not everyone can distinguish
red from green — some readers are colorblind. Never write "the red bars"
or "the green line." Reference the legend's own text label instead (e.g.
`chartMainLabel = "Blocks the bot"` → say "the sites that block the bot,"
not "the red bars"). This applies to plain body copy too, not just chart
captions — the county-bot post originally had a note reading "Red bars are
firewall or CDN blocks. Green bars are not blocks..." which said the same
thing the legend already said, in a way a colorblind reader couldn't use;
it was rewritten to reference the legend label ("Doesn't block covers...")
instead of the color, and the color-only half of the sentence was cut as
redundant with the legend.

**Attribution.** Chart.js has no built-in attribution field or plugin
(unlike e.g. a mapping library's attribution control) — every
`report-chart.html` chart gets a plain "Source: ScanGov" line
(`.report-chart-credit` in `scangov.css`) under the chart, in HTML, not
drawn on the canvas.

**Source of truth for these files** is `components` — `_includes/`,
`public/js/`, and the relevant section of `public/css/scangov.css`. Edit
there first, then copy into `scangov-com` (and any other site that adopts
this system); `scangov-com`'s own `_includes/` and `public/js/` aren't
auto-synced from `components` (only `scangov.css` is, via
`scripts/getcomponents.js`, and only in local dev — a production build
needs the file copied in and committed).

**A gotcha if you extend `report-chart.html`:** any Nunjucks-syntax example
inside its own HTML documentation comment must be wrapped in
`{% raw %}...{% endraw %}`. Nunjucks processes `{% %}` tags regardless of
HTML comments — an unwrapped `{% include "report-chart.html" %}` inside the
file's own usage-example comment caused infinite self-inclusion the first
time this was built. This bit twice: a later doc edit added a new inline
`{% set %}` example outside the existing raw block and broke the build
again — wrap the *entire* comment, not just the original code sample.

**A gotcha that silently broke every chart the first time this was built:**
markdown-it doesn't recognize a bare `<canvas>` as block-level HTML, so it
wraps it in a stray `<p>` — `<p><canvas ...></canvas></p><script
type="application/json">...</script>`. A sibling `<script>` right after the
canvas in the source ends up as a sibling of that `<p>` in the actual DOM,
not of the canvas, so `canvas.nextElementSibling` finds nothing and no
chart ever renders — no error, just a blank space where a chart should be.
Fixed by putting the chart's data directly on the canvas as a `data-rows`
attribute instead of a sibling script, which doesn't depend on DOM
adjacency at all. If you're debugging a report chart that isn't showing,
check the actual built HTML (`_site/.../index.html`), not just the source
template — a markdown-mangled DOM structure won't show up in the source.

---

## Data pipeline

- Raw CSV lives in `public/data/*.csv` (e.g.
  `county-bot-blocking-2026-09.csv`) — this is the citable "raw data" link
  every report ends with.
- Not yet built: a per-report summary script
  (`scripts/reports/<slug>.js`) that reads the CSV and outputs the numbers
  `chartRows` uses, so a report can be regenerated if the underlying data
  changes rather than hand-recomputing percentages. Add one if a report's
  numbers need to be refreshed after publish.
