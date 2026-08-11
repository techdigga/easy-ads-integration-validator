# Quickstart

This walkthrough takes a Unity developer from installation to a first AppLovin MAX audit. For the complete public guide map and support links, see the [Documentation index](index.md) and [README](../README.md).

## 1. Install The CLI

Install .NET 8, .NET 9, or .NET 10, then install the public beta:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 0.1.0-beta.8
```

Verify the command:

```bash
dotnet --info
easy-ads-validator --version
```

On Windows PowerShell, the install command is the same. If the command is not found after installation, add the global .NET tools directory to the current session:

```powershell
$env:PATH = "$env:PATH;$env:USERPROFILE\.dotnet\tools"
```

On macOS or Linux:

```bash
export PATH="$PATH:$HOME/.dotnet/tools"
```

Open a new shell after adding the directory permanently to the user PATH.

## 2. Run A First Scan

Pass the root folder of the Unity project:

```bash
easy-ads-validator scan /path/to/unity-project
```

PowerShell example:

```powershell
easy-ads-validator scan "C:\Projects\My Unity Game"
```

The default profile is Unity AppLovin MAX. You can state it explicitly:

```bash
easy-ads-validator scan /path/to/unity-project --platform unity --mediation max --profile max-unity
```

The command reads committed project files only. It does not open Unity or change the project.

## 3. Choose Report Output

Use Markdown for a human-readable terminal report:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown
```

Use compact JSON for CI or a coding agent:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Write both formats to a directory:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report
```

Use full evidence while investigating a specific rule:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report --include-passes --report-detail full
```

Interactive Markdown output can show progress and ask whether to display the report or write it to a file. Explicit output paths and JSON output remain non-interactive.

## 4. Read Results And Gate CI

Each finding has a status, severity, confidence, evidence, remediation, and limitations:

| Status | Meaning |
| --- | --- |
| `PASS` | Expected static evidence was found. |
| `FAIL` | Strong static evidence indicates a release-blocking problem. |
| `WARN` | A likely issue or manual follow-up was detected. |
| `UNKNOWN` | Available committed files cannot prove the result. |
| `INTERNAL_WARN` | The scanner reported an internal rule or infrastructure problem. |

Use `--fail-on` to gate CI on failed findings at a severity threshold:

```bash
easy-ads-validator scan "$UNITY_PROJECT" --format json --report-detail compact --fail-on high
```

Exit code `1` means the scan completed and findings met the configured threshold. Exit code `2` means the command or input was invalid. Exit code `3` means an unexpected internal error occurred.

## 5. Tune Local Policy

Start from the sample policy:

```bash
easy-ads-validator scan /path/to/unity-project --policy examples/ads-audit.json
```

Policy can adjust supported thresholds, required networks, severity overrides, and MAX API aliases. It does not enable dashboard checks or runtime validation. See [Configuration](configuration.md).

## 6. Optional MCP Tools

Install the optional MCP package at the same version as the CLI:

```bash
dotnet tool install --global EasyAdsIntegrationValidator.Mcp --version 0.1.0-beta.8
easy-ads-validator-mcp
```

Register `easy-ads-validator-mcp` as a local stdio server in Codex, Claude, or another MCP client. It provides read-only scan and report tools around the CLI and does not apply fixes. See the [agent contract](agent-contract.md).

## 7. Beta Limits

The validator is a public beta. It does not run builds or runtime checks, query dashboards, prove ad serving or revenue behavior, or certify legal/privacy compliance. Review `WARN` and `UNKNOWN` findings manually and validate release behavior in the normal Unity, platform, and mediation workflows.
