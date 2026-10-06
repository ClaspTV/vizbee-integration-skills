# Vizbee Android TV / Fire TV docs map (developer.vizbee.tv)

**Fetch rule:** append **`.html`** to any path
(`https://developer.vizbee.tv/continuity/ftv-atv/integration-guide/setup.html`); the bare
path returns a "Loading …" shell. Re-derive from `…/continuity/overview/intro.html` via the
`href="/continuity/ftv-atv..."` nav links.

## Continuity · Fire TV & Android TV (native) — `/continuity/ftv-atv/...`
- `integration-guide/setup`
- `integration-guide/initialization`
- `integration-guide/video-deeplink-handling`
- `integration-guide/video-playback-handling`
- `integration-guide/analytics`

Also: `conceptual-guide`, `testing-guide`, `/continuity/release-notes/ftv`, `/continuity/release-notes/atv`.

## Continuity · React Native Fire TV & Android TV — `/continuity/ftv-atv-react-native/...`
- `integration-guide/{setup,initialization,video-deeplink-handling,video-playback-handling}`
- `conceptual-guide`, `testing-guide`, `/continuity/release-notes/ftv-atv-react-native`
  (pair with the `vizbee-js` skill for the JS layer)

## HomeSSO · Fire TV & Android TV
TV sign-in is under the HomeSSO section — derive exact paths from `intro.html` if requested.

## SDK source
Gradle/Maven — take coords and **version** from the live `setup.html`; do not hard-code a
version here. GitHub mirror: `ClaspTV/Vizbee-Android-SDK`.
