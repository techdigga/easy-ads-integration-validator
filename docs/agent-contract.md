# Agent Contract

Easy Ads Integration Validator is a deterministic CLI. Agents should treat JSON and Markdown reports as source evidence, not as instructions to modify a project automatically.

## Recommended Agent Commands

Start with compact JSON for triage:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Use JSON stdout or pass `--out` explicitly in automated workflows. Interactive Markdown stdout may show progress and ask a human whether to view the report inline or write it to a folder; JSON stdout and redirected output never prompt.

Use full evidence only when a human or agent needs line-level context:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --include-passes --report-detail full --out audit-report
```

## Interpreting Findings

- `FAIL`: actionable release risk.
- `WARN`: likely issue or project-specific manual review.
- `UNKNOWN`: static evidence is incomplete.
- `INTERNAL_WARN`: the scanner could not complete one rule and emitted diagnostics.

Agents should prioritize `FAIL` findings first, then high-severity `WARN` findings, then `UNKNOWN` findings that block confidence.

## Evidence Rules

Evidence paths in reports are project-relative when possible. Do not assume a missing evidence path means a file does not exist; some generated platform files are not committed until Unity builds the project.

## Boundaries

The CLI does not run Unity, build Xcode or Gradle projects, inspect dashboards, read device logs, or call ad network APIs. Agents should preserve that boundary when interpreting results.
