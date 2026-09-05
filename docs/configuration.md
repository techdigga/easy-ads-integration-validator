# Configuration

The current public package supports embedded default policy plus an optional local JSON policy passed with `--policy <path>`. Local policy values override defaults. YAML and remote policy loading are not available. Select `levelplay-unity` with `--mediation levelplay --profile levelplay-unity` for LevelPlay SDK `9.0.0+` scans; the default remains `max-unity`. MAX-specific policy values under `maxUnity` apply only to MAX scans.

## Scan scope

Scans use a bounded default scope. Supported files present under the supplied Unity project root are eligible, including untracked or uncommitted files. Generated Unity directories (`Library`, `Temp`, `Obj`, `Build`, `Builds`, `Logs`, `UserSettings`, and `.git`) and bulky content directories such as `Art`, `Audio`, `Models`, `Prefabs`, `Textures`, and `Videos` are skipped during recursive source and asset-settings enumeration. Required project settings and mediation SDK evidence paths remain explicitly inspected by their readers. Scan an intended copy of the project and review redaction before sharing reports.

Use `--exclude-path <path>` to exclude an additional project-relative path. Repeat the option for multiple paths. Use `--include-path <path>` to force a path and its ancestors into recursive enumeration, including a path that matches a default exclusion or a C# SDK-source exclusion. Include paths take precedence over matching excludes and are restricted to the project root.

```bash
easy-ads-validator scan . --exclude-path Assets/Art --exclude-path Assets/Generated
easy-ads-validator scan . --include-path Assets/Plugins/AdNetwork --verbose
```

Scope decisions are deterministic and are surfaced as a bounded verbose diagnostic with the number, reason, and relative-path examples of skipped paths. Scope diagnostics never include source contents or credential values.

## Scan resource limits

Normal scans use bounded defaults so an unusually large or problematic project cannot consume unbounded memory or remain apparently stalled. Configure stricter local limits with `--limits <path>` and a JSON file:

```json
{
  "maxFileBytes": 4194304,
  "maxFiles": 100000,
  "phaseTimeoutSeconds": 300,
  "totalTimeoutSeconds": 900
}
```

`maxFileBytes` is checked before a supported file is read or parsed. `maxFiles` bounds the files claimed by the scan. Phase and total timeouts cancel work defensively. When input is skipped or cancellation interrupts a scan, the report contains `summary.isPartial: true`, a `SCAN_PARTIAL` diagnostic, and an explicit limitation; partial output exits with code `2` and must not be treated as a complete audit. A partial scan does not convert unavailable evidence into a proven `FAIL`.

## Policy schema

```json
{
  "profile": "max-unity",
  "severityOverrides": {
    "MAX092": "critical"
  },
  "maxUnity": {
    "minimumUnityVersion": "2022.3.62f2",
    "minimumMaxPluginVersion": "8.6.0",
    "minimumExternalDependencyManagerVersion": "1.2.185",
    "android": {
      "minimumApi": 24,
      "targetApiAtLeast": 36,
      "requireJetifier": true
    },
    "ios": {
      "minimumDeploymentTarget": "13.0",
      "recommendedDeploymentTarget": "15.0",
      "requireAttDescription": true,
      "requireSkanIds": true
    },
    "requiredNetworks": [
      "google-admob",
      "unity-ads",
      "ironsource",
      "meta",
      "mintegral",
      "pangle"
    ],
    "analyticsRevenueSinksMode": "recognized",
    "analyticsRevenueSinks": [
      "firebase",
      "appsflyer",
      "adjust"
    ],
    "requirements": {
      "consentFlow": true,
      "attDescription": true,
      "skanIds": true,
      "revenueTracking": true,
      "qualityService": false,
      "childDirectedFlags": false
    }
  }
}
```

See `examples/ads-audit.json` for a complete editable policy file.

Unknown or malformed `maxUnity.apiAliases` entries do not fail policy loading. They are ignored and reported as diagnostics so scans continue while still surfacing policy typos.

## Report detail

`--report-detail compact` is the default and is intended for normal terminal and agent usage. It caps evidence per finding and records omitted counts.

Markdown reports are optimized for humans: they include a stable `## Readiness` section, short score guidance, the line `Focus first on Critical and High Fail findings. Warn and Unknown items are static-analysis review prompts, not runtime proof.`, and restrained symbols for severity/status cues. Markdown findings are ordered for action: `FAIL` and `INTERNAL_WARN` blockers first, then manual-review warnings, other warnings, unknown/static-limit items, and passes; each group is ordered by severity and then rule ID. JSON reports use deterministic status, severity, and rule-ID ordering for agents and CI, keep stable enum/string values, and do not include Markdown-only labels, symbols, emoji, or guidance copy.

Readiness scoring starts at 100 and is reduced by proven failures: critical, high, and medium failures cost 20, 12, and 8 points. High warnings cost 2 points; lower-severity warnings do not reduce the score. Critical-severity warnings are normalized to high severity because warnings are advisory and are never counted as critical issues. Unknown critical/high findings cost 2 points as static-analysis limits. The score is clamped to 0 and is separate from the `--fail-on` exit-code threshold.

Use `--report-detail full --include-passes` when archiving a complete evidence report.

## Exit codes

Exit code `0` means the scan completed below the configured failure threshold. Exit code `1` means completed findings met that threshold. Exit code `2` means invalid input/configuration or an incomplete/partial scan; check JSON `summary.isPartial` and the `SCAN_PARTIAL` diagnostic before treating the report as complete. Exit code `3` means an unexpected internal error.

## Report secret redaction

