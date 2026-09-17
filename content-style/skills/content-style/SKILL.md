---
name: content-style
description: Plain-language, inclusive content style rules for ScanGov digital properties — voice, headings, terminology, reading level, and acronyms. Use when writing or reviewing site copy, headings, docs, or marketing content for any ScanGov site (scangov.com, scangov.org, standards.scangov.org, docs.scangov.org, my.scangov.com).
---

# ScanGov content style

Reference for writing and reviewing prose, headings, and UI copy across ScanGov sites. For `description`/`risk` fields in `audits.json`, see the companion `attribute-content-guide` skill — it's a stricter, field-specific subset of these rules.

---

## Voice and language

**Reading level:** 9th grade. Use short sentences and common words.

**Active voice.** Say who does what.
- ~~"Data is encrypted by the site."~~ → "The site encrypts data."
- ~~"Issues were found on 12 pages."~~ → "We found issues on 12 pages."

**Second person.** Address the reader as "you," not "the user" or "one."
- ~~"Users can export their results."~~ → "You can export your results."

**Contractions are fine.** Plain language favors them for readability ("it's," "don't," "you'll"). This isn't a sign of informality — don't flag it.

**No Latin abbreviations in prose.** Spell out "for example," "that is," "and so on" instead of "e.g.," "i.e.," "etc."

**Avoid condescending qualifiers.** Words like "simply," "just," "easily," and "obviously" minimize a task that isn't obvious or easy for every reader. Cut them rather than replace them.

**No directional references that break on reflow or for screen readers.**
- ~~"See the box on the right."~~ → "See [linked term]."
- ~~"The table below shows..."~~ → "The following table shows..." (or link to it)

---

## Inclusive and neutral language

**No government-only framing** unless the content is specifically about government (e.g., a `.gov` domain check, a government-organization schema check):
- "citizens" → "users"
- "visitors" → "users"
- "government services" → "services"
- "government records" → "data" or "confidential data"

"Visitors" → "users" applies to ScanGov's own product and marketing copy, talking about ScanGov's audience. Third-person reporting *about another organization's site* and *its* visitors (a data post, a case study) is a different context — "visitor" is fine there.

**"Digital," not "websites," when referring broadly.** A specific, named site is still "a site" or its name. The word "website(s)" is a flag to reconsider only when it's standing in for the whole category of digital properties/services.
- ~~"We help agencies build better websites."~~ → "We help agencies build better digital services."
- "scangov.com is a website that scans government websites." — the first "website" naming this specific site is fine; the second, referring broadly to the category, should be "digital properties" or "sites."

**Singular "they."** Don't default to "he" or "she," and don't force "he/she."

**No ableist idioms.** Avoid language that treats disability as a metaphor for something negative ("blind to," "crazy," "lame"). This matters more here than most places — ScanGov's own product is accessibility auditing.

---

## Structure

**Sentence case headings.** Capitalize the first word and proper nouns only.
- ~~"How To Fix Your Accessibility Score"~~ → "How to fix your accessibility score"
- Proper nouns and product names keep their casing: "How ScanGov calculates your score"

**Oxford comma.** Use a comma before the final "and"/"or" in a list of three or more.
- ~~"Fast, accurate and free"~~ → "Fast, accurate, and free"

**Spell out acronyms on first use per page**, then use the acronym alone after.
- "This check verifies the page has a Cascading Style Sheet (CSS) file that loads correctly. If the CSS file 404s, ..."
- Common web/tech acronyms readers already know don't need spelling out: URL, HTML, CSS, PDF, FAQ, API, SEO, DNS, HTTP/HTTPS, AI, IP, GET, COVID, IT.
- Not acronyms, so don't try to "spell out": WCAG conformance levels (AA, AAA), Roman numerals in legal citations (ADA Title II), date-format placeholders (YYYY-MM-DD).

**"US" → "U.S."** Always write it with periods, not as a bare acronym.
- ~~"US government websites"~~ → "U.S. government websites"

**No double quotes in headings.** Quotation marks in a heading signal an actual quotation, not emphasis or a "clever" turn of phrase.
- ~~How to fix "broken" links~~ → How to fix broken links
- "What customers are saying about ScanGov" is fine — quote marks appear inside the body quoting an actual person, not in the heading itself.

**Tables follow the components reference.** Use `class="table"` wrapped in `.table-responsive`, with a `<caption class="visually-hidden">` describing the table's contents and `scope="col"` on header cells. Add `table-sm` only for dense data grids (CSV-style previews, data dictionaries). Never add `table-bordered`, `table-hover`, or `shadow` — `scangov.css` applies those automatically. See `components/public/templates/tables.html` for working markup.
- Markdown table syntax (`| a | b |`) does **not** pick up these classes automatically on scangov-com — it renders as a bare `<table>`. Write the table as raw HTML inside the markdown file instead.

**`<ol>` is for steps only.** Use an ordered list only when the reader must complete items in sequence (a how-to, a setup process). Everything else — features, examples, links, any collection without a required order — is a `<ul>`.

**Stat-led bullets: percentage first, then the raw count in parentheses, then a short description.** One consistent order, every time — not "X count (Y%)" in one bullet and "Y% of things" with the count buried in a second sentence in the next.
- Pattern: `**X% (N units)** — description.`
- ~~"284 county websites (9.9%) turn an identified, polite bot away with..."~~ → "**9.9% (284 counties)** turn an identified, polite bot away — with..."
- ~~"85.2% of county websites let the bot in. That is 2,435 of 2,858."~~ → "**85.2% (2,435 counties)** let the bot in."
- A bullet with no natural percentage (a synthesis point, not a raw stat) doesn't need to force one in.

**Descriptive link text.** Never "click here" or "read more" alone — the link text should say what it goes to.

**Numerals.** Spell out one through nine; use numerals for 10 and above.

---

## Examples (good)

- "The site has a valid language code so screen readers pronounce content correctly."
- "You can compare two audits side by side to see what changed."
- "We built ScanGov to help teams find and fix accessibility issues before they reach users."
- "Add your domain, and we'll scan it within a few minutes."

## Anti-patterns (avoid)

- Passive voice: ~~"Alt text should be added to images."~~ → "Add alt text to images."
- Government-only framing outside government-specific content: ~~"Citizens rely on government websites for services."~~ → "People rely on digital services."
- Title case headings: ~~"Getting Started With ScanGov"~~ → "Getting started with ScanGov"
- Missing Oxford comma: ~~"Scan, review and fix."~~ → "Scan, review, and fix."
- Undefined acronym on first use: ~~"Check your CWV score."~~ → "Check your Core Web Vitals (CWV) score."
- Double quotes in a heading for emphasis: ~~Why "AI-ready" content matters~~ → Why AI-ready content matters
- A hand-styled table instead of the components pattern: ~~`<table class="table table-bordered table-hover shadow">`~~ → `<table class="table">` (border/hover/shadow are automatic)
- `<ol>` for a non-sequential list: ~~numbered list of unrelated features~~ → `<ul>` (steps only get `<ol>`)
