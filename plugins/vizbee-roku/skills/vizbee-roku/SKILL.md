---
name: vizbee-roku
description: Integrate Vizbee into a Roku app, grounded in developer.vizbee.tv. Use when adding Vizbee Continuity (cross-device casting receiver, deep-link / video handoff), HomeSSO/TV sign-in, or the Vizbee Roku SDK to a Roku (BrightScript / SceneGraph) channel, or when reviewing or continuing such an integration.
---

# Vizbee Roku Integration

Integrate a Vizbee **product** (Continuity receiver, HomeSSO, …) into a **Roku**
(BrightScript / SceneGraph) channel by reading the **canonical, always-current steps from
developer.vizbee.tv** and applying them. This skill is the *orchestration + guardrail*
layer — **always fetch the docs**; never rely on memory.

> **Fast path not yet distilled for Roku.** Drive straight from the doc pages below; distill
> verified components + version into `references/roku-continuity.md` when a slice is done.

## Operating rules
1. **Confirm the product and role.** Roku is usually the **receiver/TV side** of Continuity
   (accept a cast, handle a deep-linked video) and/or the TV side of **HomeSSO** sign-in.
   See [references/doc-map.md](references/doc-map.md).
2. **Read the docs before editing.** SPA site; every page returns real HTML at the **`.html`
   suffix** — fetch `https://developer.vizbee.tv/<path>.html`. Roku paths live under the
   Omni / HomeSSO / Continuity-receiver nav; derive from `…/continuity/overview/intro.html`.
3. **Order (Roku flow):** Setup → SDK Initialization → Handle Sign In Request / deep-link
   handling → Handle Start Video → testing. Smallest requested slice first.
4. **Gather inputs first** — the **Vizbee App ID** (`vzbNNNNNNN`). Never invent one.
5. **Where things go:** init early in the main scene / `init()`; wire the Vizbee
   `.brs`/SceneGraph nodes per the doc; handle sign-in and video-start callbacks.
6. **Keep edits clean and minimal** — match the channel's style, short comments only where
   they help, no marker comments. Stub not-yet-implemented handlers honestly; never fake success.
7. **Verify:** sideload to a **real Roku device** (the emulator can't do cross-device
   discovery), confirm init and the targeted flow. Report changed files, docs followed, what
   you verified, next steps.
8. **Don't commit or push** unless asked.
