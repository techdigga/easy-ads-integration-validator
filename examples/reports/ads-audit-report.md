# Easy Ads Integration Validator Report

Profile: `max-unity`
Project: `valid-max-basic`

## Summary

Failure threshold (--fail-on `fail`): `PASS`
Readiness score: `80/100` Grade: `B`
Close to release-ready. Work through the highest-severity findings next.

- Pass: 23
- Fail: 0
- Warn: 11
- Unknown: 10
- Internal warnings: 0
- Partial: False
- Critical issues: 0
- High issues: 4
- Manual review: 0
- Unknown / static limits: 10

Category progress:
- Sdk Import: 3/3 pass (100%)
- Platform Settings: 2/5 pass (40%)
- Initialization: 6/8 pass (75%)
- Consent: 3/6 pass (50%)
- Dependencies: 3/5 pass (60%)
- Callbacks: 3/5 pass (60%)
- Loading: 1/5 pass (20%)
- Revenue: 1/4 pass (25%)
- Production Safety: 1/3 pass (33%)

## Ad Network Matrix

Mediation: `max-unity`

| Network | Installed | Adapter | Dependency XML | Android resolved | Status |
| --- | --- | --- | --- | --- | --- |
| Google AdMob | no | no | no | no | `missing` |
| Meta | no | no | no | no | `missing` |
| Mintegral | no | no | no | no | `missing` |
| Pangle | no | no | no | no | `missing` |
| Unity Ads | no | no | no | no | `missing` |
| ironSource | no | no | no | no | `missing` |

## Scan Metadata

- Tool: `easy-ads-validator`
- Version: `0.1.0-beta.5`
- Schema: `1.0`
- Started at: `2026-07-19T12:17:22.4336530+00:00`
- Project root: `sample-unity-project`
- Unity project: `True`
- Include passes: `False`
- Fail on: `fail`

## Displayed Counts By Severity

- Info: 0
- Low: 7
- Medium: 10
- High: 4
- Critical: 0

## Displayed Counts By Status

- Pass: 0
- Fail: 0
- Warn: 11
- Unknown: 10
- InternalWarn: 0

## Warnings ⚠️ - Worth re-checking

### MAX014: SKAdNetwork IDs are configured

Status: ⚠️ `Warn` Severity: `High` Confidence: `Medium`

Category: `Platform Settings`

SKAdNetwork ID evidence was not found after checking MAX settings, committed Info.plist files, and iOS post-process scripts.

Remediation: Commit SKAdNetwork identifiers or deterministic iOS postprocess evidence for required mediated networks.

### MAX021: Google AdMob app IDs are configured when required

Status: ⚠️ `Warn` Severity: `High` Confidence: `Medium`

Category: `Platform Settings`

Policy requires Google/AdMob mediation but app ID evidence was not found.

Remediation: Add Android and iOS Google Mobile Ads app IDs to committed platform configuration when AdMob is enabled.

### MAX022: Ad unit IDs are present and organized

Status: ⚠️ `Warn` Severity: `High` Confidence: `Medium`

Category: `Initialization`

Ad unit ID evidence was not found.

Remediation: Declare MAX ad unit IDs in committed configuration or application code, organized by ad format.

### MAX081: Revenue callback forwards to configured analytics sinks

Status: ⚠️ `Warn` Severity: `High` Confidence: `Medium`

Category: `Revenue`

Revenue callback exists but no configured analytics sink evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

Remediation: Forward MAX impression revenue to a configured analytics sink or update analyticsRevenueSinks policy.

### MAX032: MAX initialization is guarded

Status: ⚠️ `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Initialization`

InitializeSdk does not appear guarded by local initialization state.

Evidence:
- `Assets/Scripts/AdsManager.cs:10`

Remediation: Guard MAX initialization so repeated lifecycle entry points cannot initialize the SDK more than once.

### MAX042: Privacy policy URL is configured

