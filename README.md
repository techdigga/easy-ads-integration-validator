# Easy Ads Integration Validator

### Find Unity mediation gaps before they become build failures, lost revenue, or release surprises.

Easy Ads Integration Validator is a local, read-only static analysis tool for Unity game development teams using AppLovin MAX or Unity LevelPlay ad mediation. It checks the project files under a supplied Unity project root and produces evidence-based reports for developers, CI, Codex, Claude, and compatible coding agents.

**One scan. Clear evidence. Faster fixes.**

## Why Teams Use It

- Check mediation setup without opening Unity or building the game.
- Find missing SDK files, adapters, dependencies, privacy settings, callbacks, and ad lifecycle wiring.
- See each result's status, severity, confidence, evidence, remediation, and limitations.
- Use compact JSON in CI or provide focused report evidence to an AI coding tool.
- Keep scans deterministic, local, read-only, and independent of dashboard APIs.

## What It Validates

The stable package is `1.0.0`. Both supported mediation profiles cover their platform-specific setup while sharing the same report format and static-analysis boundaries.

| Area | Coverage examples |
| --- | --- |
| SDK setup | Assets or UPM installation, duplicate installs, supported versions, assembly definitions, settings, and app keys. |
| Dependencies | External Dependency Manager, resolver settings, adapters, network libraries, Android dependency evidence, and network matrix. |
| Privacy | Consent flow, privacy URL, ATT-related ownership, age signals, and conflicting direct consent configuration. |
| Platform readiness | Unity version, Android API levels, Gradle/resolver settings, manifest signals, and iOS deployment evidence. |
| Ad configuration | App IDs, SDK keys, ad units, duplicate IDs, placement tracking, and local fallback values for remote configuration. |
| Initialization | Initialization call sites, singular guards, callback subscriptions, state handling, and load ordering. |
| Ad lifecycle | Readiness checks, show/load callbacks, terminal-path recovery, banner refresh, retries, and backoff delays. |
| Events and revenue | Impression, click, display, paid-revenue, analytics forwarding, expected payload fields, and background-callback UI risks. |
| Production safety | Debug settings, diagnostic tools, production flags, and unsafe UI work in background callbacks. |

## Supported Profiles

### AppLovin MAX

The default profile for Unity MAX projects.

```bash
easy-ads-validator scan /path/to/unity-project
```

MAX validation covers direct or UPM installation, plugin version, settings, platform configuration, keys, ad units, initialization, consent, callbacks, loading, retry behavior, revenue, analytics, adapters, dependencies, and production safety.

Details: [MAX Unity profile](docs/max-unity-profile.md).

### Unity LevelPlay

The LevelPlay profile supports Unity LevelPlay SDK `9.0.0+`.

```bash
easy-ads-validator scan /path/to/unity-project --mediation levelplay --profile levelplay-unity
```

LevelPlay validation covers direct or UPM installation, SDK version, settings, platform configuration, app keys, ad units, initialization, consent, callbacks, loading, retry behavior, revenue, analytics, adapters, dependencies, and production safety.

Details: [LevelPlay Unity profile](docs/levelplay-unity-profile.md).

## Supported Versions

| Component | Default policy floor |
| --- | --- |
| .NET runtime | `8`, `9`, or `10` |
| Unity editor evidence | `2022.3.62f2` or newer |
| AppLovin MAX Unity plugin | `8.6.0` or newer |
| External Dependency Manager for Unity | `1.2.185` or newer when MAX evidence is present |
| Unity LevelPlay SDK | `9.0.0` or newer |

