# Easy Ads Integration Validator

### Find Unity mediation integration gaps before they become build failures, lost revenue, or release surprises.

Easy Ads Integration Validator is a developer-first static audit tool for Unity game teams using AppLovin MAX or Unity LevelPlay. It reads supported files present under the supplied project root, including untracked and uncommitted files, explains what it found, and produces focused Markdown or JSON reports that developers, CI jobs, Codex, and Claude can use immediately.

Keywords: Unity, AppLovin MAX, LevelPlay, ad mediation, static analysis, .NET CLI, MCP, Codex, Claude, and game development.

**One scan. Clear evidence. Faster fixes.**

## Why Teams Use It

- Review a MAX integration without opening Unity or building the project.
- Catch missing SDK setup, adapters, dependencies, privacy configuration, callbacks, and ad lifecycle wiring early.
- Get status, severity, confidence, evidence, remediation, and limitations for every finding; documentation links apply to non-pass findings when relevant.
- Use compact JSON in CI or hand targeted evidence to an AI coding tool for explanation and fix planning.
- Keep the validator deterministic, local, read-only, and independent of dashboard APIs.

## What It Validates

The current public package is `1.0.0`. It supports AppLovin MAX mediation and Unity LevelPlay mediation in Unity. MAX remains the default profile. The LevelPlay profile supports Unity LevelPlay SDK `9.0.0+` and covers the corresponding static integration areas with LevelPlay-specific APIs, settings, callbacks, loading, revenue, and adapter evidence.

## Supported versions

| Component | Supported public package baseline |
| --- | --- |
| .NET runtime | `8`, `9`, or `10` |
| Unity editor evidence | Unity `2022.3.62f2` or newer by default; local policy can raise the floor |
| AppLovin MAX Unity plugin | `8.6.0` or newer by default |
| External Dependency Manager for Unity | `1.2.185` or newer by default when MAX evidence is present |
| Unity LevelPlay SDK | `9.0.0` or newer with `levelplay-unity` |

