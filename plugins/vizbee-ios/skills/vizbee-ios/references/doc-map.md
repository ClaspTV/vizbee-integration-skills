# Vizbee iOS docs map (developer.vizbee.tv)

**Fetch rule:** the site is a client-rendered SPA. A bare path returns a `Loading …`
shell; append **`.html`** to get the real content. Example:
`https://developer.vizbee.tv/continuity/ios/integration-guide/setup.html`

To re-derive this map, fetch `…/continuity/overview/intro.html` and read the
`href="/continuity/ios/..."` links in the rendered nav.

## Continuity · iOS (`/continuity/ios/...`)

Integration guide (append `.html`):
- `integration-guide/setup` — SDK install (SPM / CocoaPods / Carthage / manual), entitlements, VizbeeAppAdapter
- `integration-guide/initialization` — `Vizbee.start(...)` in AppDelegate
- `integration-guide/cast-icon` — nav-bar / floating cast button
- `integration-guide/cast-bar` — persistent cast bar
- `integration-guide/cast-videos` — map app videos → Vizbee, start casting
- `integration-guide/smart-prompt` — smart install / sign-in prompts
- `integration-guide/analytics` — analytics hooks

Also: `conceptual-guide`, `testing-guide`, `ux-guide/overview` (+ `customize-theme`,
`customize-cast-button`, `customize-tv-icons`, `customize-overlay-cards`,
`customize-interstitial-cards`, `customize-specific-cards`), and
`/continuity/release-notes/ios`.

## HomeSSO · iOS
TV sign-in from the iOS app lives under the HomeSSO section of the nav
(`.../ios/...` pages for SDK init, Handle Sign In Request, etc.). Derive exact paths from
`intro.html` nav if a HomeSSO integration is requested.

## SDK sources (confirm the latest version from the setup doc / repo tags before pinning)

| SDK | iOS source |
|---|---|
| VizbeeKit | SPM `https://github.com/ClaspTV/vizbee-ios-sdk.git` (product `VizbeeKit`) |
| VizbeeHomeOSKit | SPM `https://github.com/ClaspTV/vizbee-homeos-sdk.git` (product `VizbeeHomeOSKit`) |
| GoogleCast | **manual** (no SPM) — download link in the setup doc; add `strip_unused_archs.sh` run-script phase |

CocoaPods: spec repo `https://git.vizbee.tv/Vizbee/Specs.git` + CocoaPods Specs, pod `VizbeeKit`.
