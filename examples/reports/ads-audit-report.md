# Easy Ads Integration Validator Report

Profile: `max-unity`
Project: `valid-max-basic`

## Readiness

Failure threshold (--fail-on `fail`): `PASS`
Score: `93/100` Grade: `A`
Strong shape. Keep an eye on manual-review and unknown items before release.
Focus first on Critical and High Fail findings. Warn and Unknown items are static-analysis review prompts, not runtime proof.

- Pass: 31
- Fail: 0
- Warn: 7
- Unknown: 9
- Internal warnings: 0
- Partial: False
- 🔴 Critical issues: 0
- 🟠 High issues: 2
- Needs Review: 0
- ❔ Unknown / static limits: 9

Category progress:
- Sdk Import: 4/5 pass (80%)
- Platform Settings: 4/6 pass (66%)
- Initialization: 7/8 pass (87%)
- Consent: 5/6 pass (83%)
- Dependencies: 5/5 pass (100%)
- Callbacks: 3/5 pass (60%)
- Loading: 1/5 pass (20%)
- Revenue: 1/4 pass (25%)
- Production Safety: 1/3 pass (33%)

## Ad Network Matrix

Mediation: `max-unity`

| Network | Installed | Android | iOS | Resolution |
| --- | --- | --- | --- | --- |
| Google AdMob | yes | 23.6.0.0 | unknown | needs iOS evidence |
| Meta | no | unknown | unknown | missing |
| Mintegral | no | unknown | unknown | missing |
| Pangle | no | unknown | unknown | missing |
| Unity Ads | no | unknown | unknown | missing |
| ironSource | no | unknown | unknown | missing |

## Scan Metadata

- Tool: `easy-ads-validator`
- Version: `0.1.0-beta.10`
- Schema: `1.0`
- Started at: `2026-07-19T12:17:22.4336530+00:00`
- Project root: `sample-unity-project`
- Unity project: `True`
- Include passes: `False`
- Fail on: `fail`

## Displayed Counts By Severity

- Info: 0
- Low: 6
- Medium: 8
- High: 2
- Critical: 0

## Displayed Counts By Status

- Pass: 0
- Fail: 0
- Warn: 7
- Unknown: 9
- InternalWarn: 0

## Warnings 🟡 - Worth Re-checking

### MAX081: Revenue callback forwards to configured analytics sinks

Status: ⚠️ `Warn` Severity: 🟠 `High` Confidence: `Medium`

Category: `Revenue`

Revenue callback exists but no configured analytics sink evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

Remediation: Forward MAX impression revenue to a configured analytics sink or update analyticsRevenueSinks policy.

### MAX032: MAX initialization is guarded

Status: ⚠️ `Warn` Severity: 🟡 `Medium` Confidence: `Medium`

Category: `Initialization`

InitializeSdk does not appear guarded by local initialization state.

Evidence:
- `Assets/Scripts/AdsManager.cs:10`

Remediation: Guard MAX initialization so repeated lifecycle entry points cannot initialize the SDK more than once.

### MAX071: Retry system exists for failed loads

Status: ⚠️ `Warn` Severity: 🟡 `Medium` Confidence: `Medium`

Category: `Loading`

Load-failure callbacks exist but retry scheduling evidence was not found.

Evidence:
- `Assets/Scripts/AdsManager.cs:16`
- `Assets/Scripts/AdsManager.cs:22`

Remediation: Schedule bounded retries from failed load/display callbacks.

### MAX073: Video ads reload after close or fail callbacks

Status: ⚠️ `Warn` Severity: 🟡 `Medium` Confidence: `Medium`

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

Status: ⚠️ `Warn` Severity: 🔵 `Low` Confidence: `Medium`

Category: `Revenue`

MAX show or placement calls do not consistently include meaningful placement names.

Evidence:
- `Assets/Scripts/AdsManager.cs:36`

Remediation: Pass placement/source names to MAX show calls, banner/MREC placement setters, or a connected show abstraction.

### MAX074: Ad type loading is staged after initialization

Status: ⚠️ `Warn` Severity: 🔵 `Low` Confidence: `Medium`

Category: `Loading`

Ad load calls do not show obvious staging or delay.

Evidence:
- `Assets/Scripts/AdsManager.cs:28`
- `Assets/Scripts/AdsManager.cs:29`

Remediation: Consider staging initial ad loads to reduce startup contention.

### MAX082: Revenue payload includes important fields

Status: ⚠️ `Warn` Severity: 🔵 `Low` Confidence: `Medium`

Category: `Revenue`

Revenue payload evidence may be missing fields: network, precision.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`
- `Assets/Scripts/AdsManager.cs:20`
- 1 more evidence item(s) omitted. Use `--report-detail full` for complete evidence.

Remediation: Include revenue, network, ad unit, placement, and precision where available in analytics payloads.

## Unknown / Static Limits ❔ - Static-analysis Limit

### MAX006: MAX SDK assembly definitions exist

Status: ❔ `Unknown` Severity: 🟠 `High` Confidence: `Low`

Category: `Sdk Import`

UPM MAX package metadata exists, but package contents are not committed, so assembly definitions could not be inspected.

Evidence:
- `Packages/manifest.json`
  - `com.applovin.mediation.ads`

### MAX011: Android Gradle settings are compatible

Status: ❔ `Unknown` Severity: 🟡 `Medium` Confidence: `Low`

Category: `Platform Settings`

Unity custom Gradle template toggle evidence is missing.

### MAX012: iOS deployment target meets policy

Status: ❔ `Unknown` Severity: 🟡 `Medium` Confidence: `Low`

Category: `Platform Settings`

iOS deployment target setting was not found after checking Unity ProjectSettings, committed plist, and xcconfig evidence.

### MAX062: Banner and MREC callbacks are wired when used

Status: ❔ `Unknown` Severity: 🟡 `Medium` Confidence: `Low`

Category: `Callbacks`

Banner/MREC use evidence is absent.

### MAX063: App open callbacks are wired when used

Status: ❔ `Unknown` Severity: 🟡 `Medium` Confidence: `Low`

Category: `Callbacks`

App open ad use evidence is absent.

### MAX072: Retry delay appears exponential or bounded

Status: ❔ `Unknown` Severity: 🟡 `Medium` Confidence: `Low`

Category: `Loading`

Retry scheduling evidence is absent.

### MAX044: Child-directed and age-related flags are considered

Status: ❔ `Unknown` Severity: 🔵 `Low` Confidence: `Low`

Category: `Consent`

Age-related privacy flag evidence was not found and is not required by default policy.

### MAX090: MAX Ad Review or quality service is configured

Status: ❔ `Unknown` Severity: 🔵 `Low` Confidence: `Low`

Category: `Production Safety`

Ad quality service evidence was not found and is not required by default policy.

### MAX091: Creative Debugger is not disabled unexpectedly

Status: ❔ `Unknown` Severity: 🔵 `Low` Confidence: `Low`

Category: `Production Safety`

Creative Debugger configuration evidence is absent.

## Limitations

- Static analysis only. Runtime behavior, dashboard setup, and legal compliance are not verified.