Status: ⚠️ `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Consent`

Privacy policy URL/path evidence was not found.

Remediation: Configure a privacy policy URL in consent/privacy UI or committed app configuration.

### MAX071: Retry system exists for failed loads

Status: ⚠️ `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Loading`

Load-failure callbacks exist but retry scheduling evidence was not found.

Evidence:
- `Assets/Scripts/AdsManager.cs:16`
- `Assets/Scripts/AdsManager.cs:22`

Remediation: Schedule bounded retries from failed load/display callbacks.

### MAX073: Video ads reload after close or fail callbacks

Status: ⚠️ `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Loading`

Video ad use exists but reload-after-close/fail evidence was not found.

Evidence:
- `context` `Assets/Scripts/AdsManager.cs:28`
- `context` `Assets/Scripts/AdsManager.cs:29`
- `context` `Assets/Scripts/AdsManager.cs:36`

Unresolved:
- No direct, scheduled, or terminal-named reload chain was proven from static C# evidence.

Remediation: Reload interstitial and rewarded ads from hidden/closed and load/display failure callbacks.

### MAX023: Placement names are tracked

Status: ⚠️ `Warn` Severity: `Low` Confidence: `Medium`

Category: `Revenue`

MAX show calls do not include obvious placement names.

Evidence:
- `Assets/Scripts/AdsManager.cs:36`

Remediation: Pass placement names to MAX show calls or record equivalent placement metadata.

### MAX074: Ad type loading is staged after initialization

Status: ⚠️ `Warn` Severity: `Low` Confidence: `Medium`

Category: `Loading`

Ad load calls do not show obvious staging or delay.

Evidence:
- `Assets/Scripts/AdsManager.cs:28`
- `Assets/Scripts/AdsManager.cs:29`

Remediation: Consider staging initial ad loads to reduce startup contention.

### MAX082: Revenue payload includes important fields

Status: ⚠️ `Warn` Severity: `Low` Confidence: `Medium`

Category: `Revenue`

Revenue payload evidence may be missing fields: network, precision.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

Remediation: Include revenue, network, ad unit, placement, and precision where available in analytics payloads.

## Unknown / Static Limits ❓ - Static-analysis limit

### MAX003: External Dependency Manager evidence exists

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Dependencies`

External Dependency Manager evidence is absent or not committed.

### MAX004: Android resolver settings are acceptable

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Dependencies`

Committed Android resolver settings were not found.

### MAX012: iOS deployment target meets policy

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Platform Settings`

iOS deployment target setting was not found after checking Unity ProjectSettings, committed plist, and xcconfig evidence.

### MAX062: Banner and MREC callbacks are wired when used

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Callbacks`

Banner/MREC use evidence is absent.

### MAX063: App open callbacks are wired when used

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Callbacks`

App open ad use evidence is absent.

### MAX072: Retry delay appears exponential or bounded

Status: ❓ `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Loading`

Retry scheduling evidence is absent.

### MAX013: ATT usage description is configured

Status: ❓ `Unknown` Severity: `Low` Confidence: `Low`

Category: `Consent`

ATT usage description was not found in committed static inputs; generated iOS plist output cannot be evaluated without building.

### MAX044: Child-directed and age-related flags are considered

Status: ❓ `Unknown` Severity: `Low` Confidence: `Low`

Category: `Consent`

Age-related privacy flag evidence was not found and is not required by default policy.

### MAX090: MAX Ad Review or quality service is configured

Status: ❓ `Unknown` Severity: `Low` Confidence: `Low`

Category: `Production Safety`

Ad quality service evidence was not found and is not required by default policy.

### MAX091: Creative Debugger is not disabled unexpectedly

Status: ❓ `Unknown` Severity: `Low` Confidence: `Low`

Category: `Production Safety`

Creative Debugger configuration evidence is absent.

## Limitations

- Static analysis only. Runtime behavior, dashboard setup, and legal compliance are not verified.
