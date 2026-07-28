# MAX Unity Profile

The `max-unity` profile implements static MAX mediation checks. Default severities below are the rule defaults before policy overrides.

Static-analysis limits apply to all rules: the scanner reads committed repository files only, parses C# syntax without semantic compilation, and does not run Unity, mobile builds, SDKs, ad platforms, or device flows.

Many MAX rules use the shared MAX SDK API reference. The reference is extracted from committed `Assets/MaxSdk/Scripts` files when available, falls back to an embedded MAX Unity reference, and can be extended with local `maxUnity.apiAliases` policy entries for project wrappers.

## SDK Import And Version

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX001` | Detects MAX plugin evidence in Unity packages or known MAX/AppLovin asset/plugin artifacts. | Critical | Cannot see uncommitted imports or generated-only artifacts. | Import the MAX Unity plugin and commit package or asset evidence. |
| `MAX002` | Compares discovered MAX Unity plugin version with `minimumMaxPluginVersion`. | High | Returns `UNKNOWN` when version text is absent or unparsable. | Upgrade the MAX Unity plugin to the configured minimum or newer. |
| `MAX003` | Looks for External Dependency Manager evidence in settings or committed files. | Medium | Missing EDM4U may be generated or omitted from fixtures, so absence is `UNKNOWN`. | Add/commit EDM4U or equivalent dependency resolution evidence. |
| `MAX004` | Checks Android resolver settings for disabled resolution and Jetifier-related static evidence. | Medium | Requires committed resolver settings; missing settings are `UNKNOWN`. | Enable Android dependency resolution and keep Jetifier aligned with policy. |
| `MAX005` | Reads Unity version and compares it with `minimumUnityVersion`. | Medium | Cannot infer version when `ProjectVersion.txt` is missing or malformed. | Upgrade Unity or adjust policy if the project intentionally uses an older supported editor. |

## Platform Settings

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX010` | Checks Android minimum API, target API, and required Jetifier evidence. | High | Requires committed Android settings/templates. Missing fields are `UNKNOWN`. | Set Android API levels and Jetifier according to policy. |
| `MAX011` | Looks for Android Gradle template/settings evidence and flags obvious incompatible settings. | Medium | Does not execute Gradle or resolve dependency graphs. | Commit compatible Gradle templates and repository/dependency settings. |
| `MAX012` | Checks iOS deployment target against `minimumDeploymentTarget`. | Medium | Missing or unparsable iOS target evidence is `UNKNOWN`. | Set iOS deployment target to the policy threshold or higher. |
| `MAX013` | Checks `NSUserTrackingUsageDescription` evidence when ATT description is required and committed iOS plist/postprocess evidence exists. | High | Treats projects with no committed iOS output as pass-level static scope because Unity may generate the final plist only during build. | Add ATT usage description in plist, settings, localization, or postprocess evidence when iOS output is committed. |
| `MAX014` | Checks committed or generated SKAdNetwork ID evidence when required. | High | Generated-only evidence returns `UNKNOWN` when committed project evidence is absent. | Commit SKAdNetwork IDs or document/commit generated output evidence. |
| `MAX021` | Checks Google/AdMob app ID evidence when `google-admob` is required. | High | Warns on absence; cannot query Google or AdMob configuration. | Add Android/iOS Google app ID evidence in supported Unity or platform files. |

## SDK Keys, App IDs, And Ad Units

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX020` | Checks AppLovin/MAX SDK key evidence when MAX use is detected, including variable-backed `MaxSdk.SetSdkKey(...)` calls. | Critical | Does not validate the key against AppLovin. | Add SDK key evidence in MAX settings, manifest metadata, code, or generated files. |
| `MAX022` | Looks for ad unit ID or ad-format organization evidence. | High | Placeholder detection is token-based and cannot validate IDs server-side. | Define real ad unit IDs and organize them by format. |
| `MAX023` | Checks placement names on show calls or nearby placement analytics evidence. | Low | Local syntax heuristics cannot prove every show path has analytics context. | Pass placement names to show calls or emit placement analytics near show calls. |

## Initialization

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX030` | Detects `MaxSdk.InitializeSdk()` when MAX evidence exists. | Critical | Missing C# index produces `UNKNOWN`. | Initialize MAX from a central ad service before ad loading/showing. |
| `MAX031` | Checks that actual `MaxSdk.InitializeSdk` calls are singular. | High | Multiple syntax call sites may be runtime-exclusive; rule warns rather than fails. | Centralize initialization or guard alternative call paths. |
| `MAX032` | Checks for local state guards around initialization calls. | Medium | Only sees simple local guards. | Add an initialized/initializing guard around MAX initialization. |
| `MAX033` | Checks initialized callback subscription order relative to initialization in the same method. | High | Cross-method ordering is `UNKNOWN`; syntax order is not runtime proof. | Subscribe to `OnSdkInitializedEvent` before calling `InitializeSdk()`. |
| `MAX034` | Checks that ad loading starts after initialization or is guarded by initialized state. | High | Cross-method paths often become `UNKNOWN`; burst loading may warn. | Start ad loading from initialization completion or behind initialized guards. |
| `MAX035` | Looks for MAX API calls on obvious background-thread paths. | Low | Heuristic token check only; cannot prove actual thread affinity. | Marshal MAX SDK calls to the Unity/main-thread path. |

