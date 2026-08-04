# MAX Unity Profile

The `max-unity` profile implements static MAX mediation checks. Default severities below are the rule defaults before policy overrides.

Static-analysis limits apply to all rules: the scanner reads committed repository files only, parses C# syntax without semantic compilation, and does not run Unity, mobile builds, SDKs, ad platforms, or device flows.

Many MAX rules use the shared MAX SDK API reference. The reference is extracted from committed `Assets/MaxSdk/Scripts` files when available, falls back to an embedded MAX Unity reference, and can be extended with local `maxUnity.apiAliases` policy entries for project wrappers.

## SDK Import And Version

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX001` | Detects one MAX install mode: direct Unity package import under known asset roots, or UPM package `com.applovin.mediation.ads`. | Critical | Cannot see uncommitted imports or generated-only artifacts. Direct plus UPM evidence is a duplicate install failure. | Keep exactly one MAX install mode and commit package or asset evidence. |
| `MAX002` | Compares discovered MAX Unity plugin version with `minimumMaxPluginVersion` (`8.6.0` by default). | High | Returns `UNKNOWN` when version text is absent or unparsable. | Upgrade the MAX Unity plugin to the configured minimum or newer. |
| `MAX003` | Requires External Dependency Manager evidence and version `minimumExternalDependencyManagerVersion` (`1.2.185` by default) when MAX is imported. | Medium | Returns `UNKNOWN` when EDM exists but its version is absent or unparsable. | Install or upgrade External Dependency Manager for Unity and commit resolver evidence. |
| `MAX004` | Checks Android resolver settings from `ProjectSettings/GvhProjectSettings.xml` and resolved AppLovin output in `AndroidResolverDependencies.xml`. | Medium | Requires committed resolver settings/output; generated-only or manually resolved libraries that are not committed cannot be proven. | Keep Android resolver settings aligned with policy and commit AppLovin resolver output evidence. |
| `MAX005` | Reads Unity version and compares it with `minimumUnityVersion` (`2022.3.62f2` by default). | Medium | Cannot infer version when `ProjectVersion.txt` is missing or malformed. | Upgrade to the configured minimum or newer; Unity 6 is recommended for current ad network and Google Play policy support. |
| `MAX006` | Requires MAX SDK assembly definition files for direct imports and checks committed UPM package contents when available. | High | Manifest-only UPM installs may be `UNKNOWN` because package contents are not committed. | Commit/import the complete MAX package with expected asmdefs. |
| `MAX007` | Requires committed MAX `AppLovinSettings.asset` evidence. | High | Generated settings files do not satisfy the rule because existing projects should include the settings asset. | Open MAX settings or Integration Manager in Unity, configure the project, and commit `AppLovinSettings.asset`. |

## Platform Settings

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX010` | Checks Android min API and target API only; default target is API `36`. | High | Requires committed Unity Android settings or Gradle template API values. Jetifier belongs to `MAX004`. | Set Android min API to `24+` and target API to `36+`. |
| `MAX011` | Checks Unity custom Gradle template toggles against committed Gradle template files. | Medium | Does not execute Gradle or resolve dependency graphs; optional templates are only checked when their toggles are enabled. | Align custom Gradle template toggles with files under `Assets/Plugins/Android`. |
| `MAX012` | Checks iOS deployment target against hard minimum `13.0` and recommended target `15.0`. | Medium | Missing, conflicting, or unparsable iOS target evidence is `UNKNOWN`. | Use iOS deployment target `15.0+` when possible; below `13.0` fails. |
| `MAX013` | Checks ATT usage description source evidence, including MAX consent-flow privacy URL and base usage description. | High | Does not inspect generated Info.plist/Xcode output and does not prove runtime ATT behavior. | Configure MAX Terms and Privacy Policy Flow with a production HTTPS privacy URL and base tracking usage description, or provide project-owned source plist/postprocess evidence. |
| `MAX014` | Skips strict SKAdNetwork validation pending the dedicated SKAN source model. | High | Missing SKAN evidence does not warn or fail in this pass. | No immediate action; revisit after the SKAN deep-dive rule is implemented. |
| `MAX015` | Checks project-authored Android manifest ownership and ad permission evidence. | High | Does not inspect the final merged manifest and does not require AppLovin build-time entries in source manifests. | If using a custom manifest, commit it with `INTERNET`, `ACCESS_NETWORK_STATE`, and `AD_ID` where required. |
| `MAX021` | Checks Google/AdMob app IDs from AppLovin settings when Google demand is required. | High | Does not query AdMob and does not inspect generated platform output. | Configure valid Android and iOS Google app IDs in AppLovin settings and avoid placeholder test IDs. |

