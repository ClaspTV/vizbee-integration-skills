# Vizbee React Native / JS docs map (developer.vizbee.tv)

**Fetch rule:** append **`.html`** to any path
(`https://developer.vizbee.tv/continuity/react-native/integration-guide/setup.html`); the
bare path returns a "Loading …" shell. Re-derive from `…/continuity/overview/intro.html` via
the `href="/continuity/react-native/..."` nav links.

## Continuity · React Native (`/continuity/react-native/...`)
- `integration-guide/setup` — npm package + iOS (pod/SPM, entitlements) and Android (Gradle, manifest) native wiring
- `integration-guide/sdk-init`
- `integration-guide/cast-icon`
- `integration-guide/cast-videos`
- `integration-guide/smart-prompt`
- `integration-guide/cast-bar`
- `integration-guide/notification-lockscreen-controls`
- `integration-guide/analytics`

Also: `ux-guide`, `conceptual-guide`, `testing-guide`, `/continuity/release-notes/react-native`.

## TV / receiver JS
RN Fire TV & Android TV receivers:
`/continuity/ftv-atv-react-native/integration-guide/{setup,initialization,video-deeplink-handling,video-playback-handling}`
(pair with the `vizbee-androidtv-firetv` skill for native TV specifics).

## SDK source
npm package + native iOS/Android SDKs. Take the exact package name, **version**, and native
setup from the live `setup.html`; do not hard-code a version here.
