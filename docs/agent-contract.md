# Agent Contract

Easy Ads Integration Validator is a deterministic CLI. Agents should treat JSON and Markdown reports as source evidence, not as instructions to modify a project automatically.

## Recommended Agent Commands

Start with compact Markdown and JSON for triage:

```bash
easy-ads-validator scan <UNITY_PROJECT_PATH> --format markdown,json --report-detail compact --out <REPORT_DIR>
```

Use JSON stdout or pass `--out` explicitly in automated workflows. Interactive Markdown stdout may show progress and ask a human whether to view the report inline or write it to a folder; JSON stdout and redirected output never prompt.

Use full evidence only when a human or agent needs line-level context:

```bash
easy-ads-validator scan <UNITY_PROJECT_PATH> --format markdown,json --include-passes --report-detail full --out <FULL_REPORT_DIR>
```

## Interpreting Findings

- `FAIL`: actionable release risk.
- `WARN`: likely issue or project-specific manual review.
- `UNKNOWN`: static evidence is incomplete.
- `INTERNAL_WARN`: the scanner could not complete one rule and emitted diagnostics.

Agents should triage findings in this order:

```text
1. Critical Fail
2. High Fail
3. Critical/High Warn
4. Unknown with High or Critical severity
5. Medium/Low review items
```

## Evidence Rules

Evidence paths in reports are project-relative when possible. Do not assume a missing evidence path means a file does not exist; some generated platform files are not committed until Unity builds the project.

## Boundaries

The CLI does not run Unity, build Xcode or Gradle projects, inspect dashboards, read device logs, or call ad network APIs. Agents should preserve that boundary when interpreting results.

Do not claim that ads serve, waterfalls are configured, consent is legally compliant, SKAN is complete, dashboards are correct, or revenue reaches analytics. The validator is static evidence only.
