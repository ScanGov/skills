---
name: tool-page
description: How to build a free tool page on scangov.com (ScanGov/scangov-com repo, content/tools/). Use when adding or changing a tool, such as the good bot scan or readability scan, so every tool shares one layout and one set of form styles. Companion to content-style for prose and feature-announcement for the features listing and news post.
---

# Tool page (scangov.com)

Every tool under `content/tools/` uses the same page shell and the same form. Do not restyle the form per tool.

---

## Page shell

- Front matter: `layout: layouts/default`, `isTool: true`, `permalink: /tools/{slug}/`, plus `title` and `description`.
- Body: `container` > `row` > three columns, `col-md-2` (empty), `col-md-8` (the tool), `col-md-2` (empty).
- Order inside the middle column: the form, then a loading block (if the tool needs one), then the results block, then the `alert alert-info` "About" box with the tool's explanation and FAQ.

Reference pages: `content/tools/good-bot-scan.html` and `content/tools/readability/index.html`.

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

- The field is `name="site"` `id="site"`, with a required-field asterisk, `form-control-lg`, and a `btn btn-primary` submit button with `id="submitbutton"` (see the `buttons` skill for button rules).
- The button says what the tool does in one word, such as "Check". Do not use "Submit".
- A tool with more than one field puts extra fields in a `<details>` labelled "Options" under the main field, as the policy generator does.
- The tool's script binds to the form by `toolFormId`. Do not bind to a CSS class.

---

## Changing the shared form

Edit `_includes/tool-form.html` in ScanGov/components first, then copy it to scangov-com. Both tool pages must still render the same way afterward.
