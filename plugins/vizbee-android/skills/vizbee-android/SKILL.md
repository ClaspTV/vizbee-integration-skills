---
name: vizbee-android
description: Integrate Vizbee into an Android app, grounded in developer.vizbee.tv. Use when adding Vizbee Continuity casting, a cast icon/cast bar, cast videos, HomeSSO/TV sign-in, SmartPlay/SmartHelp, or the Vizbee Android SDK to an Android (Kotlin/Java) app, or when reviewing or continuing such an integration.
---

# Vizbee Android Integration

Integrate a Vizbee **product** (Continuity, HomeSSO, …) into an **Android** app by reading
the **canonical, always-current steps from developer.vizbee.tv** and applying them to the
app in front of you. This skill is the *orchestration + guardrail* layer — **always fetch
the docs**; never rely on memory for versions or symbols.

> **Fast path not yet distilled for Android.** Drive straight from the doc pages below, and
> distill verified symbols + SDK version into `references/android-continuity.md` (mirroring
> the iOS skill) when a slice is done.

## Operating rules
1. **Confirm the product** (Continuity casting is common; TV sign-in is HomeSSO); read its
   pages — see [references/doc-map.md](references/doc-map.md).
2. **Read the docs before editing.** SPA site; every page returns real HTML at the **`.html`
   suffix** — fetch `https://developer.vizbee.tv/<path>.html`. Note the current SDK
   **version** and exact symbols.
3. **Order:** Setup → SDK Initialization → Cast Icon → Cast Bar → Cast Videos → Smart Prompt
   → Analytics. Smallest requested slice first (Setup + Init + Cast Icon).
4. **Gather inputs first** — the **Vizbee App ID** (`vzbNNNNNNN`). Never invent one.
5. **Where things go:** init in the `Application` subclass `onCreate`; dependency via Gradle
   (Maven coords from the setup doc); permissions/services in `AndroidManifest.xml`; cast
   icon on the home `Activity`/toolbar.
6. **Keep edits clean and minimal** — match the app's style, short comments only where they help, no marker comments. Stub not-yet-implemented
   required callbacks with a failure/no-op + `// [Vizbee] TODO (<slice>)`, never faking success.
7. **Verify:** Gradle build, launch, no crash at init, cast icon renders. Discovery/casting
   needs a **real device** on the same Wi-Fi as a Vizbee TV (emulators can't discover).
   Report files changed, docs followed, what you verified, next steps.
8. **Don't commit or push** unless asked.