These are validator policy floors, not a guarantee that a mediation SDK supports every Unity or platform combination. Confirm platform compatibility in the official [MAX Unity integration guide](https://developers.applovin.com/en/max/unity/overview/integration/) or [LevelPlay Unity documentation](https://docs.unity.com/grow/levelplay/sdk/unity).

The coverage table uses both mediation profiles: MAX terminology identifies the default profile, while LevelPlay applies the corresponding SDK 9+ APIs, settings, adapters, app keys, callbacks, and policy-owned evidence.

| Area | Examples of validation coverage |
| --- | --- |
| **SDK foundation** | MAX or LevelPlay SDK import through `Assets` or UPM, duplicate installation risk, supported SDK version, assembly definitions, settings, app-key evidence, and required project structure. |
| **Dependency and adapter setup** | External Dependency Manager/EDM4U presence and version, resolver settings, MAX or LevelPlay adapter/network detection, dependency XML evidence, Android library resolution, and a mediation network matrix. |
| **Consent and privacy** | MAX privacy-flow or LevelPlay privacy settings, consent/privacy evidence, privacy URL requirements, ATT-related code paths, and conflicting direct consent/ATT ownership. |
| **Unity and platform readiness** | Unity version policy, Android API levels, Gradle and resolver settings, Android manifest signals, iOS deployment target evidence, and platform-specific configuration available under the supplied project root, including untracked and uncommitted files. |
| **Keys, app IDs, and ad units** | MAX SDK keys or LevelPlay app keys, Google/AdMob app IDs when relevant, ad unit presence by format and platform, duplicate IDs, placement tracking, and default local IDs when remote configuration is used. |
| **Initialization and lifecycle** | MAX or LevelPlay initialization call sites, singular initialization, staged guards, callback subscription timing, initialization failure/completion state handling, and load sequencing after initialization. |
| **Loading, showing, and retry** | Interstitial and rewarded readiness checks, load/show callback wiring, reload after close or display failure, banner refresh and destruction behavior, retry coverage, and fixed or exponential backoff patterns. |
| **Events and revenue** | Display, click, impression, paid-revenue, and ad-format callbacks for both profiles; event forwarding to detected Firebase, AppsFlyer, and Adjust integrations; expected revenue event names and payload fields; and background-callback UI risks. |
| **Production safety** | MAX Creative Debugger or LevelPlay diagnostic settings, debug logging, production flags, unsafe UI work in background callbacks, and other release-safety signals. |

### Unity LevelPlay profile

Select the `levelplay-unity` profile explicitly:

```bash
easy-ads-validator scan ./MyUnityProject --mediation levelplay --profile levelplay-unity --format markdown
easy-ads-validator scan ./MyUnityProject --profile levelplay-unity --format json --summary-only
```

The profile recognizes direct imports under `Assets/LevelPlay` and legacy-compatible `Assets/IronSource`, plus UPM package IDs `com.unity.services.levelplay` and legacy `com.ironsource.mediation`. It rejects known SDK versions before `9.0.0` and reports `LP001`-`LP040` for installation, version, duplicate installation, settings, assembly definitions, platform configuration, app keys, local fallback ad units, placement arguments, adapter dependency evidence, initialization, callback wiring, load ordering, thread/UI safety, consent, privacy URL, ATT ownership, audience-signal evidence, duplicate or unsupported callback subscriptions, revenue/analytics forwarding, and production safety. See Unity's [LevelPlay package integration guide](https://docs.unity.com/en-us/grow/levelplay/sdk/unity/package-integration) and [LevelPlay initialization guide](https://docs.unity.com/grow/levelplay/sdk/unity/migrate-to-init-api).

| Rule | Severity | Confidence and status semantics | Remediation |
| --- | --- | --- | --- |
| `LP001` | Critical | High-confidence `FAIL` when no recognized direct/UPM installation evidence exists. | Install one recognized LevelPlay SDK form. |
| `LP002` | High | High-confidence `FAIL` for known versions below `9.0.0`; low-confidence `UNKNOWN` when authoritative version evidence is missing or malformed. | Use LevelPlay SDK `9.0.0+` and consistent package metadata. |
| `LP003` | High | High-confidence `FAIL` when direct and UPM installations are both present. | Keep one installation source. |
| `LP004` | High | High-confidence `FAIL` when settings evidence is missing or malformed. | Restore valid LevelPlay settings evidence. |
| `LP005` | High | High-confidence `FAIL` when required assembly-definition evidence is missing or malformed. | Restore the required LevelPlay assembly definition. |
| `LP006` | Medium | High-confidence `WARN` for unresolved or conflicting adapter/dependency evidence under the supplied root; low-confidence `UNKNOWN` when matrix evidence is unavailable. | Review static adapter metadata and resolver evidence. |

`FAIL`, `WARN`, and `UNKNOWN` are static report semantics. They do not prove callback execution, runtime behavior, dashboard state, generated builds, native output, or dependency resolution on a device.

LevelPlay matrix statuses are `missing`, `resolved`, `needs-resolution-evidence`, `conflict`, and `platform-gap`. A `resolved` row means matching resolver evidence is present under the supplied root; it does not mean that Unity, Gradle, CocoaPods, or a device resolved the dependency.

Static analysis reports evidence and uncertainty. It does not pretend that a source-code pattern proves runtime behavior.

## AI-Friendly By Design

The validator is built to work with modern coding agents without turning the scanner itself into an opaque AI system.

- **Compact JSON reports** keep the first response small enough for agent context windows.
- **Full evidence reports** provide the exact files and lines needed for a targeted fix.
- **MCP support** exposes local read-only scan, summary, finding, and rule-documentation tools to Codex, Claude, and other MCP clients.
- **Stable rule IDs** let an agent request only the findings that need investigation.
- **Deterministic results** let the agent explain, suggest, and plan changes without replacing the source-of-truth validator.
- **Bounded scans** protect large projects with configurable file-size, file-count, phase, and total-time limits; interrupted scans are labeled partial instead of being presented as complete.

A practical workflow is:

1. Run a compact JSON scan.
2. Give the report to Codex or Claude, or let the MCP server retrieve the summary and selected findings.
3. Ask the agent to explain each missing or failing integration, identify the likely Unity files to change, and propose a minimal fix plan.
4. Review and apply the change through your normal development workflow.
5. Run the validator again to confirm that the static evidence changed as expected.

The validator does not silently edit Unity projects. Automatic approved-fix workflows are future work.

## Install

The CLI is distributed as a .NET global tool. Install .NET 8, .NET 9, or .NET 10 first, then install the current stable release:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 1.0.0
```

The installed command is `easy-ads-validator`:

```bash
easy-ads-validator --version
```

Update an existing installation:

```bash
dotnet tool update --global EasyAdsIntegrationValidator --version 1.0.0
```

Remove the CLI:

```bash
dotnet tool uninstall --global EasyAdsIntegrationValidator
```

For users who do not want a global .NET tool, self-contained macOS and Windows archives are attached to the [GitHub Releases](https://github.com/techdigga/easy-ads-integration-validator/releases) page. Verify `checksums.txt` before running an archive; see [Artifact Verification](docs/artifact-verification.md).

## First Scan

Point the command at the root folder of a Unity project:

```bash
easy-ads-validator scan /path/to/unity-project
```

Windows PowerShell example:

```powershell
easy-ads-validator scan "C:\Projects\My Unity Game"
```

The default profile is Unity AppLovin MAX. The equivalent explicit command is:

```bash
easy-ads-validator scan /path/to/unity-project --platform unity --mediation max --profile max-unity
```

The scan is read-only and does not require the Unity Editor to be installed or open.

## Reports

Generated reports are safer to share for common integration secrets: the Markdown, JSON, and SARIF writers mask credential-shaped SDK keys, app IDs, tokens, and URL credentials while retaining evidence locations and useful code context. Redaction is heuristic, not a full secret scanner; review reports before publishing them and never share the original Unity project or raw source snippets.

For CI and code-scanning integrations, use `--format sarif --out <REPORT_DIR>`. The SARIF report contains non-passing findings with stable rule IDs, severity, confidence, evidence locations, remediation, and official documentation links.

For CI and agent integrations, validate JSON against the published [audit report schema](docs/schemas/audit-report.schema.json). Schema version `1.0` is compatible with additive unknown fields; breaking contract changes use a new major version.

Markdown is the default human-readable output:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown
```

For CI and coding agents, write compact JSON:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Write both formats to a directory:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report
```

Include passing rules and full evidence when investigating a finding:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report --include-passes --report-detail full
```

Interactive terminal Markdown scans show progress and may ask whether to display the report or write it to a file. JSON stdout, redirected output, and explicit `--out` runs do not prompt, so they are safe for automation.

When a scan is partial because a limit, timeout, or cancellation interrupted it, the report sets `summary.isPartial` and the process exits with code `2`. Treat that result as incomplete and rerun it before using the findings as a release gate.

## CI And MCP

Fail CI when high-severity failed findings are present:

```bash
easy-ads-validator scan "$UNITY_PROJECT" --format json --report-detail compact --fail-on high
```

The command returns:

| Code | Meaning |
| --- | --- |
| `0` | The scan completed without a failed finding at the configured threshold. |
| `1` | The scan completed and at least one failed finding met the threshold. |
| `2` | Input/configuration is invalid, or the scan is incomplete/partial because of a limit, timeout, or cancellation. Check JSON `summary.isPartial` and the `SCAN_PARTIAL` diagnostic when present. |
| `3` | An unexpected internal error occurred. |

Install the optional MCP package at the same version as the CLI:

```bash
dotnet tool install --global EasyAdsIntegrationValidator.Mcp --version 1.0.0
easy-ads-validator-mcp
```

Register `easy-ads-validator-mcp` as a local stdio server in Codex, Claude, or another MCP client. It delegates to the same deterministic CLI, does not call an LLM, and does not modify Unity files. See the [agent contract](docs/agent-contract.md) for the tool boundary.

For a LevelPlay scan, call `scan_project` with a matching mediation/profile pair and compact report settings:

```json
{
  "projectPath": "/absolute/path/to/unity-project",
  "mediation": "levelplay",
  "profile": "levelplay-unity",
  "reportDetail": "compact",
  "includePasses": false
}
```

The `mediation` and `profile` fields are optional and default to `max` and `max-unity`. For LevelPlay, supply `levelplay` and `levelplay-unity` together.

## Configuration

Use a local JSON policy when the project needs different thresholds, required networks, API aliases, or other supported policy overrides:

```bash
easy-ads-validator scan /path/to/unity-project --policy examples/ads-audit.json
```

Read [Configuration](docs/configuration.md) and inspect the [sample policy](examples/ads-audit.json) before changing defaults.

## Boundaries

This is static repository inspection, not a build or runtime certification. It does not:

- Query AppLovin, AdMob, Firebase, AppsFlyer, Adjust, Apple, Google, or other dashboards/APIs.
- Run Unity, Gradle, Xcode, an emulator, a simulator, a device, or a game build.
- Prove that an ad loads, displays, earns revenue, or invokes a callback at runtime.
- Verify waterfalls, bidding, server-side reporting, or account configuration.
- Certify GDPR, ATT, TCF, COPPA, or other legal/privacy compliance.
- Modify the Unity project automatically.

`WARN` and `UNKNOWN` findings are review signals, not proof of runtime failure. Read the confidence, evidence, remediation, and limitations in the report.

## Documentation

- [Documentation index](docs/index.md): public guide map and recommended workflow.
- [Quickstart](docs/quickstart.md): install-to-first-scan walkthrough.
- [Configuration](docs/configuration.md): local JSON policy options.
- [MAX Unity profile](docs/max-unity-profile.md): supported MAX validation scope.
- [LevelPlay Unity profile](docs/levelplay-unity-profile.md): SDK `9.0.0+`, `LP001`-`LP040`, and static installation, platform, credential, dependency, initialization, callback, loading, revenue, consent, privacy, production-safety, and audience evidence.
- [Agent contract](docs/agent-contract.md): MCP tool behavior and response boundaries.
- [Artifact verification](docs/artifact-verification.md): checksums, download provenance, and current release trust limitations.

## Troubleshooting

- **`easy-ads-validator` is not found:** add `$HOME/.dotnet/tools` on macOS/Linux or `%USERPROFILE%\\.dotnet\\tools` on Windows to `PATH`, then open a new shell.
- **The command reports an unsupported profile:** use `max/max-unity` or `levelplay/levelplay-unity`; LevelPlay requires both values together.
- **A report is empty or unexpectedly `UNKNOWN`:** confirm the path is the Unity project root, the relevant SDK/settings files are present below it, and no scan limit skipped them. Use `--verbose` for bounded scope and diagnostic counts.
- **JSON is not safe to publish:** output redaction is heuristic. Review reports before sharing and never include the original project, raw source, SDK keys, credentials, or personal data in an issue.
- **A dependency or ad-serving issue remains after a pass:** run the Unity editor, platform build, mediation integration test, and dashboard checks separately. This CLI cannot perform those checks.

## Support And Security

Open reproducible product issues in the [GitHub issue tracker](https://github.com/techdigga/easy-ads-integration-validator/issues). Include the tool version, operating system, command shape, status/exit code, and a redacted report excerpt. Do not upload private Unity projects, credentials, SDK keys, or personal data.

For suspected vulnerabilities, leaked credentials, or private project data exposure, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.

## License

Apache-2.0. See [LICENSE](LICENSE).
