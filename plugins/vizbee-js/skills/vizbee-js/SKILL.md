---
name: vizbee-js
description: Integrate Vizbee into a React Native / JavaScript app, grounded in developer.vizbee.tv. Use when adding Vizbee Continuity casting, a cast icon/cast bar, cast videos, HomeSSO/TV sign-in, SmartPlay/SmartHelp, or the Vizbee React Native sender SDK to a React Native or JavaScript app, or when reviewing or continuing such an integration.
---

# Vizbee React Native / JS Integration

Integrate a Vizbee **product** (Continuity, HomeSSO, …) into a **React Native / JavaScript**
app by reading the **canonical, always-current steps from developer.vizbee.tv** and applying
them to the app. This skill is the *orchestration + guardrail* layer — **always fetch the
docs**; never rely on memory for versions or symbols.

> **Fast path not yet distilled for React Native.** Drive straight from the doc pages below;
> distill verified symbols + version into `references/rn-continuity.md` when a slice is done.

## Operating rules
1. **Confirm the product** (Continuity casting is common; TV sign-in is HomeSSO); read its
   pages — see [references/doc-map.md](references/doc-map.md).
2. **Read the docs before editing.** SPA site; every page returns real HTML at the **`.html`
   suffix** — fetch `https://developer.vizbee.tv/<path>.html`. Note the current npm package
   **version** and exact JS API.
3. **Order:** Setup → SDK Initialization → Cast Icon → Cast Videos → Smart Prompt → Cast Bar
   → Notification/Lockscreen → Analytics. Smallest requested slice first.
4. **React Native has a native layer.** `Setup` covers the npm package **and** the iOS
   (pod/SPM + entitlements) and Android (Gradle + manifest) native wiring — do both native
   sides. The native rules still apply (iOS multicast / Access WiFi Information, etc.); the
   `vizbee-ios` / `vizbee-android` skills are useful companions for native specifics.
5. **Gather inputs first** — the **Vizbee App ID** (`vzbNNNNNNN`). Never invent one.
6. **Init** high in the app lifecycle (`App.tsx`/root mount, or native delegates). Cast UI
   uses the SDK's RN components.
7. **Mark every edit** with `// [Vizbee Begin] … // [Vizbee End]` (JS and native). Stub
   required callbacks honestly with `// [Vizbee] TODO (<slice>)`; never fake success.
8. **Verify:** install pods / Gradle sync, build both platforms, launch, no crash at init,
   cast icon renders. Discovery/casting needs a **real device**. Report changed files, docs
   followed, what you verified, next steps.
9. **Don't commit or push** unless asked.
