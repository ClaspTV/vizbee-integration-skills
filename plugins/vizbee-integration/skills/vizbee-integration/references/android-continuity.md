# Android · Continuity — distilled steps

Driven by `https://developer.vizbee.tv/continuity/android/...`. The Android API/symbols are
**not yet distilled here** — fetch the live `*.html` pages and follow them exactly, using
the same workflow as iOS. Confirm the current SDK version from the setup page / release
notes before pinning.

Pages (append `.html`):
- `integration-guide/setup` — Gradle dependency (Maven), permissions/services in the
  `AndroidManifest`, Google Cast / Play Services Cast
- `integration-guide/initialization` — initialize the SDK in your `Application` subclass
  with the **Vizbee App ID** and an app adapter
- `integration-guide/cast-icon` — MediaRouteButton / Vizbee cast button on the home screen
- `integration-guide/cast-videos` — map app media → Vizbee, start casting
- `integration-guide/cast-bar`, `integration-guide/notification-lockscreen-controls`,
  `integration-guide/smart-prompt`, `integration-guide/analytics`
- `conceptual-guide`, `testing-guide`, `ux-guide/*`

Order and guardrails are identical to iOS: Setup → Initialization → Cast Icon → Cast Bar →
Cast Videos → Smart Prompt → Analytics; gather the App ID first; wrap edits in
`// [Vizbee Begin] … // [Vizbee End]`; implement required adapter callbacks honestly
(stub with failure, never fake success) until their slice; build and verify each slice.

When you complete an Android integration, distill the verified symbols into this file
(mirroring `ios-continuity.md`) so the next app gets the fast path.
