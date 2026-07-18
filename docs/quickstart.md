# Quickstart

## 1. Install

Install the .NET 8 SDK or runtime, then install the tool:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 0.1.0-beta.1
```

Check the installed command:

```bash
easy-ads-validator --version
```

## 2. Run Your First Scan

Point the scanner at a Unity project folder:

```bash
easy-ads-validator scan /path/to/unity-project
```

The beta defaults to Unity and AppLovin MAX:

```bash
easy-ads-validator scan /path/to/unity-project --platform unity --mediation max
```

## 3. Choose Report Output

Markdown is the default human-readable output:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown
```

JSON is better for CI and coding agents:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Write both formats to a folder:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report
```

## 4. Read Results

Each finding has:

- `PASS`: evidence was found for the expected integration behavior.
- `FAIL`: strong evidence of a release-blocking problem.
- `WARN`: likely issue or manual follow-up needed.
- `UNKNOWN`: static analysis could not prove the result from committed files.
- `INTERNAL_WARN`: a rule could not complete and emitted diagnostics.

Severity describes potential impact. Confidence describes how strong the static evidence is.

## 5. Tune Policy

Copy the sample policy and adjust it for the project:

```bash
easy-ads-validator scan /path/to/unity-project --policy examples/ads-audit.json
```

Use policy overrides for required networks, minimum SDK versions, severity changes, and project-specific MAX API aliases.

## 6. Beta Limits

The scanner reads committed files only. It does not run Unity, build the project, call ad network dashboards, inspect device logs, or certify legal compliance.

