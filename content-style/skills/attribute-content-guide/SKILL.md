---
name: attribute-content-guide
description: Reference for writing the description and risk fields in audits.json (ScanGov's accessibility/usability audit attributes). Use when adding or editing an audit attribute's copy. A stricter, field-specific subset of the content-style skill.
---

# Attribute content guide

Reference for writing `description` and `risk` fields in `audits.json`. See the `content-style` skill for general site copy — this skill only covers these two fields, where length limits and a fixed sentence pattern apply on top of those general rules.

---

## Audiences

**Executive** — Needs to understand business impact and compliance exposure in plain language. No jargon.

**Digital team** — Needs enough specificity to know what to fix and who is affected.

---

## Voice and language

**Reading level:** 9th grade. Use short sentences and common words.

**Avoid citizen/government-centric language** unless the check is specifically about government (e.g., `schema-government-organization`, `dotgov`):
- "citizens" → "users"
- "visitors" → "users"
- "government services" → "services"
- "government records" → "data" or "confidential data"

**Avoid technical jargon.** If a term has no plain equivalent, use the plain description of what it does:
- ~~"BCP 47 language value"~~ → "valid language code"
- ~~"ARIA attributes"~~ → "accessibility attributes"
- ~~"accessible name"~~ → "label that screen readers can announce"
- ~~"assistive technology"~~ → "screen readers" (more specific and recognizable)
- ~~"mime type sniffing"~~ → "guessing the type of files it serves"
- ~~"HTTP status"~~ → "loads and returns a successful response"

---

## `description` field

**Answers:** What does this check test?

**Rules:**
- One sentence, active voice
- Under 100 characters
- States what the check verifies, not the risk or benefit
- Do not describe the failing state — describe what passing looks like

**Patterns:**
```
The page [has / declares / loads / allows] ...
The site [has / forces / uses / tells] ...
[Elements] have [labels / alt text / titles] so [outcome].
[Elements] use [correct / valid] [markup / values].
```

**Examples (good):**
- "The page has a valid language code so screen readers pronounce content correctly."
- "The page has a doctype declaration so browsers render it correctly."
- "Buttons have labels so screen readers can describe their action."
- "The site uses HTTPS to encrypt data between the server and users."
- "Page content stays in place as the page loads."
- "The site has a security.txt file so researchers know how to report issues."

**Anti-patterns (avoid):**
- Describing the risk instead of the check: ~~"Disabling zooming is problematic for users with low vision."~~
- Describing guidance instead of the check: ~~"Informative elements should aim for short, descriptive alternate text."~~
- Identical descriptions for different checks: ~~"Describes webpage content in a few words."~~
- Technical jargon without context: ~~"The HTTP status of /sitemap.xml is OK."~~
- Passive/hedging: ~~"Adding discernable and accessible text may help screen reader users."~~
- Two sentences: ~~"Screen readers cannot translate non-text content. Adding alternate text helps."~~

---

## `risk` field

**Answers:** Who is affected, and what specifically breaks?

**Rules:**
- One sentence, active voice
- Under 80 characters
- Specific to this attribute — no templated copy-paste across many attributes
- Executive framing: user impact, compliance exposure, trust/reputation
- Name who is affected (type of user) and what happens to them

**Patterns:**
```
Screen readers [cannot / ignore / misinterpret] ...
[User type] cannot [specific action].
[User type] [negative outcome].
```

**Examples (good):**
- "Screen readers ignore attributes that don't match the element's role."
- "Screen readers cannot tell users what a dialog box is for."
- "Keyboard users can focus elements that screen readers cannot announce."
- "Screen readers reference the wrong element when IDs are duplicated."
- "Mobile users with low vision cannot zoom in to read content."
- "Voice control users cannot activate elements by their visible label."

**Anti-patterns (avoid):**
- Generic across many attributes: ~~"Screen readers announce elements incorrectly or skip them entirely."~~ (was used for 23 different ARIA attributes)
- Too vague: ~~"Users with disabilities encounter barriers using your site."~~
- Describing the fix, not the consequence: ~~"Screen readers may not announce elements correctly."~~

---

## Accessibility risk categories

When writing accessibility risks, use the most specific category that applies:

| Category | Use when | Example |
|---|---|---|
| Navigation | User can't move through the page | "Keyboard users cannot bypass repeated navigation." |
| Identification | User can't tell what an element is | "Screen readers cannot tell users what a dialog box is for." |
| Announcement | Screen reader reads the element wrong | "Screen readers ignore invalid roles and treat elements as plain text." |
| Language | Wrong pronunciation/language | "Screen readers mispronounce content, confusing users." |
| Structure | Page structure is broken | "Screen readers cannot navigate grouped elements like menus or lists." |
| Low vision | Visual contrast/zoom issues | "Mobile users with low vision cannot zoom in to read content." |
| Deaf/HoH | Audio-only content inaccessible | "Users who are deaf cannot access video content." |
| Motor | Small targets, timing issues | "Users with motor disabilities struggle to tap small buttons." |
| Voice control | Visible label doesn't match announced name | "Voice control users cannot activate elements by their visible label." |
| Focus | Keyboard/screen reader focus mismatch | "Keyboard users can focus elements that screen readers cannot announce." |

Prefer the most specific category. If an attribute fits a unique risk not in this table, write it fresh rather than forcing a match.

### ARIA attribute risks

ARIA attributes each have a distinct failure mode. Do not share a risk across multiple ARIA checks. The pattern is:

```
Screen readers [ignore / misinterpret / cannot describe / cannot navigate] [specific thing].
```

---

## Length targets

| Field | Target | Maximum |
|---|---|---|
| `description` | 60–90 chars | 120 chars |
| `risk` | 50–70 chars | 90 chars |
