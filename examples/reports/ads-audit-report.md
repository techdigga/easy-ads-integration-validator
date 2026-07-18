# Easy Ads Integration Validator Report

Profile: `max-unity`
Project: `valid-max-basic`

## Summary

Failure threshold (--fail-on `fail`): `PASS`
Readiness score: `75/100` Grade: `C`

- Pass: 23
- Fail: 0
- Warn: 12
- Unknown: 9
- Internal warnings: 0
- Partial: False
- Critical issues: 0
- High issues: 5
- Manual review: 0
- Unknown / static limits: 9

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

## Scan Metadata

- Tool: `easy-ads-validator`
- Version: `0.1.0-beta.1`
- Schema: `1.0`
- Started at: `2026-07-18T17:03:29.4999530+00:00`
- Project root: `.`
- Unity project: `True`
- Include passes: `True`
- Fail on: `fail`

## Displayed Counts By Severity

- Info: 0
- Low: 8
- Medium: 15
- High: 15
- Critical: 6

## Displayed Counts By Status

- Pass: 23
- Fail: 0
- Warn: 12
- Unknown: 9
- InternalWarn: 0

## Warnings

### MAX013: ATT usage description is configured

Status: `Warn` Severity: `High` Confidence: `Medium`

Category: `Consent`

ATT usage description evidence was not found.

Remediation: Add NSUserTrackingUsageDescription to committed iOS plist/postprocess configuration when ATT is required.

### MAX014: SKAdNetwork IDs are configured

Status: `Warn` Severity: `High` Confidence: `Medium`

Category: `Platform Settings`

Committed SKAdNetwork ID evidence was not found.

Remediation: Commit SKAdNetwork identifiers or deterministic iOS postprocess evidence for required mediated networks.

### MAX021: Google AdMob app IDs are configured when required

Status: `Warn` Severity: `High` Confidence: `Medium`

Category: `Platform Settings`

Policy requires Google/AdMob mediation but app ID evidence was not found.

Remediation: Add Android and iOS Google Mobile Ads app IDs to committed platform configuration when AdMob is enabled.

### MAX022: Ad unit IDs are present and organized

Status: `Warn` Severity: `High` Confidence: `Medium`

Category: `Initialization`

Ad unit ID evidence was not found.

Remediation: Declare MAX ad unit IDs in committed configuration or application code, organized by ad format.

### MAX081: Revenue callback forwards to configured analytics sinks

Status: `Warn` Severity: `High` Confidence: `Medium`

Category: `Revenue`

Revenue callback exists but no configured analytics sink evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

Remediation: Forward MAX impression revenue to a configured analytics sink or update analyticsRevenueSinks policy.

### MAX032: MAX initialization is guarded

Status: `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Initialization`

InitializeSdk does not appear guarded by local initialization state.

Evidence:
- `Assets/Scripts/AdsManager.cs:10`

Remediation: Guard MAX initialization so repeated lifecycle entry points cannot initialize the SDK more than once.

### MAX042: Privacy policy URL is configured

Status: `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Consent`

Privacy policy URL/path evidence was not found.

Remediation: Configure a privacy policy URL in consent/privacy UI or committed app configuration.

### MAX071: Retry system exists for failed loads

Status: `Warn` Severity: `Medium` Confidence: `Medium`

Category: `Loading`

Load-failure callbacks exist but retry scheduling evidence was not found.

Evidence:
- `Assets/Scripts/AdsManager.cs:16`
- `Assets/Scripts/AdsManager.cs:22`

Remediation: Schedule bounded retries from failed load/display callbacks.

### MAX073: Video ads reload after close or fail callbacks

Status: `Warn` Severity: `Medium` Confidence: `Medium`

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

Status: `Warn` Severity: `Low` Confidence: `Medium`

Category: `Revenue`

MAX show calls do not include obvious placement names.

Evidence:
- `Assets/Scripts/AdsManager.cs:36`

Remediation: Pass placement names to MAX show calls or record equivalent placement metadata.

### MAX074: Ad type loading is staged after initialization

Status: `Warn` Severity: `Low` Confidence: `Medium`

Category: `Loading`

Ad load calls do not show obvious staging or delay.

Evidence:
- `Assets/Scripts/AdsManager.cs:28`
- `Assets/Scripts/AdsManager.cs:29`

