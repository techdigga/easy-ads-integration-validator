# Documentation

Easy Ads Integration Validator is a static Unity AppLovin MAX mediation audit tool. Use this page to choose the right public guide.

## Guides

| Guide | Use it for |
| --- | --- |
| [Quickstart](quickstart.md) | Install the CLI, run a first scan, read findings, gate CI, and configure optional MCP tools. |
| [Configuration](configuration.md) | Customize supported policy thresholds, required networks, aliases, and report detail. |
| [MAX Unity Profile](max-unity-profile.md) | Understand the validation areas, evidence, statuses, and static-analysis limits. |
| [Agent Contract](agent-contract.md) | Feed compact or full reports to Codex, Claude, or another coding agent safely. |
| [Artifact Verification](artifact-verification.md) | Verify release checksums and understand the public beta trust model. |

## Recommended Workflow

1. Install the `EasyAdsIntegrationValidator` .NET tool.
2. Run a compact JSON scan for quick triage.
3. Read the critical and high-severity findings first.
4. Request full evidence only for findings you need to investigate.
5. Review changes through your normal development process and rerun the scan.

The optional `EasyAdsIntegrationValidator.Mcp` package exposes the same read-only scan and report workflow to local Codex, Claude, and other MCP clients. The validator remains deterministic and does not silently modify Unity files.

## Public Scope

The public beta supports Unity projects using AppLovin MAX mediation and reads committed project files. It does not run Unity, builds, devices, runtime logs, dashboard APIs, or legal compliance checks. `WARN` and `UNKNOWN` findings are review prompts, not runtime certification.

## Support

Use the [GitHub issue tracker](https://github.com/techdigga/easy-ads-integration-validator/issues) for reproducible product issues. For security concerns, follow [SECURITY.md](../SECURITY.md).
