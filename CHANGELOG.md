# Changelog

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
