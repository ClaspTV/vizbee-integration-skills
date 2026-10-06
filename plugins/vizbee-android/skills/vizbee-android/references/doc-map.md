# Vizbee Android docs map (developer.vizbee.tv)

**Fetch rule:** append **`.html`** to any path
(`https://developer.vizbee.tv/continuity/android/integration-guide/setup.html`); the bare
path returns a "Loading …" shell. Re-derive from `…/continuity/overview/intro.html` via the
`href="/continuity/android/..."` nav links.

## Continuity · Android (`/continuity/android/...`)
- `integration-guide/setup` — Gradle dependency (Maven), manifest permissions/services, Google Cast / Play Services Cast
- `integration-guide/initialization` — initialize the SDK in the `Application` subclass
- `integration-guide/cast-icon`
- `integration-guide/cast-bar`
- `integration-guide/cast-videos`
- `integration-guide/notification-lockscreen-controls`
- `integration-guide/smart-prompt`
- `integration-guide/analytics`

Also: `conceptual-guide`, `testing-guide`, `ux-guide/*`, `/continuity/release-notes/android`.

## HomeSSO · Android
TV sign-in from the Android app is under the HomeSSO section — derive exact paths from
`intro.html` if requested.

## SDK source
Delivered via Gradle/Maven — take the repo URL, artifact coords and **current version** from
the live `integration-guide/setup.html`; do not hard-code a version here. GitHub mirror:
`ClaspTV/Vizbee-Android-SDK`.