## SDK Keys, App IDs, And Ad Units

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX020` | Requires valid SDK key evidence in `AppLovinSettings.asset`; deprecated `MaxSdk.SetSdkKey(...)` usage warns when settings are valid. | Critical | Does not validate the key against AppLovin and does not allow runtime calls to replace the settings asset. | Configure the SDK key in `AppLovinSettings.asset`, remove placeholder/conflicting values, and remove `SetSdkKey` calls. |
| `MAX022` | Checks each detected MAX ad format for non-placeholder local fallback ad unit IDs and blocks obvious duplicate reuse. | High | Cannot validate dashboard state or remote config values that are not committed. | Define real ad unit IDs by format/platform in committed config or code, and avoid Google sample IDs or shared IDs across incompatible slots. |
| `MAX023` | Checks placement names on direct MAX show calls and banner/MREC placement setters. | Low | Missing or dynamic placement evidence warns; random unrelated placement strings no longer satisfy the rule. | Pass short placement/source names to show calls or placement setters; direct placement values over 24 characters fail. |

## Initialization

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX030` | Detects `MaxSdk.InitializeSdk()` when MAX evidence exists. | Critical | Missing C# index produces `UNKNOWN`. | Initialize MAX from a central ad service before ad loading/showing. |
| `MAX031` | Fails when multiple direct `MaxSdk.InitializeSdk()` call sites are visible. | High | Compile-time platform exclusivity is only recognized when the source index exposes it; runtime call count is not proven. | Centralize initialization so each supported platform has one effective call site. |
| `MAX032` | Checks local initialization guards and visible pre-call initializing/requested state transitions. | Medium | Only simple syntax-level guards and assignments are recognized; failure and callback state paths may remain incomplete. | Check initialized/initializing state, set the in-progress flag before MAX init, clear it on failure, and mark completion from the initialized callback. |
| `MAX033` | Requires the public initialized callback and checks its order relative to initialization. | High | Cross-method ordering is a `WARN` because syntax-only analysis cannot prove runtime execution order. | Subscribe to `MaxSdkCallbacks.OnSdkInitializedEvent` before calling `InitializeSdk()`. |
| `MAX034` | Fails same-method loads before initialization and otherwise checks initialized callbacks/guards. | High | Cross-method paths become `WARN`; runtime ordering is not proven. | Start ad loading from initialization completion or behind initialized guards. |
| `MAX035` | Warns on obvious background-thread MAX calls and Unity UI/scene mutation in revenue callback handlers. | Low | Heuristic token check only; cannot prove actual thread affinity or dispatcher correctness. | Marshal MAX SDK calls and callback UI changes to the Unity/main-thread path. |

## Consent And Privacy

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX040` | Checks consent flow evidence; complete MAX-managed privacy flow additionally requires a full Privacy Policy URL and base tracking usage description. | Critical | Cannot judge whether consent UX or jurisdiction handling is legally sufficient. Standalone setters do not prove a flow. | Add a CMP/UMP/custom consent flow before initialization, or complete MAX-managed privacy-flow settings. |
| `MAX041` | Treats complete MAX-managed privacy flow as automatic consent handling; otherwise checks manual consent before initialization and warns on manual consent-state writes. | High | Cross-method ordering is a `WARN`; runtime execution order is not proven. | Keep MAX flow settings complete or apply manual CMP/consent results before `InitializeSdk()`. |
| `MAX042` | Checks for a full Privacy Policy URL when consent is required or MAX-managed flow is enabled; HTTPS passes and HTTP warns. | Medium | Does not fetch or inspect the policy page. | Configure a full production HTTPS Privacy Policy URL. |
| `MAX043` | Neutral compatibility rule; existing-user privacy settings paths are not checked in the current MAX Unity profile. | Low | This project-specific UX is intentionally out of scope. | No action required by this rule. |
| `MAX044` | Provides an advisory self-review for age-related or child-directed flags. | Low | The scanner cannot infer audience or legal obligations. | Review the app audience and configure age flags before initialization when required. |

## Mediated Networks And Native Dependencies

MAX reports include an `Ad Network Matrix` before individual findings when the `max-unity` profile is used. The matrix is a compact static view of policy-required and detected mediated networks. It checks whether adapter evidence, dependency XML, committed Android resolver evidence, and static Android/iOS adapter version strings are present for each network. Markdown reports show concise `Android`, `iOS`, and `Resolution` columns; JSON matrix rows preserve the detailed evidence fields and also expose nullable `androidAdapterVersion` and `iosAdapterVersion` fields for agent consumers.

Matrix statuses:

- `resolved`: AppLovin adapter evidence appears in committed resolver output.
- `needs-resolution-evidence`: adapter or dependency XML evidence exists, but committed Android resolver output was not found.
- `version-mismatch`: multiple Android or iOS adapter versions were found for the same network.
- `missing`: no AppLovin adapter evidence was found for the network.
- `needs-review`: static evidence was ambiguous.

The matrix does not run External Dependency Manager, Gradle, CocoaPods, Unity, or dashboard APIs. Dynamic version ranges such as `10.+` are displayed as static evidence, not resolved to concrete package versions. Singular adapter-version fields are populated only when exactly one static version was found for that platform; they are `null` in JSON and `unknown` in Markdown when evidence is missing or ambiguous.

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX050` | Compares `requiredNetworks` with detected adapter/dependency evidence, including common MAX adapter aliases such as `google-adapter`, `facebook-adapter`, and `bytedance-adapter`. | High | Incomplete dependency evidence returns `UNKNOWN`; no dashboard/API confirmation. | Add required adapter/dependency evidence or adjust policy network list. |
| `MAX051` | Checks native dependency evidence for required networks. | High | Missing committed dependency files may produce `UNKNOWN` or `WARN`. | Commit resolver XML, Gradle, pods, postprocess, or plugin evidence for required networks. |
| `MAX052` | Neutral compatibility rule; direct mediated SDK initialization is not checked in the current MAX Unity profile. | Low | No mediated SDK initialization claim is made because direct setup can be legitimate for another integration. | Review adapter documentation separately when an adapter requires additional setup. |

