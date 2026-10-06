# Verification checklist

What to confirm after each slice, and what genuinely can't be verified without a device or
the Vizbee console. Be honest in the report about which is which.

## Build & launch (every slice, verifiable locally)
- The project builds with the Vizbee SDK linked (no missing-symbol/link errors).
- On iOS, a missing **manual GoogleCast** embed or a missing entitlement is the usual
  first failure — check the setup doc.
- The app launches and does not crash during `Vizbee.start` / SDK init. An empty or wrong
  **App ID** is the most common init crash/no-op.

## SDK initialization
- Init runs once, early (iOS `AppDelegate`; Android `Application`).
- A real App ID (`vzbNNNNNNN`) is in place — not a placeholder — before you claim init works.
- `isProduction`/production config is set correctly for a store build (iOS: the
  `isProduction:` init variant must be `true` for App Store builds).

## Cast icon
- The cast button renders on the home screen's navigation bar (or as the floating icon).
- **Device discovery / active state needs a real device** on the same Wi-Fi as a
  Vizbee-enabled TV (Roku, Fire TV, Samsung, LG, Chromecast, …). Simulators/emulators can't
  discover — don't claim casting works from one. iOS also shows a **Local Network** access
  prompt on first discovery; it must be allowed.

## Cast videos (later slice)
- The adapter maps the app's real media object → `VZBVideoMetadata` / `VZBVideoStreamInfo`
  (iOS) with a stable GUID == the app's mediaID.
- Tapping cast on a playing video hands off to the TV and controls work.

## Analytics / console (needs Vizbee console)
- Sessions and events show up in the Vizbee console / analytics for the App ID.
- Cross-check with the Vizbee Analytics skill/MCP if available.

## Report template
- Files changed (+ why), doc pages followed (URLs).
- Verified here: build ✓/✗, launch ✓/✗, init no-crash ✓/✗, icon visible ✓/✗.
- Needs device/console: discovery, casting, analytics.
- Exact next inputs/steps (real App ID, Apple multicast entitlement, next slice).