These are validator policy floors, not guarantees about every SDK/platform combination. Confirm compatibility in the official [MAX Unity documentation](https://developers.applovin.com/en/max/unity/overview/integration/) and [LevelPlay Unity documentation](https://docs.unity.com/grow/levelplay/sdk/unity).

## AI-Friendly By Design

- Compact JSON keeps the first agent response small.
- Full reports provide targeted files and lines for investigation.
- The optional MCP server exposes local scan, summary, finding, and rule-documentation tools.
- Stable rule IDs let agents retrieve only the evidence needed for the next decision.
- Deterministic reports let Codex, Claude, and compatible agents explain findings and propose fixes without replacing the validator.

Typical workflow:

1. Run a compact JSON scan.
2. Give the report to Codex or Claude, or use the MCP server.
3. Ask for an explanation and a minimal fix plan.
4. Apply changes through the normal development workflow.
5. Scan again to confirm the static evidence changed.

The validator does not modify Unity projects automatically.

## Install

Install .NET 8, .NET 9, or .NET 10, then install the CLI:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 1.0.0
```

Verify, update, or remove it:

```bash
easy-ads-validator --version
dotnet tool update --global EasyAdsIntegrationValidator --version 1.0.0
dotnet tool uninstall --global EasyAdsIntegrationValidator
```

Self-contained macOS and Windows archives are available from [GitHub Releases](https://github.com/techdigga/easy-ads-integration-validator/releases). Verify `checksums.txt` before running an archive with [Artifact Verification](docs/artifact-verification.md).

## First Scan

The default command scans a Unity project with the MAX profile:

```bash
easy-ads-validator scan /path/to/unity-project
```

Windows PowerShell:

```powershell
easy-ads-validator scan "C:\Projects\My Unity Game"
```

The scan is read-only and does not require Unity to be installed or open.

## Reports

Markdown is the default human-readable output:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown
```

Use compact JSON for CI and agents:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Write both formats to a directory:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report
```

Use SARIF for GitHub Code Scanning or compatible CI systems:

```bash
easy-ads-validator scan /path/to/unity-project --format sarif --out audit-report
```

Use full evidence when investigating a finding:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report --include-passes --report-detail full
```

Reports redact common credential-shaped values, but redaction is heuristic rather than a complete secret scanner. Review reports before sharing them.

## CI And MCP

Gate CI on high-severity failed findings:

```bash
easy-ads-validator scan "$UNITY_PROJECT" --format json --report-detail compact --fail-on high
```

| Exit code | Meaning |
| --- | --- |
| `0` | Scan completed without a failed finding at the configured threshold. |
| `1` | Scan completed with a failed finding at the configured threshold. |
| `2` | Input/configuration was invalid or the scan was partial. |
| `3` | Unexpected internal tool error. |

Install the optional MCP server at the same version:

```bash
dotnet tool install --global EasyAdsIntegrationValidator.Mcp --version 1.0.0
easy-ads-validator-mcp
```

Register `easy-ads-validator-mcp` as a local stdio server in Codex, Claude, or another MCP client. It uses the same deterministic validator, does not call an LLM, and does not modify Unity files. See the [MCP agent contract](docs/agent-contract.md).

## Configuration

Use a local JSON policy for supported project-specific thresholds and requirements:

```bash
easy-ads-validator scan /path/to/unity-project --policy examples/ads-audit.json
```

See [Configuration](docs/configuration.md) and the [sample policy](examples/ads-audit.json).

## Boundaries

This is static repository inspection, not build or runtime certification. It does not:

- Query mediation, analytics, Apple, Google, or other dashboards/APIs.
- Run Unity, Gradle, Xcode, emulators, simulators, devices, or game builds.
- Prove that an ad loads, displays, earns revenue, or invokes a callback at runtime.
- Verify waterfalls, bidding, server-side reporting, or account configuration.
- Certify GDPR, ATT, TCF, COPPA, or other legal/privacy compliance.
- Modify the Unity project automatically.

`WARN` and `UNKNOWN` findings are review signals. Read the confidence, evidence, remediation, and limitations in the report.

## Documentation

- [Documentation index](docs/index.md)
- [Quickstart](docs/quickstart.md)
- [Configuration](docs/configuration.md)
- [MAX Unity profile](docs/max-unity-profile.md)
- [LevelPlay Unity profile](docs/levelplay-unity-profile.md)
- [MCP agent contract](docs/agent-contract.md)
- [Artifact verification](docs/artifact-verification.md)

## Troubleshooting

- **Command not found:** add `$HOME/.dotnet/tools` on macOS/Linux or `%USERPROFILE%\\.dotnet\\tools` on Windows to `PATH`, then open a new shell.
- **Unsupported profile:** use `max/max-unity` or `levelplay/levelplay-unity`; LevelPlay requires both values together.
- **Unexpected `UNKNOWN` results:** confirm the supplied path is the Unity project root and that SDK, settings, resolver, and integration files are present below it.
- **Partial scan:** review `summary.isPartial`, then rerun with appropriate scan limits before using the result as a gate.
- **Remaining runtime or build issue:** use the normal Unity, platform, mediation, and dashboard workflows; this CLI cannot perform those checks.

## Support And Security

Open reproducible product issues in the [GitHub issue tracker](https://github.com/techdigga/easy-ads-integration-validator/issues). Include the tool version, operating system, command shape, exit code, and a redacted report excerpt. Do not upload private Unity projects, credentials, SDK keys, or personal data.

For suspected vulnerabilities or private-data exposure, follow [SECURITY.md](SECURITY.md).

## License

Apache-2.0. See [LICENSE](LICENSE).
