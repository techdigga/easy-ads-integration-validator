# Agent Contract

Easy Ads Integration Validator is a deterministic CLI. Agents should treat JSON and Markdown reports as source evidence, not as instructions to modify a project automatically.

## Recommended Agent Commands

Start with compact Markdown and JSON for triage:

```bash
easy-ads-validator scan <UNITY_PROJECT_PATH> --format markdown,json --report-detail compact --out <REPORT_DIR>
```

Use JSON stdout or pass `--out` explicitly in automated workflows. Interactive Markdown stdout may show progress and ask a human whether to view the report inline or write it to a folder; JSON stdout and redirected output never prompt.

Use `--format sarif --out <REPORT_DIR>` when the CI platform consumes SARIF. The SARIF artifact contains only non-passing findings and is suitable for GitHub Code Scanning-compatible workflows.

Use full evidence only when a human or agent needs line-level context:

```bash
easy-ads-validator scan <UNITY_PROJECT_PATH> --format markdown,json --include-passes --report-detail full --out <FULL_REPORT_DIR>
```

The stable JSON contract is [`audit-report.schema.json`](schemas/audit-report.schema.json), currently version `1.0`. Agents should use stable fields and statuses, ignore unknown additive fields, and request migration guidance for an unsupported major version. The schema version is separate from the CLI package version and mediation SDK versions.

LevelPlay Beta 11 examples:

```bash
easy-ads-validator scan ./MyUnityProject --mediation levelplay --profile levelplay-unity --format markdown
easy-ads-validator scan ./MyUnityProject --profile levelplay-unity --format json --summary-only
```

The equivalent MCP `scan_project` request is:

```json
{
  "projectPath": "/absolute/path/to/unity-project",
  "mediation": "levelplay",
  "profile": "levelplay-unity",
  "reportDetail": "compact",
  "includePasses": false
}
```

`mediation` and `profile` are optional and default to `max` and `max-unity` for backward-compatible MAX scans. LevelPlay requests must supply `levelplay` and `levelplay-unity` as a matching pair. Use the compact summary first, then request individual `LP001`-`LP040` findings through MCP when evidence is needed. Use `get_rule_docs` with `profile: "levelplay-unity"` to retrieve the matching rule explanation. The LevelPlay profile is static-only and supports SDK `9.0.0+`; known pre-v9 versions fail its version check.

For unusually large projects, a scan may receive an optional local `--limits <path>` JSON configuration with `maxFileBytes`, `maxFiles`, `phaseTimeoutSeconds`, and `totalTimeoutSeconds`. Agents must treat `summary.isPartial: true` or a `SCAN_PARTIAL` diagnostic as incomplete output and request a rerun or manual review before using the findings as a complete audit.

Reports also include an additive `performance` object with total scan-preparation duration, ordered phase timings, claimed-file count, skipped-path count, diagnostic count, and partial status. It contains only bounded counters and timings; report-file serialization is outside the measured duration.

Long-lived hosts may reuse a process-local C# syntax index for unchanged source files and scan namespaces. This cache does not persist source-derived data to disk and does not change report semantics. Use `--no-cache` when a fresh index is required.

## Interpreting Findings

- `FAIL`: actionable release risk.
- `WARN`: likely issue or project-specific manual review.
- `UNKNOWN`: static evidence is incomplete.
- `INTERNAL_WARN`: the scanner could not complete one rule and emitted diagnostics.

For LevelPlay, `FAIL` is a high-confidence static integration problem, `WARN` is a review signal for unresolved evidence or source-level callback/load/thread risks, and `UNKNOWN` means committed files cannot prove the result. None of these statuses certifies callback delivery, runtime behavior, dashboards, generated builds, or native dependency resolution.

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
