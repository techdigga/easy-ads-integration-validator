# Agent Contract

Easy Ads Integration Validator is a deterministic CLI. Agents should treat JSON, Markdown, and SARIF reports as source evidence, not as instructions to modify a project automatically. The current public package is `1.0.0`; the report schema is independently versioned at `1.0`.

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

Exit code `0` means the scan completed below the configured failure threshold. Exit code `1` means completed findings met that threshold. Exit code `2` means invalid input/configuration or an incomplete/partial scan; check `summary.isPartial` and `SCAN_PARTIAL` before treating the report as complete. Exit code `3` means an unexpected internal error.

LevelPlay examples:

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

For LevelPlay, `FAIL` is a high-confidence static integration problem, `WARN` is a review signal for unresolved evidence or source-level callback/load/thread risks, and `UNKNOWN` means files under the supplied root cannot prove the result. None of these statuses certifies callback delivery, runtime behavior, dashboards, generated builds, or native dependency resolution. The profile supports SDK `9.0.0+`.

Agents should triage findings in this order:

```text
1. Critical Fail
2. High Fail
3. Critical/High Warn
4. Unknown with High or Critical severity
5. Medium/Low review items
```

## Evidence Rules

Evidence paths in reports are project-relative when possible. Do not assume a missing evidence path means a file does not exist; some generated platform files may not exist under the supplied root until Unity builds the project.

## Boundaries

The CLI does not run Unity, build Xcode or Gradle projects, inspect dashboards, read device logs, or call ad network APIs. Agents should preserve that boundary when interpreting results.

Do not claim that ads serve, waterfalls are configured, consent is legally compliant, SKAN is complete, dashboards are correct, or revenue reaches analytics. The validator is static evidence only.

Before sharing a report outside the project team, review redaction and remove project-specific paths, source excerpts, SDK identifiers, credentials, and personal data. Output redaction is heuristic and does not replace secret scanning or access control.

## MCP onboarding

Install both tools at the same version and ensure the global .NET tools directory is on `PATH`:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 1.0.0
dotnet tool install --global EasyAdsIntegrationValidator.Mcp --version 1.0.0
export PATH="$PATH:$HOME/.dotnet/tools"
easy-ads-validator --version
```

Codex local stdio configuration:

```toml
[mcp_servers.easy_ads_validator]
command = "easy-ads-validator-mcp"
args = []
env = { PATH = "/absolute/path/to/dotnet-tools:/usr/local/bin:/usr/bin:/bin", ADS_AUDIT_CLI_COMMAND = "/absolute/path/to/easy-ads-validator" }
```

Claude Code registration:

```bash
claude mcp add --transport stdio easy-ads-validator -- easy-ads-validator-mcp
claude mcp list
```

For Claude Code or another MCP client, configure the same stdio command and environment variables through that client's documented server settings. Set `ADS_AUDIT_CLI_COMMAND` to an absolute `easy-ads-validator` path when the client has a different `PATH`; do not include shell arguments in that value. Use `scan_project`, read the compact summary first, and request individual findings as needed.
