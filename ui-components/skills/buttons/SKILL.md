---
name: buttons
description: The one set of rules for buttons on every ScanGov site (scangov.com, scangov.org, data, docs, standards, components and my.scangov.com). Use whenever you add or change a button, a link that looks like a button, a form submit, a call to action, a toolbar or row action, or a delete or confirm step, and when reviewing any markup or JS-built HTML that contains `btn`. Stops one-off button styles from being created.
---

# Buttons

ScanGov has two button styles and one size rule. If a design seems to need a third style, do not make one: stop and raise it, because the fix belongs in ScanGov/components (`public/css/scangov.css`) for every site at once, not in a page.

All ScanGov sites follow these rules. If you find an older pattern in a repo, replace it with the pattern here; do not copy it.

---

## The two styles

| Style | Classes | Use for |
|---|---|---|
| Primary | `btn btn-primary` | The one main action on a view: a form submit, Save, Send, Get started. |
| Secondary | `btn btn-outline-primary` | Everything else: going to another page, Cancel, Close, Edit, Remove, Copy, Download. |

- One primary per view, placed first. Secondary buttons follow it.
- Two primaries side by side is always wrong. Make one of them secondary.
- Size: default. Never `btn-lg`. `btn-sm` only in these places: an icon-only page action toolbar; a table row or cell; a row action inside a card or list; an "Add" button inside a form (Add website). Never `btn-sm` on a form submit, Generate, a call to action (Plans, Demo), or a modal confirm. One size per group of buttons; do not mix sm and default in a row.
- Buttons side by side sit in a flex container with `gap-2`. Never separate them with whitespace only. `scangov.css` adds a gap for a missed case, but do not rely on it.
- `btn-sm` has its own, lighter shadow. Cards, inputs and regular buttons keep the shared one. Do not set a shadow on a button.

---

## Choosing, in order

1. Does it submit a form, or is it the main next step on this view? Use primary. Otherwise use secondary.
2. Does it go to a URL? Use `<a href="...">`. Does it do something on the page or post data? Use `<button>`. Both take the same classes.
3. Is it destructive (remove, delete)? Use secondary, then a confirmation step. The confirm button inside that step is the only place `btn-danger` is allowed.
4. Is it a tab, a view switcher, a page number, or an accordion header? It is not a button. Use the component for it (see Not buttons).
5. Is it inside a paragraph, a list or report copy? Use a plain text link. The one exception is a raw-data download in a report (secondary, default size).

---

## Markup

Link that goes somewhere:

```html
<a href="/plans/" class="btn btn-primary">Plans</a>
```

Action:

```html
<button type="button" class="btn btn-outline-primary" id="js-copy-link">Link</button>
```

Form submit (a form has exactly one):

```html
<button type="submit" class="btn btn-primary">Save</button>
```

With an icon (leading, decorative):

```html
<button type="button" class="btn btn-outline-primary">
  <i class="fa-solid fa-arrows-rotate" aria-hidden="true"></i> Rescan site
</button>
```

Icon only (for example at phone width): keep a name for screen readers, and make it contain the visible word.

```html
<a href="/demo/" class="btn btn-outline-primary" aria-label="Demo">
  <i class="fa-solid fa-wand-magic-sparkles" aria-hidden="true"></i><span class="d-none d-md-inline ms-2">Demo</span></a>
```

### Action buttons

Page and document actions (Edit, Copy link, Copy, Download, Print, Share, Rescan, Reset) are one set, in that order, on every site. Use `_includes/action-buttons.html` and `public/js/action-buttons.js` from ScanGov/components. Do not hand-write them.

- Icon-only and `btn-sm`, with `aria-label` and `title`. Reset keeps its text because it clears what the person entered.
- Keep the text on an action when: it stands alone, it is consequential (Reset, Delete, Remove), its icon is not familiar, or it is the primary action in a form. Icon-only fits a toolbar of repeated actions with familiar icons.
- One icon per function: Edit `fa-pen`, Copy link `fa-link`, Copy `fa-copy`, Download `fa-download`, Print `fa-print`, Share `fa-share-from-square`, Rescan `fa-arrows-rotate`, Reset `fa-rotate-left`, Back `fa-arrow-left`.
- "Copy link", not "Link". Copy is one menu (Text, Markdown). Download is one menu (HTML, JSON, Markdown, Text, Word). A format that does not apply is left out: formats that need a document are removed when the page has none, a menu with one format becomes a single button, and a menu with none is removed. Site formats (CSV, PDF) are added with `window.sgActions.register`.
- Messages, the same everywhere: an instant action shows a check in the button for 2 seconds and a live region announces it ("Link copied", "Markdown copied", "Downloaded {file}"). A failure shows a toast with text. A long action (rescan, generate) is disabled with a spinner while it runs, then a toast says what happened. Do not use an alert box or a text line for these. Use `window.sgActions.succeed`, `fail`, `setState` and `announce` from JS-built buttons.
- Buttons built in JavaScript use the same markup: two icons in the button (the action and a hidden check, plus a hidden spinner when it can be busy), in that order.

```html
<div class="d-flex flex-wrap gap-2" data-actions data-content="#document" data-filename="name">
  <button type="button" class="btn btn-outline-primary btn-sm" data-action="copy-link" aria-label="Copy link" title="Copy link">
    <i class="fa-solid fa-link" aria-hidden="true"></i><i class="fa-solid fa-check d-none" aria-hidden="true"></i>
  </button>
  <div class="dropdown" data-menu="download">
    <button type="button" id="download-menu" class="btn btn-outline-primary btn-sm" data-bs-toggle="dropdown" aria-expanded="false" aria-label="Download" title="Download">
      <i class="fa-solid fa-download" aria-hidden="true"></i><i class="fa-solid fa-check d-none" aria-hidden="true"></i>
    </button>
    <ul class="dropdown-menu" aria-labelledby="download-menu">
      <li class="small"><button type="button" class="dropdown-item" data-download="md">Markdown (.md)</button></li>
    </ul>
  </div>
</div>
```

