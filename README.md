# Easy Ads Integration Validator

Easy Ads Integration Validator is a command-line Unity MAX integration checker for static AppLovin MAX mediation audits.

The public beta scans files already committed in a Unity project and reports integration risks in Markdown or JSON. It is designed for Unity game developers, QA teams, CI jobs, and coding agents that need fast feedback before a release review.

Use it when you need a local AppLovin MAX mediation audit, Unity mobile ads static analyzer, or pre-release ad integration validator without dashboard API access.

## Install

Prerequisite: .NET 8 or .NET 9 SDK/runtime.

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 0.1.0-beta.4
```

Update:

```bash
dotnet tool update --global EasyAdsIntegrationValidator
```

Uninstall:

```bash
dotnet tool uninstall --global EasyAdsIntegrationValidator
```

## Run

From any terminal:

```bash
easy-ads-validator scan /path/to/unity-project
```

Agent-friendly JSON:

```bash
easy-ads-validator scan /path/to/unity-project --format json --report-detail compact
```

Archive both report formats:

```bash
easy-ads-validator scan /path/to/unity-project --format markdown,json --out audit-report --include-passes --report-detail full
```

Public beta defaults:

- `--platform unity`
- `--mediation max`
- `--profile max-unity`

## What It Checks

- MAX SDK import and version evidence.
- Unity, Android, and iOS project settings.
- SDK keys, AdMob app IDs when required, ad unit IDs, and placement tracking.
- MAX initialization ordering and guarding.
- Consent and privacy evidence before MAX initialization.
- Required mediated networks and native dependency evidence.
- Callback wiring, reload/retry behavior, revenue callbacks, and production debug safety.

## What It Does Not Verify

- Dashboard or API configuration.
- Unity Editor, Gradle, Xcode, simulator, emulator, or device behavior.
- Runtime proof that ads load, show, or generate revenue.
- Waterfall, bidding, or server-side reporting correctness.
- Legal compliance certification.
- Automatic project modification.

## Exit Codes

| Code | Meaning |
| --- | --- |
| `0` | Scan completed and no failed finding met the configured `--fail-on` threshold. |
| `1` | Scan completed and at least one failed finding met the configured `--fail-on` threshold. |
| `2` | Invalid arguments, missing project path, invalid policy, unsupported format/profile, or output path problem. |
| `3` | Unexpected internal error. |

## Documentation

- [Quickstart](docs/quickstart.md)
- [Configuration](docs/configuration.md)
- [Agent contract](docs/agent-contract.md)
- [MAX Unity profile](docs/max-unity-profile.md)
- [Artifact verification](docs/artifact-verification.md)

## Support And Security

Open non-security issues in the GitHub issue tracker.

For suspected vulnerabilities, leaked credentials, or private project data exposure, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.

## License

Apache-2.0. See [LICENSE](LICENSE).
