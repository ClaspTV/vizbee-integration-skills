---
name: vizbee-androidtv-firetv
description: Integrate Vizbee into an Android TV or Fire TV app, grounded in developer.vizbee.tv. Use when adding Vizbee Continuity (cross-device casting receiver, deep-link / video handoff), HomeSSO/TV sign-in, or the Vizbee SDK to an Android TV / Fire TV (Kotlin/Java, or React Native for TV) app, or when reviewing or continuing such an integration.
---

# Vizbee Android TV / Fire TV Integration

Integrate a Vizbee **product** (Continuity receiver, HomeSSO, …) into an **Android TV /
Fire TV** app by reading the **canonical, always-current steps from developer.vizbee.tv**
and applying them. This skill is the *orchestration + guardrail* layer — **always fetch the
docs**; never rely on memory.

> **Fast path not yet distilled for Android TV / Fire TV.** Drive straight from the doc pages
> below; distill verified symbols + version into `references/ftv-atv-continuity.md` when done.

## Operating rules
1. **Confirm the product and role.** These are the **TV/receiver side** of Continuity (accept
   a cast, handle a deep-linked video, play it) and/or the TV side of **HomeSSO** sign-in.
   See [references/doc-map.md](references/doc-map.md).
2. **Read the docs before editing.** SPA site; every page returns real HTML at the **`.html`
   suffix** — fetch `https://developer.vizbee.tv/<path>.html`. Note the current SDK
   **version** and exact symbols.
3. **Order:** Setup → SDK Initialization → Video Deeplink Handling → Video Playback Handling
   → Analytics (native), or the HomeSSO flow (Setup → Init → Handle Sign In Request → Handle
   Start Video). Smallest requested slice first.
4. **Native vs React Native for TV:** `/continuity/ftv-atv/...` for native Kotlin/Java,
   `/continuity/ftv-atv-react-native/...` for the RN-for-TV variant (pair with `vizbee-js`).
5. **Gather inputs first** — the **Vizbee App ID** (`vzbNNNNNNN`). Never invent one.
6. **Where things go:** init in the `Application`/leanback entry; deep-link and playback
   handlers wired to the app's player; Gradle dependency + manifest.
7. **Keep edits clean and minimal** — match the app's style, short comments only where they help, no marker comments. Stub not-yet-implemented
   handlers with `// [Vizbee] TODO (<slice>)`; never fake success.
8. **Verify:** build and deploy to a **real Fire TV / Android TV device** (emulators can't do
   cross-device discovery), confirm init and the targeted flow. Report changed files, docs
   followed, what you verified, next steps.
9. **Don't commit or push** unless asked.