Busy: set `disabled`, swap the icon for `fa-compact-disc fa-spin`, use a present-tense label ("Scanning..."), and restore both when it finishes.

Reset after a primary, on a form that takes input: a secondary button with the `fa-rotate-left` icon: `<button type="reset" class="btn btn-outline-primary"><i class="fa-solid fa-rotate-left" aria-hidden="true"></i> Reset</button>`. The tool form include adds it.

Back, to the page the person came from: a secondary link with the left arrow, never a plain text link.

```html
<a href="/tools/policy-generator/done/" class="btn btn-outline-primary">
  <i class="fa-solid fa-arrow-left" aria-hidden="true"></i> Back
</a>
```

Cancel after a primary:

```html
<button type="submit" class="btn btn-primary">Save</button>
<a href="/settings" class="btn btn-outline-primary">Cancel</a>
```

---

## Labels

- A verb, one or two words, sentence case, naming what happens: "Check", "Save", "Rescan site", "Download CSV".
- Never "Submit", "Click here", "Go" (unless it is a search box), or a bare "Yes" outside a confirmation that restates the action.
- Same action, same words everywhere. A Rescan is "Rescan site" on every page.

---

## Icons

- Leading, `aria-hidden="true"`, from Font Awesome. One icon per button. The button's own `gap` spaces it, so do not add `me-*` or `ms-*` margins on the icon or the label.
- The icon always matches the label color. Never set a color on an icon inside a button.
- Adding an icon that has not been used before: check `public/css/scangov.css` in ScanGov/components for a color rule. If there is none, add it in components and sync it to all sites, as with any other shared CSS.

---

## Not buttons

Do not give these button classes, and do not use button classes for them:

- Tabs and view switchers (Card/List, indicator tabs): `nav-pills` or `nav-tabs`.
- Pagination: `page-link`.
- Accordion headers: `accordion-button`. Menu toggler: `navbar-toggler`. Close X: `btn-close`. Toggle choices: `btn-check`.
- Cards and card headers that link: `cardlink`, `stretched-link`.

---

## Not allowed in new code

| Do not use | Use instead |
|---|---|
| `btn-primary-outline`, `btn-secondary-outline`, `btn-outline-secondary`, `btn-secondary` | `btn-outline-primary` |
| `btn-lg` | default size |
| `btn-link` | a plain `<a>` |
| `btn-outline-danger`, `btn-danger` as a first click | `btn-outline-primary`, then a confirmation |
| `<a href="#">`, `javascript:void(0)`, `onclick="..."` | a real `<a href>` or a `<button>` with a JS listener by id or `data-` attribute |
| `<div>` or `<span>` with `role="button"` | `<button>` |
| `<input type="submit">` | `<button type="submit">` |
| A color, border, radius or padding set on a button in a page or component | the two styles above |
| A page-level stylesheet or `<style>` block for a button | change `scangov.css` in components |
| `fw-bold`, `<strong>`, or classes on `p`, `li` or headings around a button | none: leave them plain |

---

## JS-built buttons

Buttons written into strings in JavaScript follow the same rules and the same class names, for example in `scanprogress.js`, `singlepage.js` and `pagebuttons.js`. Bind events by id or `data-` attribute, never inline. Keep the markup in one helper when the same button is built in more than one place.

---

## Checking your work

Before finishing a change, search the changed files:

```
grep -rnE 'btn-(primary-outline|secondary-outline|outline-secondary|secondary|lg|link|outline-danger)\b' <changed files>
grep -rnE 'class="[^"]*\bbtn\b' <changed files>
```

- [ ] Every `btn` is `btn-primary` or `btn-outline-primary`, with `btn-sm` only where the rules above allow it, and one size per group.
- [ ] Buttons side by side are in `d-flex gap-2`.
- [ ] Page actions come from the action-buttons include, are icon-only with `aria-label` and `title`, and use the shared messages. `check-sync` flags an icon-only button with no `aria-label`.
- [ ] The call-to-action band copies with `data-action="copy-page-link"` and `data-action="copy-page"`, not its own script.
- [ ] One primary per view, first in the group.
- [ ] `<a>` goes somewhere, `<button>` does something, and a form submit is `type="submit"`.
- [ ] The label is a one- or two-word verb, and the same action uses the same label everywhere.
- [ ] The icon (if any) is leading, `aria-hidden`, and has no color set by you.
- [ ] Destructive actions have a confirmation step.
- [ ] Nothing in the Not buttons list has button classes.
- [ ] Checked light and dark theme, and hover, focus and disabled states, with and without the icon.

---

## Where this lives

- Button CSS: `public/css/scangov.css` in ScanGov/components, mirrored to every site (and `scripts/bundle.css` where tracked). Change it there first. Then do a full sync: copy the whole file to every site, including my.scangov.com, even when the site is several changes behind. Never add only your lines on top of an older copy. Then run `node scripts/check-sync.js` in components. That script also fails on any button class that is not in the allowed set.
- A live reference with every state: `public/templates/buttons.html` in ScanGov/components.
- Related skills: `tool-page` (the tool form submit) and `report-format` (the raw-data download).
