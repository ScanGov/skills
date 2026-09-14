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
- Common web/tech acronyms readers already know don't need spelling out: URL, HTML, CSS, PDF, FAQ, API, SEO, DNS, HTTP/HTTPS, AI, US, IP, GET, COVID.
- Not acronyms, so don't try to "spell out": WCAG conformance levels (AA, AAA), Roman numerals in legal citations (ADA Title II), date-format placeholders (YYYY-MM-DD).

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