Remediation: Consider staging initial ad loads to reduce startup contention.

### MAX082: Revenue payload includes important fields

Status: `Warn` Severity: `Low` Confidence: `Medium`

Category: `Revenue`

Revenue payload evidence may be missing fields: network, precision.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

Remediation: Include revenue, network, ad unit, placement, and precision where available in analytics payloads.

## Unknown / Static Limits

### MAX003: External Dependency Manager evidence exists

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Dependencies`

External Dependency Manager evidence is absent or not committed.

### MAX004: Android resolver settings are acceptable

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Dependencies`

Committed Android resolver settings were not found.

### MAX012: iOS deployment target meets policy

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Platform Settings`

iOS deployment target setting was not found.

### MAX062: Banner and MREC callbacks are wired when used

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Callbacks`

Banner/MREC use evidence is absent.

### MAX063: App open callbacks are wired when used

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Callbacks`

App open ad use evidence is absent.

### MAX072: Retry delay appears exponential or bounded

Status: `Unknown` Severity: `Medium` Confidence: `Low`

Category: `Loading`

Retry scheduling evidence is absent.

### MAX044: Child-directed and age-related flags are considered

Status: `Unknown` Severity: `Low` Confidence: `Low`

Category: `Consent`

Age-related privacy flag evidence was not found and is not required by default policy.

### MAX090: MAX Ad Review or quality service is configured

Status: `Unknown` Severity: `Low` Confidence: `Low`

Category: `Production Safety`

Ad quality service evidence was not found and is not required by default policy.

### MAX091: Creative Debugger is not disabled unexpectedly

Status: `Unknown` Severity: `Low` Confidence: `Low`

Category: `Production Safety`

Creative Debugger configuration evidence is absent.

## Passes

### MAX001: MAX SDK is imported

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Sdk Import`

MAX import evidence was found.

Evidence:
- `Packages/manifest.json`
  - `com.applovin.mediation.ads`

### MAX020: MAX SDK key is configured

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Initialization`

MAX SDK key evidence was found.

Evidence:
- `Assets/Plugins/Android/AndroidManifest.xml:3`

### MAX030: MAX SDK is initialized

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Initialization`

MaxSdk.InitializeSdk invocation was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:10`

### MAX040: Consent flow evidence exists

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Consent`

CMP/UMP/MAX consent evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:8`

### MAX080: Ad revenue callback is wired

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Revenue`

MAX ad revenue callback evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:27`

### MAX092: Production debug and test hooks are not unconditional

Status: `Pass` Severity: `Critical` Confidence: `High`

Category: `Production Safety`

No unconditional MAX debug/test evidence was found.

### MAX002: MAX SDK version meets policy

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Sdk Import`

MAX plugin version meets the configured minimum.

Evidence:
- `Packages/manifest.json`
  - `com.applovin.mediation.ads`

### MAX010: Android API levels meet policy

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Platform Settings`

Android API levels and Jetifier setting meet policy.

Evidence:
- `Assets/Plugins/Android/mainTemplate.gradle:2`
  - `    minSdkVersion 24`
- `Assets/Plugins/Android/mainTemplate.gradle:3`
  - `    targetSdkVersion 35`
- `Assets/Plugins/Android/mainTemplate.gradle:5`
  - `android.enableJetifier=true`

### MAX031: MAX initialization is singular

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Initialization`

Exactly one InitializeSdk call was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:10`

### MAX033: MAX initialized callback is subscribed before initialization

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Initialization`

MAX initialized callback subscription appears before InitializeSdk in the same method.

Evidence:
- `Assets/Scripts/AdsManager.cs:9`

### MAX034: Ad loading starts after initialization

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Initialization`

Ad loading appears on an initialized callback path.

Evidence:
- `Assets/Scripts/AdsManager.cs:28`
- `Assets/Scripts/AdsManager.cs:29`

### MAX041: Consent is configured before MAX initialization

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Consent`

Consent evidence appears before InitializeSdk in the same method.

Evidence:
- `Assets/Scripts/AdsManager.cs:8`

### MAX050: Required mediated networks are present

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Dependencies`

All policy-required mediated network evidence was found.

Evidence:
- `Packages/manifest.json`
  - `com.applovin.mediation.ads`
- `Assets/Plugins/Android/mainTemplate.gradle:7`
  - `    implementation "google-admob unity-ads ironsource meta mintegral pangle"`

### MAX051: Native dependencies are present for required networks

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Dependencies`

