---
name: tool-page
description: How to build a free tool page on scangov.com (ScanGov/scangov-com repo, content/tools/). Use when adding or changing a tool, such as the good bot scan or readability scan, so every tool shares one layout and one set of form styles. Companion to content-style for prose and feature-announcement for the features listing and news post.
---

# Tool page (scangov.com)

Every tool under `content/tools/` uses the same page shell and the same form. Do not restyle the form per tool.

---

## Page shell

- Front matter: `layout: layouts/default`, `isTool: true`, `permalink: /tools/{slug}/`, plus `title` and `description`.
- Body: `container page-section` (the border below the tool separates it from the related content) > `row` > three columns, `col-md-2` (empty), `col-md-8` (the tool), `col-md-2` (empty).
- Nothing goes above the form. No intro paragraph or heading; the page title and description already say what the tool is. The one exception is a link to a sample result. Its text is always just "Sample" (not "Example"), in the breadcrumb font (`font-monospace small`), with the `fa-spray-can-sparkles` icon, and it opens in the same tab (no `target="_blank"`).
- An inline sample of the generated document sits in its own `container`, below the form and above the info box, at the document width (`col-md-1`, `col-md-10`, `col-md-1`), the same as the `done` page. It is wider than the tool column. With an inline example, the form container and the sample container each use `pb-4 mb-4 border-bottom` (tighter than `page-section`), and the info box container uses `page-section`. The info box then uses `mt-3` in place of `mt-5`, so the space below the border matches the space above it. The sample's headings sit under its h2 (`h3` and below): pass `policyHeadingOffset` to the policy include. The Sample link at the top jumps to its `<h2 id="sample">Sample</h2>`.
- Breadcrumb: Tools > the tool. In the tool's front matter, set `breadcrumbParent` to `/tools/` titled "Tools" and `breadcrumbCurrent` to its `toolName`.
- Order inside the middle column: the form, then a loading block (if the tool needs one), then the results block, then one `alert alert-info mt-5` box (the extra top margin sets it apart from the form). All information about the tool goes in that one box: the explanation, guidance, and the FAQ (`{% include "faq.html" %}` as its last child). Do not add separate sections for tool information outside it.

- End the page with `{% include "tool-related.html" %}`. It sits outside the narrow tool column, at the full site width with three cards per row, like the rest of the site. Set `indicator` in front matter for one indicator, or `toolIndicators` (a list of indicator urls) for a tool that covers several. Do not name the list `indicators`; that is the site-wide data.
- Set `toolName`, `date`, `modified`, `author`, `ogImage` and `ogImageAlt` in front matter, like the other tools.
- Fire `gtag('event', 'tool_run', { tool: '{slug}' })` on submit.

Reference pages: `content/tools/good-bot-scan.html` and `content/tools/readability/index.html` (single field), `content/tools/policy-generator/index.html` (several fields).

---

## Form

Use the shared include from ScanGov/components, `_includes/tool-form.html`. Set the variables first, then include it:

```
{% set toolFormId = "readability-form" %}
{% set toolFieldLabel = "Page URL" %}
{% set toolPlaceholder = "https://www.example.com/page-to-review/" %}
{% set toolButtonLabel = "Check" %}
{% include "tool-form.html" %}
```

- Leave the placeholder empty unless it shows a real format the person must follow, such as a URL. Do not use made-up example names like "City of Example".
- The field is `name="site"` `id="site"`, with a required-field asterisk, `form-control-lg`, and a `btn btn-primary` submit button with `id="submitbutton"` (see the `buttons` skill for button rules).
- The form ends with the shared Reset button (`type="reset"`, secondary, `id="resetbutton"`), after the submit button. The include adds it. A tool that shows results clears them on the form's `reset` event (see `public/js/readit.js` and `scan-check.js`).
- The button says what the tool does in one word, such as "Check". Do not use "Submit".
- The fixed call-to-action band (`cta.html`) on tool pages and their generated-document pages does not show the Link and Text buttons, because the tool has its own. Set `hideCtaCopy: true` on a document page that is not `isTool`.
- A reading level option lists plain grade levels (Kindergarten to College), not ARI scores. A WCAG option lists levels AA and AAA.
- Do not write "Free" in a tool's `description`.
- Name a tool that produces a document a "generator", not a "builder" ("policy generator", "RFP generator"). People search for "generator" and "template", so put "template" in the description. Put the tool at `/tools/{slug}/`, with its `done` and `print` pages under it.
- A tool with more than one field puts the extra fields under the main field, always visible. Do not hide them in a `<details>` or accordion. Capture them with `{% set toolFormExtra %}...{% endset %}` before the include. Set `toolFieldName` when the main field is not a site (it sets the input name and id), and `toolFormAction` when the form submits to another page.
- No extraneous spacing on forms. The shared form is a `row g-3`, so the gutters set the space. Put each extra field in a `col-*` div. Do not add `mb-*`, `mt-*` or `p-*` classes to fields, fieldsets or the button. Where a field needs space between its own parts (such as a stack of inputs), use `gap-*`. The same goes for the whole page: add no margin or padding classes unless asked.
- Every field in the form is the same size (`form-control-lg`, `form-select-lg`). Do not mix sizes.
- No placeholder on a field unless it shows a format the person must follow. Website and owner fields get none.
- List the options in the order of the ScanGov indicators the tool covers, with the one the tool is about first.
- A field that can repeat (websites, tools) starts with one field, or none, and an "Add" `btn-sm` button. The button is disabled until every field in the list is filled in. Reset removes the added fields.
- Optional settings go in one collapsed accordion below the main fields, labelled "Customize" with the `fa-sliders` icon (`<i class="fa-solid fa-sliders me-2" aria-hidden="true"></i>`). Open it when answers are restored. Use this icon and label for every Customize group on every site.
- Show only options that change the result. If an option does not change the output, cut it.
- The tool's script binds to the form by `toolFormId`. Do not bind to a CSS class.

---

## Tools that generate a document

The policy generator and the RFP generator submit to a `done` page and a `print` page, both `sitemap: false` and `noindex: true`. The done page uses `_includes/builder-actions.html` (Edit, Link, Copy, Download, Print, Reset; Edit comes first and Reset last; Reset links to the blank form, so it clears the answers) and `public/js/builder-actions.js`. Do not write inline `onclick` handlers. Do not ship a button for something that is not built.

---

## Changing the shared form

Edit `_includes/tool-form.html` in ScanGov/components first, then copy it to scangov-com. Always do a full sync of any shared file: copy the whole file to every site, never only your lines (see the `buttons` skill, "Where this lives"). Both tool pages must still render the same way afterward.
