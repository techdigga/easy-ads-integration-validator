# Documentation

Easy Ads Integration Validator is a static Unity AppLovin MAX and LevelPlay mediation audit tool. Use this page to choose the right public guide.

## Guides

| Guide | Use it for |
| --- | --- |
| [Quickstart](quickstart.md) | Install the CLI, run a first scan, read findings, gate CI, and configure optional MCP tools. |
| [Configuration](configuration.md) | Customize supported policy thresholds, required networks, aliases, and report detail. |
| [MAX Unity Profile](max-unity-profile.md) | Understand the validation areas, evidence, statuses, and static-analysis limits. |
| [LevelPlay Unity Profile](levelplay-unity-profile.md) | Understand `levelplay-unity`, SDK `9.0.0+`, `LP001`-`LP040`, adapter matrix statuses, consent/privacy evidence, callback checks, production safety, and static-only limits. |
| [Agent Contract](agent-contract.md) | Feed compact or full reports to Codex, Claude, or another coding agent safely. |
| [Artifact Verification](artifact-verification.md) | Verify release checksums and understand the public beta trust model. |

## Recommended Workflow

1. Install the `EasyAdsIntegrationValidator` .NET tool.
2. Run a compact JSON scan for quick triage.
3. Read the critical and high-severity findings first.
4. Request full evidence only for findings you need to investigate.
5. Review changes through your normal development process and rerun the scan.

The optional `EasyAdsIntegrationValidator.Mcp` package exposes the same read-only scan and report workflow to local Codex, Claude, and other MCP clients. The validator remains deterministic and does not silently modify Unity files.

For a LevelPlay MCP scan, send the mediation/profile selection as a matching pair:

```json
{
  "projectPath": "/absolute/path/to/unity-project",
  "mediation": "levelplay",
  "profile": "levelplay-unity",
  "reportDetail": "compact",
  "includePasses": false
}
```

`mediation` and `profile` are optional and default to `max` and `max-unity`. LevelPlay requests must supply `levelplay` and `levelplay-unity` together.

For LevelPlay, use the compact-first MCP workflow: call `read_report_summary` first, then request individual findings such as `LP001`-`LP040` with `get_finding`. Use `get_rule_docs` with `profile: "levelplay-unity"` when an explanation is needed. `FAIL` is an actionable static issue, `WARN` is a review signal, and `UNKNOWN` means the committed project files cannot prove the result.

## Public Scope

The historical MVP was MAX-only, but the current Beta 10 supports Unity projects using AppLovin MAX by default or the explicit Unity LevelPlay `levelplay-unity` profile. LevelPlay Beta 10 supports SDK `9.0.0+`; known pre-v9 versions fail validation. It does not run Unity, builds, devices, runtime logs, dashboard APIs, callback delivery, or legal compliance checks. It does inspect source-level callback wiring, load ordering, and obvious thread/UI risks. `WARN` and `UNKNOWN` findings are review prompts, not runtime certification.

## Support

Use the [GitHub issue tracker](https://github.com/techdigga/easy-ads-integration-validator/issues) for reproducible product issues. For security concerns, follow [SECURITY.md](../SECURITY.md).
