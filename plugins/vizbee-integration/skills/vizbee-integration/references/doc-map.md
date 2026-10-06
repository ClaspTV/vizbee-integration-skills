# Vizbee docs map (developer.vizbee.tv)

**Fetch rule:** the site is a client-rendered SPA. A bare path returns a `Loading …`
shell; append **`.html`** to get the real content. Example:
`https://developer.vizbee.tv/continuity/ios/integration-guide/setup.html`

To re-derive this map at any time, fetch `…/continuity/overview/intro.html` and read the
`href="/continuity/..."` links in the rendered nav.

## Products

| Product | Overview page | What it covers |
|---|---|---|
| Continuity | `/continuity/overview/intro` | Casting, installs, deep links, cross-device sign-in |
| HomeSSO | (under Continuity nav → HomeSSO) | Cross-device / TV sign-in from mobile |
| Omni | `/omni/overview/intro` | Build-once TV apps (Roku, FireTV, Samsung, …) |
| HomeGraph | `/homegraph/overview` | Household device map |

## Continuity — platforms and integration-guide pages

Each platform's integration guide lives under
`/continuity/<platform>/integration-guide/<page>`. Platforms seen in the nav:
`ios`, `android`, `react-native`, `ftv-atv` (Fire TV & Android TV), `ftv-atv-react-native`,
`chromecast`. Plus per-platform `conceptual-guide`, `testing-guide`, `ux-guide/*`, and
`/continuity/release-notes/<platform>`.

### iOS · Continuity (`/continuity/ios/...`)
- `integration-guide/setup` — SDK install (SPM / CocoaPods / Carthage / manual), entitlements, VizbeeAppAdapter
- `integration-guide/initialization` — `Vizbee.start(...)` in AppDelegate
- `integration-guide/cast-icon` — nav-bar / floating cast button
- `integration-guide/cast-bar` — persistent cast bar
- `integration-guide/cast-videos` — map app videos → Vizbee, start casting
- `integration-guide/smart-prompt` — smart install/sign-in prompts
- `integration-guide/analytics` — analytics hooks
- `conceptual-guide`, `testing-guide`, `ux-guide/overview` (+ `customize-theme`, `customize-cast-button`, `customize-tv-icons`, `customize-overlay-cards`, `customize-interstitial-cards`, `customize-specific-cards`)

### Android · Continuity (`/continuity/android/...`)
- `integration-guide/setup`, `integration-guide/initialization`, `integration-guide/cast-icon`,
  `integration-guide/cast-videos`, `integration-guide/smart-prompt`, `integration-guide/cast-bar`,
  `integration-guide/notification-lockscreen-controls`, `integration-guide/analytics`
- `conceptual-guide`, `testing-guide`, `ux-guide/*`

### React Native · Continuity (`/continuity/react-native/...`)
- `integration-guide/setup`, `sdk-init`, `cast-icon`, `cast-videos`, `smart-prompt`, `cast-bar`,
  `notification-lockscreen-controls`, `analytics`, `ux-guide`, `conceptual-guide`, `testing-guide`

### TV receivers (Continuity)
- Fire TV & Android TV: `/continuity/ftv-atv/integration-guide/{setup,initialization,video-deeplink-handling,video-playback-handling,analytics}`
- RN Fire TV & Android TV: `/continuity/ftv-atv-react-native/integration-guide/{setup,initialization,video-deeplink-handling,video-playback-handling}`
- Chromecast: `/continuity/chromecast/integration-guide/{initialization,vizbee-custom-channel,vizbee-load-request-interceptor}`

### Shared
- `/continuity/overview/{intro,sender-receiver-sdks,supported-devices,configuration-service,conversion-flows}`
- `/continuity/data-dictionary`, `/continuity/data-filtering`, `/continuity/api-insights`
- Reference Guide (API docs) — linked from the nav as an external "Reference Guide ↗"

## SDK sources (confirm the latest version from the doc/tags before pinning)

| SDK | iOS (SPM) | Android |
|---|---|---|
| VizbeeKit | `https://github.com/ClaspTV/vizbee-ios-sdk.git` | Maven (see Android setup doc) |
| VizbeeHomeOSKit | `https://github.com/ClaspTV/vizbee-homeos-sdk.git` | — |
| GoogleCast | manual (no SPM) — download from the setup doc | Play Services Cast |

iOS CocoaPods spec repo: `https://git.vizbee.tv/Vizbee/Specs.git` (pod `VizbeeKit`).
