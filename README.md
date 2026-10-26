# ScanGov skills

Claude Code plugin marketplace for skills shared across ScanGov's repos.

## Install

```
/plugin marketplace add ScanGov/skills
/plugin install content-style@scangov
```

## Plugins

- **content-style** — plain-language, inclusive content style rules for site copy (`content-style` skill), plus a stricter guide for `audits.json` `description`/`risk` fields (`attribute-content-guide` skill). Enforced on PRs via the shared checker in [ScanGov/components](https://github.com/ScanGov/components/tree/main/content-lint). Also includes:
  - `report-format` — structure and tooling for scangov-com's data-journalism "report" posts.
  - `docs-content` — where and how to add or update a page on docs.scangov.org when a feature ships or changes.
  - `scorecard-page` — rules for the public scorecard: cards, indicator order, system light/dark theme, trend footers, and SEO.
  - `tool-page` — shared page shell and form for free tools on scangov.com.
  - `feature-announcement` — how to reflect a shipped or changed feature on scangov.com (the features listing and a news post).
