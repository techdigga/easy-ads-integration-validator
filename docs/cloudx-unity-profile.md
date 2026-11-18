# CloudX Unity Profile

The `cloudx-unity` profile inspects Unity CloudX integrations using local, read-only static evidence. Select `--mediation cloudx --profile cloudx-unity`; MAX remains the default. CloudX is included in version 1.2.0. Acceptance used the pinned demo and fixtures only.

The reviewed Unity wrapper is **4.10.0**. Known older versions fail the minimum policy; newer versions receive a compatibility-unverified warning until reviewed. This is not a blanket support promise for every later SDK. The reference is the [official 4.10.0 release](https://github.com/cloudx-io/cloudx-unity/releases/tag/v4.10.0), including its Unity package, and the [official integration guide](https://docs.cloudx.io/en/unity/integration). No vendor SDK source or binary is distributed with this validator.

```bash
easy-ads-validator scan /path/to/unity-project --mediation cloudx --profile cloudx-unity --format json --report-detail compact
```

## Scope And Evidence

- Standalone interstitial, rewarded, banner, and MREC flows are the initial scope. First Look, Trusted Arbiter, Orchestrator, and unsupported formats receive explicit coverage limits, not complete-validation claims.
- Direct import under `Assets/CloudXSdk` is recognized using package identity, API source, assembly definitions, and platform artifacts together. Package metadata alone does not prove installation. No official UPM installation route or mandatory CloudX settings asset is assumed.
- Version evidence uses the Unity wrapper's `package.json` and `PluginVersion`, not native SDK versions or archive filenames. Conflicting reliable sources are reported.
- SDK-owned examples do not establish app behavior. App-owned calls, bounded helper/event paths, consumed configuration, and platform/unit ownership are required where applicable. Opaque reflection, remote values, runtime scheduling, and truncated analysis remain uncertain.
- Dependency declarations, commented adapter templates, committed resolver output, and installed native libraries are distinct evidence. No dependency downloads, builds, dashboard queries, or device checks occur.
- Reports retain the shared JSON, Markdown, and SARIF contracts. Excluded paths are identified in coverage and excluded from scoring; an unsupported path is never counted as validated. Inspect coverage and diagnostics alongside the score.
- Keys, tokens, consent strings, raw source dumps, and absolute project paths are not needed for remediation. Prefer compact reports and request individual findings when using an agent. Reports can still include an absolute project-root metadata field; review redaction and paths before sharing them outside the project team.

## Rules

These are static checks, not guarantees that ads serve or legal requirements are satisfied. Policy can change configurable thresholds; installed SDK semantics still need reviewed evidence. Missing or opaque evidence is not proof of a defect. Privacy and best-practice concerns are advisory; no warning has Critical severity.

| Rule | Check | Remediation |
| --- | --- | --- |
| `CX001` | Recognizable, complete CloudX installation evidence. | Retain the official Unity package structure and required source identity; do not infer installation from a filename alone. |
| `CX002` | Wrapper minimum version and agreement between authoritative sources. | Use the reviewed 4.10.0 baseline and align conflicting metadata; review newer versions before claiming compatibility. |
| `CX003` | Duplicate active installations. | Keep a single active CloudX installation; distinguish inactive backups from compiled SDK copies. |
| `CX004` | Runtime and Editor assembly definitions. | Restore valid SDK assembly definitions and their intended platform boundaries. |
| `CX005` | Selected-platform bridges and integration artifacts. | Restore the required Android/iOS source artifacts from the official package. |
| `CX006` | EDM presence and minimum version. | Keep External Dependency Manager at 1.2.185 or newer for the applicable platform. |
| `CX007` | Core native dependency declarations. | Retain valid CloudX dependency XML and platform declarations; a template is not resolved output. |
| `CX008` | Platform-specific resolver settings. | Review GVH Android/iOS settings for the selected platform; manual resolution is allowed. |
| `CX010` | Unity version meets policy. | Use Unity 2022.3.62f2 or newer; verify the chosen editor against the SDK's supported toolchain. |
| `CX011` | Android API levels meet policy. | Review explicit minimum and target SDK settings; automatic or unavailable values cannot establish a concrete API level. |
| `CX012` | Enabled custom Gradle templates and toolchain evidence. | Keep enabled templates present and compatible with the resolver and reviewed SDK; disabled optional templates need not exist. |
| `CX013` | Source Android manifest and permission ownership. | Retain required source declarations or recognized build-hook additions; generated merged manifests are outside scope. |
| `CX014` | iOS deployment target. | Align the target with policy and active dependency requirements; source-only evidence does not establish a successful Xcode build. |
| `CX015` | ATT usage description and privacy source handling. | Review the owned iOS description source or build hook; do not require a generated Info.plist in the repository. |
| `CX016` | iOS build integration. | Retain the reviewed build hook and native integration declarations; do not infer generated Xcode settings. |
| `CX017` | Conditional Google app identifier configuration. | Provide an owned platform-specific app ID when an active Google adapter requires it; recognized build assignments can satisfy source evidence. |
| `CX020` | Initialization receives a usable app key. | Trace the consumed literal, helper, or configuration source; fix proven empty/placeholder values without exposing the key in reports. |
| `CX021` | Used formats receive usable ad units. | Supply a visible local fallback through the consumed code/configuration path; opaque runtime sources need review, not invented missing-value failures. |
| `CX022` | Ad-unit ownership and binding consistency. | Keep format, platform, and unit assignments consistent; inspect proven conflicting bindings. |
| `CX023` | Placement attribution. | Use stable placement values on the appropriate SDK calls and review unresolved attribution sources. |
| `CX030` | Owned initialization entry points. | Centralize initialization per platform and remove proven duplicate active owners. |
| `CX031` | Initialization state and reentry guards. | Set in-flight state before dispatch and handle success/failure transitions; missing best-practice guards are advisory. |
| `CX032` | Initialization outcome handling. | Handle owned success and failure outcomes without leaving application state blocked. |
| `CX033` | Initialization subscriptions precede dispatch. | Wire required outcome handlers before initialization through a statically visible synchronous path. |
| `CX034` | Ad work follows initialization success. | Load from a success path or an initialized guard; unresolved execution order remains uncertain. |
| `CX035` | Callback thread ownership. | Respect the actual CloudX callback/thread configuration and dispatch Unity UI work where required. |
| `CX040` | Consent ownership and initialization gating. | Review the selected consent owner and its initialization gate; no mandatory CloudX CMP asset is invented. |
| `CX041` | Manual overrides and IAB precedence. | Review explicit overrides and raw consent writes for conflicting sources of truth. |
| `CX042` | iOS ATT ownership and timing. | Keep ATT ownership coherent with the configured consent flow and review direct requests. |
| `CX043` | Audience intent and targeting. | Review applicable age/child-directed intent and targeting signals; the scanner does not determine legal applicability. |
| `CX050` | Active adapter inventory. | Distinguish active declarations from commented templates and review optional network gaps without treating them as hard failures. |
| `CX051` | Dependency resolution and version matrix. | Compare declared and committed resolved versions per platform; review missing resolution evidence and fix proven incompatible hard requirements. |
| `CX052` | Conditional network setup. | Apply only the requirements of active, applicable adapters; opaque or unaudited requirements remain review items. |
| `CX060` | Interstitial callbacks and recovery ownership. | Wire the callbacks used by application state and follow recovery to the matching owned unit; absence alone does not inherit MAX failure policy. |
| `CX061` | Rewarded callbacks and recovery ownership. | Connect the used rewarded lifecycle to application state and matching unit recovery. |
| `CX062` | Banner and MREC callbacks. | Review only callbacks needed by the implemented view lifecycle; do not require every optional event. |
| `CX063` | Subscription lifetime and duplicates. | Balance instance subscriptions and avoid proven duplicate handling paths. |
| `CX064` | Reward delivery when implemented. | Deliver rewards from the SDK reward signal and avoid proven duplicate or incorrect reward paths. |
| `CX070` | Fullscreen show readiness. | Guard shows with the matching unit readiness or review the explicit failure-handling strategy. |
| `CX071` | Retry pacing and reset. | Use bounded fixed delay or backoff and reset retry state on success; unresolved scheduling is not runtime proof. |
| `CX072` | Banner/MREC lifecycle and refresh. | Respect SDK auto-refresh and distinguish hide from destroy before recreating or reloading a view. |
| `CX073` | Recovery after hidden/display failure. | Connect terminal callbacks or owned signals to the matching unit's recovery; a disconnected flag alone is not proof. |
| `CX080` | Revenue subscription or native route evidence. | Review owned revenue intent and connector provisioning; a dependency alone does not prove native connector initialization. |
| `CX081` | Per-sink revenue payload and provisioning. | Match the installed sink/version's event and payload contract, or retain an explicit uncertainty when its route cannot be proven. |
| `CX082` | Duplicate forwarding to a destination. | Avoid sending the same impression twice through manual and native paths to the same analytics sink. |
| `CX083` | Revenue callback thread safety. | Check the actual revenue event configuration before dispatching UI/Unity work to the main thread. |
| `CX090` | Production logging configuration. | Review verbose/debug logging on production paths; this is a best-practice warning. |
| `CX091` | Debugger exposure and initialization. | Keep developer-only diagnostic entry points scoped and initialized as required; no nonexistent visual debugger is required. |
| `CX092` | Test-only diagnostics and sensitive output. | Avoid logging recognized keys, consent, or revenue payloads; this narrow heuristic is not a security audit. |
| `CX100` | Coexisting mediations and shared dependencies. | Keep owners distinct and review shared declarations; coexistence alone is not an error. |
| `CX101` | Advanced loading/arbitration coverage. | Review First Look, Trusted Arbiter, and Orchestrator paths manually where excluded; do not treat partial standalone checks as advanced-mode validation. |
| `CX102` | Unsupported format coverage. | Preserve explicit unsupported-format coverage reasons; app-open validation is not implemented. |

## Network And Revenue Reports

The network matrix reports declaration and resolution evidence, Android/iOS versions, platform gaps, and conflicts. Commented adapter templates do not become installed networks. Unrelated native libraries remain separate rather than being attributed to CloudX. The matrix cannot prove Gradle/CocoaPods resolution or native runtime initialization.

Dependency XML discovery uses fixed SDK/Editor roots plus Editor directories already present in the shared C# index, not a second unrestricted Assets traversal. Literal top-level dependency blocks can be read inside supported Unity template shapes. External scripts, local JAR inputs, generated placeholders, and opaque template sections retain incomplete resolution/compatibility evidence even when declared adapter rows are shown.

Platform template activation is reviewed for Unity 2022.3 and final releases on the Unity 6000.0 line. Other release lines remain unresolved until reviewed. Declared PlayerSettings values can appear in evidence without being presented as effective values when overrides or custom hooks are opaque. Static reports do not inspect a generated Xcode project or prove what a future build will produce.

Revenue checks recognize owned manual routes and supported native connector evidence. Reviewed manual sink contracts cover Firebase, AppsFlyer, and Adjust with version-specific limits. Native connector declarations alone are insufficient proof that forwarding is active. A connector and a manual route targeting the same sink need duplicate-forwarding review; absence of revenue tracking does not imply a mediation integration failure.

## Known Limits

- Only pinned CloudX 4.10.0 semantics have been reviewed. Package and source-tag differences are accounted for in the internal reference; newer SDK semantics are not silently assumed.
- Cross-owner event buses, opaque delegates, reflection execution, remote configuration, runtime scheduling, and arbitrary property evaluation are not executed or assumed proven.
- The recognized First Look fallback is deliberately narrow: typed CloudX failure, matching unit, and an owned same-controller helper leading to the matching MAX load. Other advanced paths retain explicit review/exclusion reasons.
- Generated Android manifests, generated Info.plist/Xcode output, native binaries, dashboard settings, network serving, and real consent UX are outside the static proof boundary. Network-derived SKAN completeness remains deferred.
- Empty iOS session-map evidence cannot establish native dependency resolution. Analytics versions and connector initialization may remain unknown.
- Representative real-project acceptance is a separate release gate. Fixture coverage alone is not a production integration guarantee.

## Official References

- [CloudX Unity integration](https://docs.cloudx.io/en/unity/integration)
- [Interstitial](https://docs.cloudx.io/en/unity/ad-formats/interstitial), [rewarded](https://docs.cloudx.io/en/unity/ad-formats/rewarded), and [banner/MREC](https://docs.cloudx.io/en/unity/ad-formats/banner-mrec)
- [Google estimated-revenue adapter](https://docs.cloudx.io/en/unity/adapters/google-estimated)
- [Adjust connector](https://docs.cloudx.io/en/unity/connectors/adjust), [AppsFlyer connector](https://docs.cloudx.io/en/unity/connectors/appsflyer), and [Singular connector](https://docs.cloudx.io/en/unity/connectors/singular)

Links provide review guidance, not a claim that every API on a live page is supported. Official runtime/build steps remain manual and are never performed by this scanner.
