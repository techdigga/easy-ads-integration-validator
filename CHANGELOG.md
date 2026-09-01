# Changelog

## 0.1.0-beta.10

Tenth public beta for Easy Ads Integration Validator.

- Improved LevelPlay SDK 9+ direct-import recognition for current Unity package layouts, including `Unity.LevelPlay` assembly definitions and mediated-network settings assets.
- Added LevelPlay SDK version evidence from source constants such as `Constants.cs` when package metadata is unavailable.
- Reduced false installation failures for current LevelPlay projects while preserving static-only validation and the existing MAX profile.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.9

Ninth public beta for Easy Ads Integration Validator.

- Added the Unity LevelPlay mediation profile for SDK `9.0.0+`, with LP001-LP040 coverage across installation, settings, platform configuration, credentials, ad units, initialization, consent, callbacks, loading, revenue, analytics, adapter dependencies, and production safety.
- Added LevelPlay API-aware static evidence and an adapter/network matrix that reports committed dependency and platform evidence without claiming Unity or native build resolution.
- Added profile-aware MCP rule documentation so Codex, Claude, and compatible clients can request the matching MAX or LevelPlay remediation guidance.
- Preserved MAX as the default profile and retained the deterministic, read-only, static-analysis boundary.

This beta does not verify dashboard setup, runtime ad serving, device logs, generated build output, or legal compliance.

## 0.1.0-beta.8

Eighth public beta for Easy Ads Integration Validator.

- Added minified compact JSON output for lower agent and CI context usage without changing the report schema.
- Added bounded MCP finding retrieval with entry/terminal evidence preservation and explicit full-evidence expansion.
- Documented the stable compact report and MCP retrieval contract for Codex, Claude, and compatible clients.
- Hardened release validation for compact JSON and retained reproducible multi-target package and artifact checks.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.7

Seventh public beta for Easy Ads Integration Validator.

- Applied the finalized MAX rule hardening across SDK setup, platform settings, keys, ad units, initialization, consent, network dependencies, callbacks, loading, revenue, and production safety.
- Improved API-aware static evidence for Unity MAX integrations, including package references, mediated dependency resolution, and callback behavior.
- Kept critical findings prioritized and retained compact, agent-friendly reports for CLI and coding-agent workflows.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.6

Sixth public beta for Easy Ads Integration Validator.

- Hardened public release artifact validation and release tag guidance.
- Improved MAX SDK package reference coverage.
- Added adapter version visibility to the ad network matrix.
- Polished Markdown reports for faster human triage.
- Updated Codex and Claude agent workflow guidance.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.5

Fifth public beta for Easy Ads Integration Validator.

- Added interactive Markdown terminal progress for scan steps.
- Added an interactive destination prompt for terminal or file report output when running in a real terminal.
- Added generated report path output and OS-specific open hints for file reports.
- Kept JSON output, redirected output, and CI environments prompt-free for automation and agent usage.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.4

Fourth public beta for Easy Ads Integration Validator.

- Added multi-target NuGet global tool support for `net8.0` and `net9.0`.
- Updated package validation to inspect both target-framework assets.
- Updated public install docs and examples for `0.1.0-beta.4`.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.3

Third public beta for Easy Ads Integration Validator.

- Retargeted the CLI package to .NET 9 so it runs with the current installed maintainer SDK/runtime.
- Updated release workflow defaults, local release scripts, public install docs, and examples for `0.1.0-beta.3`.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.2

Second public beta for Easy Ads Integration Validator.

- Added the MAX ad network matrix report for policy-required and detected mediated networks.
- Added AppLovin adapter-aware dependency evidence matching to reduce third-party SDK false positives.
- Added compact report and JSON evidence summaries for matrix rows.
- Added diagnostics for skipped matrix evidence inputs.
- Improved MAX SDK version, consent, iOS, Android, revenue, callback, and report-priority behavior during dogfood hardening.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.

## 0.1.0-beta.1

Initial public beta release candidate for Easy Ads Integration Validator.

- Added Unity AppLovin MAX static audit profile.
- Added Markdown and JSON report output.
- Added compact, standard, full, and summary-only report modes.
- Added local JSON policy overrides.
- Added MAX SDK API-reference based checks for callbacks, loading, revenue, and production safety.
- Added NuGet tool packaging for `EasyAdsIntegrationValidator`.
- Added public command name `easy-ads-validator`.

This beta is static analysis only. It does not verify dashboard setup, runtime ad serving, device logs, build output, or legal compliance.
