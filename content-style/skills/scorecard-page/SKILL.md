---
name: scorecard-page
description: How to build or change the public scorecard (my.scangov.com /public/:orgkey/:siteslug/report) and the matching report pages on scangov.org. Use when editing scorecard layout, grade cards, indicator order, light and dark mode, or scorecard copy. Companion to content-style for prose.
---

# Scorecard page

The public scorecard shows a site's overall grade and one grade per indicator. It has no login. These rules keep it the same across my.scangov.com, scangov.org, and the docs.

---

## Where things live

- my.scangov.com: `src/http/get-public-000orgkey-000siteslug-report/index.mjs` handles the route. The page body, the indicator sort, the description, and the JSON-LD come from `src/shared/scorecard.mjs`. Cards come from `src/shared/gradecard.mjs`.
- my.scangov.com layout: `src/views/bootstrap-public.mjs`.
- scangov.org: `content/profile/report.html` is the print report. It uses the same card markup.
- components: `_includes/grade-card.html` and `README.md` ("Scorecard") describe the card contract. `public/css/scangov.css` holds the styles and is copied to every site.
- docs: `content/scorecard.md`.

---

## Rules

- **Theme.** Light and dark follow the system setting (`prefers-color-scheme`). Never add or suggest a theme toggle, a theme switcher, or a saved theme choice.
- **Order.** Overall first, then indicators alphabetically by name. Do not pin any indicator last.
- **Cards.** Use `gradeCard` only. Put the grade in a `div.display-1`, not a `p`. The grade is always a letter, so color is never the only signal. Do not add `text-center` to `.card-footer`; footers are left-aligned.
- **Footers.** Scorecard cards have no footer. Do not add a trend line or score-change text under a grade.
- **Icons.** Icon colors are black in light mode. When you add an icon, check `scangov.css` for a color rule and sync any change to all sites.
- **SEO.** Each scorecard has its own title and description, a canonical URL, and JSON-LD. Keep the sitemap entry in `src/http/get-sitemap_xml/index.mjs` in step with the public setting.
- **Copy.** Plain language, active voice, and no government-specific wording (see `content-style`). The scorecard covers all five indicators.

---

## Checks before you finish

1. Open the scorecard in light and dark mode and check every card, icon, and link.
2. Run `e2e/scorecard.spec.js` at "commit and pr" time.
3. Update `docs/content/scorecard.md` if the page changes.