## Callback Wiring

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX060` | Requires loaded, load-failed, display-failed, and hidden interstitial callbacks; display-failed recovery is reviewed and display/click tracking is low priority. | Medium | Detects subscriptions and visible handler calls syntactically, not runtime dispatch. | Wire the four core callbacks and check readiness before reloading or scheduling an interstitial after display failure. |
| `MAX061` | Requires loaded, load-failed, display-failed, and hidden rewarded callbacks; reward-grant, display, and click callbacks are not required. | High | Detects subscriptions and visible handler calls syntactically only. | Wire the four core callbacks and check readiness before reloading or scheduling a rewarded ad after display failure. |
| `MAX062` | Checks banner/MREC callbacks when banner or MREC use exists; missing click-only evidence is low severity. | Medium | Returns `UNKNOWN` when banner/MREC use is absent. | Wire relevant banner and MREC load/fail callbacks; add click callbacks when useful for analytics or debugging. |
| `MAX063` | Neutral compatibility rule; app-open callback validation is not checked by default. | Medium | App-open validation is intentionally deferred. | No action required by this rule. |
| `MAX064` | Detects duplicate MAX callback subscriptions and repeatable lifecycle registrations without a visible guard or matching unsubscribe. | Medium | Lifecycle semantics are approximated from syntax. | Register callbacks once centrally, guard registration, or unsubscribe from `OnDisable`/teardown. |

## Loading, Showing, And Retry

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX070` | Checks interstitial and rewarded show calls for matching local readiness guards; app-open is ignored. | Medium | Only local guards are recognized. | Guard fullscreen show calls with the matching MAX readiness check. |
| `MAX071` | Checks fullscreen failure paths for reload/retry evidence; immediate reload warns, while banner auto-refresh is exempt. | Medium | Does not execute timers/coroutines or prove retry reachability. | Reload from failure callbacks and add a fixed delay or bounded backoff. |
| `MAX072` | Checks retry behavior for a delay, cap, clamp, or backoff; immediate reload warns and no retry remains unknown. | Medium | Pattern-based check cannot prove dynamic scheduler bounds. | Pair fixed delays with retry caps or use capped exponential backoff. |
| `MAX073` | Checks each used interstitial/rewarded format for terminal reload or signal evidence after hidden/load/display failure callbacks. | Medium | Static evidence cannot prove runtime event dispatch or arbitrary event-bus delivery. | Reload or signal the next fullscreen load from every used format’s terminal paths. |
| `MAX074` | Advises staging initial interstitial/rewarded/banner/MREC loads; app-open is ignored and configured banner auto-refresh is exempt. | Low | Advisory syntax check; cannot prove startup performance. | Add a small delay, scheduler, priority path, or demand trigger between initial format loads. |

## Revenue And Analytics

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX080` | Checks non-app-open MAX revenue callback coverage only when revenue tracking intent or policy requires it. | Medium | Does not verify server-side reporting or runtime callback delivery. | Subscribe to the relevant MAX ad revenue callbacks for used formats. |
| `MAX081` | Checks direct analytics forwarding and project event subscribers; visible subscriber payload usage can pass, while unproven downstream paths remain manual review. | Medium | Cannot prove downstream analytics ingestion or runtime event delivery. | Forward revenue payloads to analytics, or manually verify indirect event pipelines. |
| `MAX082` | Advises on useful revenue payload fields and warns on direct Unity UI/scene mutation from revenue callback handlers. | Low | Does not validate runtime thread affinity or payload completeness. | Preserve useful MAX ad info fields and dispatch UI work to the Unity main thread. |

## Debugging And Production Safety

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX090` | Recommends MAX Ad Review, MAX Ad Quality, or an equivalent creative quality workflow. | Low | Missing dashboard configuration cannot be verified statically. | Configure an ad quality/review workflow when useful for release support. |
| `MAX091` | Warns only when Creative Debugger is explicitly disabled; absence of a call is acceptable because MAX defaults remain active. | Low | Cannot prove runtime debugger access. | Avoid disabling Creative Debugger unless intentional because creative diagnosis access may be lost. |
| `MAX092` | Warns on unguarded or guarded MAX debug/test hooks; it never fails by default. | Medium | Static analysis cannot prove the final production build path. | Remove or guard verbose logging, debugger, test-device, and sample/test hooks for release. |
