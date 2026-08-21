# Configuration

The public beta supports embedded default policy plus an optional local JSON policy passed with `--policy <path>`. Local policy values override defaults. YAML and remote policy loading are not implemented. Select `levelplay-unity` with `--profile levelplay-unity` for LevelPlay SDK 9+ scans; the default remains `max-unity`. MAX-specific policy values under `maxUnity` apply only to MAX scans.

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

Unknown or malformed `maxUnity.apiAliases` entries do not fail policy loading. They are ignored and reported as diagnostics so beta scans continue while still surfacing policy typos.

## Report detail

`--report-detail compact` is the default and is intended for normal terminal and agent usage. It caps evidence per finding and records omitted counts.

Markdown reports are optimized for humans: they include a stable `## Readiness` section, short score guidance, the line `Focus first on Critical and High Fail findings. Warn and Unknown items are static-analysis review prompts, not runtime proof.`, and restrained symbols for severity/status cues. Markdown findings are ordered for action: `FAIL` and `INTERNAL_WARN` blockers first, then manual-review warnings, other warnings, unknown/static-limit items, and passes; each group is ordered by severity and then rule ID. JSON reports use deterministic status, severity, and rule-ID ordering for agents and CI, keep stable enum/string values, and do not include Markdown-only labels, symbols, emoji, or guidance copy.

Use `--report-detail full --include-passes` when archiving a complete evidence report.

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

If a project intentionally uses a smaller or different network set, set this list in local policy. Static evidence may come from Unity packages, EDM4U resolver files/settings, Android Gradle files, iOS pods/plists/postprocess files, or committed plugin artifacts.

The same list also seeds the MAX `Ad Network Matrix` report section. The matrix may include additional detected networks when adapter evidence appears in committed files. It only treats Android dependencies as resolved when committed resolver evidence contains AppLovin mediation adapter artifacts such as `com.applovin.mediation:*adapter*` or `AppLovinMediation*Adapter`; third-party SDK libraries alone are not counted as MAX adapter resolution.

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
| `skanIds` | `MAX014` is currently skipped by default pending the dedicated SKAN evidence model; disabling this requirement still produces a pass. |
| `revenueTracking` | `MAX080`–`MAX082` are neutral when false and no revenue intent is visible; explicit policy true makes missing non-app-open callback evidence a warning. |
| `qualityService` | `MAX090` always warns when quality/ad review evidence is absent; true changes the wording to a policy request. |
| `childDirectedFlags` | `MAX044` warns when true and no age-related flag evidence is found; missing evidence remains an advisory warning even when false. |

## Remote-policy-ready boundary

The policy layer is designed around `IPolicyProvider` and profile policy parsers. The MVP implements embedded defaults and local JSON only. Future remote policy should remain data-only: versions, severities, required network IDs, known adapter tokens, expected analytics sinks, and platform thresholds. Remote executable rules are out of scope for the MVP.