## Consent And Privacy

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX040` | Checks CMP, UMP, MAX consent, or custom consent evidence when required. | Critical | Cannot judge whether the consent UX or jurisdiction handling is legally sufficient. | Add a consent flow before ad initialization/loading, or disable requirement by policy only when appropriate. |
| `MAX041` | Checks consent evidence appears before MAX initialization in the same method. | High | Cross-method ordering is `UNKNOWN`; evidence is limited to known CMP/consent APIs to avoid unrelated substring matches. | Run consent setup before initializing or loading MAX ads. |
| `MAX042` | Checks privacy policy URL/path evidence. | Medium | Does not fetch or inspect the policy page. | Configure a privacy policy URL/path in consent or project settings. |
| `MAX043` | Checks existing-user privacy settings path evidence. | Low | Missing evidence is advisory `UNKNOWN` by default. | Provide a settings entry point for existing users to revisit privacy choices. |
| `MAX044` | Checks age-related or child-directed flag evidence when policy requires it. | Low | The scanner cannot infer audience or app category. | Add appropriate child-directed/age flags or set policy to match the app audience review. |

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
| `MAX052` | Flags known direct manual initialization calls for MAX-mediated SDKs. | High | Only known syntax tokens are detected. | Remove direct initialization for mediated SDKs and let MAX mediate them. |

## Callback Wiring

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX060` | Checks core interstitial callbacks when interstitial use exists; missing display/click-only evidence is low severity. | Medium | Detects subscriptions syntactically, not whether handlers execute. | Wire load, fail, and hidden callbacks; add display/click callbacks when useful for analytics or debugging. |
| `MAX061` | Checks core rewarded callbacks, including reward callback; missing display/click-only evidence is low severity. | High | Detects subscriptions syntactically only. | Wire rewarded load/fail/hide/reward/revenue callbacks; add display/click callbacks when useful for analytics or debugging. |
| `MAX062` | Checks banner/MREC callbacks when banner or MREC use exists; missing click-only evidence is low severity. | Medium | Returns `UNKNOWN` when banner/MREC use is absent. | Wire relevant banner and MREC load/fail callbacks; add click callbacks when useful for analytics or debugging. |
| `MAX063` | Checks app-open callbacks when app-open use exists. | Medium | Returns `UNKNOWN` when app-open use is absent. | Wire app-open load, display, hide/fail, and revenue callbacks where used. |
| `MAX064` | Detects duplicate callback subscriptions without obvious unsubscribe/singleton protection. | Medium | Lifecycle semantics are approximated from syntax. | Move subscriptions to a singleton path or pair lifecycle subscriptions with unsubscribe. |

## Loading, Showing, And Retry

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX070` | Checks show calls for local readiness guards such as `IsInterstitialReady`. | Medium | Only local guards are recognized. | Guard show calls with the matching MAX readiness check. |
| `MAX071` | Checks retry scheduling evidence after failed loads. | Medium | Does not execute timers/coroutines or prove retry reachability. | Add bounded retry scheduling in load-failure callbacks. |
| `MAX072` | Checks retry delay evidence for exponential or bounded behavior. | Medium | Pattern-based check for caps, counters, multipliers, delays, or `Math.Pow`. | Cap retries or use exponential/backoff delays. |
| `MAX073` | Checks video ads reload after close or fail callbacks, including MAX terminal callback handlers, scheduled reloads, and clear event-handler reload paths. | Medium | Static evidence can detect common indirect paths but cannot prove runtime event dispatch. | Reload interstitial/rewarded ads after hidden/closed and failed-load callbacks, directly or through a clear event pipeline. |
| `MAX074` | Checks whether ad loading is staged or meaningfully delayed after initialization. | Low | Advisory heuristic; cannot prove performance impact. | Stagger or delay loading rather than starting all formats in one burst. |

## Revenue And Analytics

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX080` | Checks MAX ad revenue callback evidence when revenue tracking is required. | Critical | Does not verify server-side reporting. | Subscribe to MAX ad revenue callbacks. |
| `MAX081` | Checks forwarding from revenue callbacks to configured analytics sinks, or marks project-specific event forwarding for manual review. | High | Token-based sink recognition; cannot prove downstream event listeners reach analytics. | Forward revenue payloads to configured analytics sinks, add project-specific sink tokens, or manually verify indirect event pipelines. |
| `MAX082` | Checks revenue payload field evidence such as ad unit, network, placement, revenue, currency, precision, creative, or country. | Low | Does not validate completeness at runtime. | Include important MAX revenue fields in forwarded analytics payloads. |

## Debugging And Production Safety

| Rule | Check | Default severity | Static-analysis limits | Remediation pattern |
| --- | --- | --- | --- | --- |
| `MAX090` | Checks MAX Ad Review or quality service evidence. | Low | Missing evidence is `UNKNOWN` unless policy requires quality service. | Configure MAX Ad Review/quality service evidence when the release process requires it. |
| `MAX091` | Checks that Creative Debugger is not obviously disabled unexpectedly. | Low | Only obvious `SetCreativeDebuggerEnabled(false)`-style evidence is detected. | Remove unconditional Creative Debugger disabling unless intentional. |
| `MAX092` | Flags unconditional debug logs, test modes, debugger display calls, or test ad unit evidence in production-sensitive paths. | Critical | Build-path scope may be uncertain, producing `WARN` instead of `FAIL`. | Guard debug/test hooks behind development builds or remove them before release. |