Native dependency evidence was found for required networks.

Evidence:
- `Assets/Plugins/Android/mainTemplate.gradle:7`
  - `    implementation "google-admob unity-ads ironsource meta mintegral pangle"`
- `Packages/manifest.json`
  - `com.applovin.mediation.ads`

### MAX052: Mediated SDKs are not manually initialized

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Dependencies`

No known manual initialization calls for MAX-mediated SDKs were found.

### MAX061: Rewarded callbacks are wired

Status: `Pass` Severity: `High` Confidence: `High`

Category: `Callbacks`

Core rewarded callback evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:21`
- `Assets/Scripts/AdsManager.cs:22`
- `Assets/Scripts/AdsManager.cs:23`
- `Assets/Scripts/AdsManager.cs:24`
- `Assets/Scripts/AdsManager.cs:25`
- `Assets/Scripts/AdsManager.cs:26`
- `Assets/Scripts/AdsManager.cs:27`

### MAX005: Unity version meets policy

Status: `Pass` Severity: `Medium` Confidence: `High`

Category: `Sdk Import`

Unity version meets the configured minimum.

Evidence:
- `ProjectSettings/ProjectVersion.txt`
- `Assets`
- `ProjectSettings`
- `Packages/manifest.json`

### MAX011: Android Gradle settings are compatible

Status: `Pass` Severity: `Medium` Confidence: `High`

Category: `Platform Settings`

Android Gradle settings evidence was found.

Evidence:
- `Assets/Plugins/Android/mainTemplate.gradle:5`
  - `android.enableJetifier=true`

### MAX060: Interstitial callbacks are wired

Status: `Pass` Severity: `Medium` Confidence: `High`

Category: `Callbacks`

Core interstitial callback evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:15`
- `Assets/Scripts/AdsManager.cs:16`
- `Assets/Scripts/AdsManager.cs:17`
- `Assets/Scripts/AdsManager.cs:18`
- `Assets/Scripts/AdsManager.cs:19`
- `Assets/Scripts/AdsManager.cs:20`

### MAX064: Callback subscriptions are not duplicated

Status: `Pass` Severity: `Medium` Confidence: `High`

Category: `Callbacks`

No duplicate MAX callback subscriptions were found.

Evidence:
- `Assets/Scripts/AdsManager.cs:9`
- `Assets/Scripts/AdsManager.cs:15`
- `Assets/Scripts/AdsManager.cs:16`
- `Assets/Scripts/AdsManager.cs:17`
- `Assets/Scripts/AdsManager.cs:18`
- `Assets/Scripts/AdsManager.cs:19`
- `Assets/Scripts/AdsManager.cs:20`
- `Assets/Scripts/AdsManager.cs:21`
- `Assets/Scripts/AdsManager.cs:22`
- `Assets/Scripts/AdsManager.cs:23`
- 4 more evidence item(s) omitted. Use `--report-detail full` for complete evidence.

### MAX070: Show calls are guarded by readiness checks

Status: `Pass` Severity: `Medium` Confidence: `High`

Category: `Loading`

MAX show calls have local readiness guards.

Evidence:
- `Assets/Scripts/AdsManager.cs:36`

### MAX035: MAX APIs appear on main thread path

Status: `Pass` Severity: `Low` Confidence: `High`

Category: `Initialization`

MAX API calls do not obviously appear on background-thread paths.

Evidence:
- `Assets/Scripts/AdsManager.cs:8`
- `Assets/Scripts/AdsManager.cs:10`
- `Assets/Scripts/AdsManager.cs:28`
- `Assets/Scripts/AdsManager.cs:29`
- `Assets/Scripts/AdsManager.cs:34`
- `Assets/Scripts/AdsManager.cs:36`

### MAX043: Existing-user privacy settings path exists

Status: `Pass` Severity: `Low` Confidence: `High`

Category: `Consent`

Existing-user privacy settings evidence was found.

Evidence:
- `Assets/Scripts/AdsManager.cs:8`

## Limitations

- Static analysis only. Runtime behavior, dashboard setup, and legal compliance are not verified.