Report writers apply deterministic, output-only redaction before serialization. The redactor masks common SDK/app-key assignments and initialization arguments, known AdMob identifiers, credential-bearing URL userinfo, and raw evidence snippets whose symbols are clearly sensitive. It preserves file paths, line numbers, rule IDs, SDK versions, and surrounding method or field names so findings remain actionable.

Redaction is heuristic and intentionally bounded. It is not a complete secret scanner and does not prove that a project contains no credentials. Agents should treat `[REDACTED]` as an opaque value and never attempt to reconstruct it. Keep source projects and full raw evidence private when sharing reports.

## JSON report contract

The stable machine-readable contract is published as [`audit-report.schema.json`](schemas/audit-report.schema.json). It describes the report identity, scan options, summary and partial-scan flags, performance metadata, finding statuses and severities, evidence, diagnostics, remediation, mediated-network matrix, and analytics inventory. Markdown output may evolve independently.

The optional `performance` object reports `totalDurationMs`, ordered phase durations, `claimedFileCount`, `skippedPathCount`, `diagnosticCount`, and `isPartial`. The duration ends at report preparation; writing the final report files is not included. These values help identify slow or incomplete scans without exposing source contents or credentials.

The current schema version is `1.0`. Consumers should accept the same major version, ignore unknown additive fields, and fail closed or request migration guidance for an unknown major version. Breaking field or meaning changes increment the major version; additive compatible fields may increment the minor version. The `schemaVersion` field is independent from the CLI package version and mediation SDK versions.

Agent triage command:

```bash
easy-ads-validator scan . --format json --report-detail compact
```

Full evidence archive:

```bash
easy-ads-validator scan . --format markdown,json --out audit-report --include-passes --report-detail full
```

## Default values

| Field | Default |
| --- | --- |
| `profile` | `max-unity` |
| `minimumUnityVersion` | `2022.3.62f2` |
| `minimumMaxPluginVersion` | `8.6.0` |
| `minimumExternalDependencyManagerVersion` | `1.2.185` |
| `android.minimumApi` | `24` |
| `android.targetApiAtLeast` | `36` |
| `android.requireJetifier` | `true` |
| `ios.minimumDeploymentTarget` | `13.0` |
| `ios.recommendedDeploymentTarget` | `15.0` |
| `ios.requireAttDescription` | `true` |
| `ios.requireSkanIds` | `true` |
| `requiredNetworks` | `google-admob`, `unity-ads`, `ironsource`, `meta`, `mintegral`, `pangle` |
| `analyticsRevenueSinksMode` | `recognized` |
| `analyticsRevenueSinks` | `firebase`, `appsflyer`, `adjust` |
| `requirements.consentFlow` | `true` |
| `requirements.attDescription` | `true` |
| `requirements.skanIds` | `true` |
| `requirements.revenueTracking` | `false` |
| `requirements.qualityService` | `false` |
| `requirements.childDirectedFlags` | `false` |

## Severity overrides

`severityOverrides` maps a rule ID to an effective severity:

```json
{
  "severityOverrides": {
    "MAX001": "critical",
    "MAX023": "medium",
    "MAX092": "critical"
  }
}
```

Valid severities are `info`, `low`, `medium`, `high`, and `critical` case-insensitively. Overrides affect reporting and `--fail-on <severity>` exit-code behavior. They do not change a rule's status from `WARN` to `FAIL`.

## Required networks

`maxUnity.requiredNetworks` drives MAX rules that compare expected mediated demand sources with static dependency evidence. LevelPlay scans use the LevelPlay profile's adapter catalog and matrix policy. The default MAX list is:

- `google-admob`
- `unity-ads`
- `ironsource`
- `meta`
- `mintegral`
- `pangle`

If a project intentionally uses a smaller or different network set, set this list in local policy. Static evidence may come from Unity packages, EDM4U resolver files/settings, Android Gradle files, iOS pods/plists/postprocess files, or plugin artifacts present under the supplied root.

The same list also seeds the MAX `Ad Network Matrix` report section. The matrix may include additional detected networks when adapter evidence appears under the supplied root. It only treats Android dependencies as resolved when resolver evidence contains AppLovin mediation adapter artifacts such as `com.applovin.mediation:*adapter*` or `AppLovinMediation*Adapter`; third-party SDK libraries alone are not counted as MAX adapter resolution.

## Analytics revenue sink mode

`analyticsRevenueSinks` lists sink tokens used by revenue forwarding checks. The default values are `firebase`, `appsflyer`, and `adjust`.

`analyticsRevenueSinksMode` controls `MAX081`:

- `recognized`: pass when the revenue callback forwards to at least one configured/recognized sink.
- `all`: pass only when the revenue callback forwards to all configured sinks.

Any other value is treated like `recognized` by the current rule implementation.

## Requirement booleans

The `requirements` object controls whether selected checks are required:

| Field | Effect |
| --- | --- |
| `consentFlow` | `MAX040` passes immediately when false; otherwise consent evidence is required when MAX use is detected. |
| `attDescription` | `MAX013` passes immediately only when both `requirements.attDescription` and `ios.requireAttDescription` are false; otherwise ATT usage description evidence is expected. |
| `skanIds` | `MAX014` does not assess SKAdNetwork identifiers in this profile; disabling this requirement still produces a pass. |
| `revenueTracking` | `MAX080`–`MAX082` are neutral when false and no revenue intent is visible; explicit policy true makes missing non-app-open callback evidence a warning. |
| `qualityService` | `MAX090` always warns when quality/ad review evidence is absent; true changes the wording to a policy request. |
| `childDirectedFlags` | `MAX044` warns when true and no age-related flag evidence is found; missing evidence remains an advisory warning even when false. |

## Policy limits

Only embedded defaults and local JSON policy are available in the current public package. A policy file can change supported thresholds and requirements; it cannot add executable rules, dashboard checks, network access, or runtime validation.
