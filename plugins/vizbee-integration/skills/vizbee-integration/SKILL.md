---
name: vizbee-integration
description: Integrate a Vizbee product into a host app, grounded in developer.vizbee.tv. Use when adding Vizbee casting/Continuity, HomeSSO/sign-in, SmartPlay/SmartHelp, a cast icon/cast bar, or the Vizbee SDK to an iOS, Android, React Native, Fire TV, Android TV, Roku, Smart TV or Chromecast app, or when reviewing/continuing such an integration.
---

# Vizbee Integration

Integrate a Vizbee **product** (Continuity, HomeSSO, Omni, …) into a host app on a
**platform** (iOS, Android, React Native, Fire TV / Android TV, Roku, Smart TV,
Chromecast) by reading the **canonical, always-current steps from developer.vizbee.tv**
and applying them to the app in front of you.

This skill is the *orchestration + guardrail* layer. The authoritative integration
steps, SDK versions and API names live in the docs — **always fetch them**; never rely
on memory for versions, symbols or URLs, which change between SDK releases.

## Operating rules

1. **Identify product + platform first.** Ask or infer which Vizbee product and which
   platform. Both pick the exact doc section. If unsure, read
   [references/doc-map.md](references/doc-map.md).
2. **Read the canonical docs before editing.** The docs site is a client-rendered SPA,
   but every page returns real HTML at the **`.html` suffix** — fetch
   `https://developer.vizbee.tv/<path>.html` (not the bare path, which returns a
   "Loading …" shell). The page→URL map is in
   [references/doc-map.md](references/doc-map.md).
3. **Follow the integration order** for the product/platform (Setup → SDK
   Initialization → Cast Icon → Cast Bar → Cast Videos → Smart Prompt → Analytics for
   Continuity). Do the smallest shippable slice the user asked for; don't pull in later
   steps unprompted.
4. **Gather the required inputs up front** — above all the **Vizbee App ID**
   (`vzbNNNNNNN`, from the Vizbee console). Never invent one. If the user doesn't have
   it, say where to get it and stop at the point it's needed.
5. **Mark every edit** so the integration is reviewable and reversible. Wrap inserted
   code in `// [Vizbee Begin] … // [Vizbee End]` (or the platform's comment syntax), the
   convention the Vizbee docs themselves use.
6. **Verify.** After each slice, build the app and confirm it compiles and launches, then
   run the relevant checks in [references/verification.md](references/verification.md).
   Report what you verified and what still needs a device/console.
7. **Don't commit or push** unless asked. Leave the working tree with clearly-marked,
   described changes.

## Workflow

Read [references/workflow.md](references/workflow.md) for the step-by-step loop
(discover → read docs → gather inputs → apply slice → verify → report).

Platform/product specifics that this skill has already distilled from the docs (use as a
fast path, but still open the live doc page to confirm versions and exact API before
editing):

- **iOS · Continuity** → [references/ios-continuity.md](references/ios-continuity.md)
- **Android · Continuity** → [references/android-continuity.md](references/android-continuity.md)

For any product/platform without a reference file here, drive the integration straight
from the doc pages in [references/doc-map.md](references/doc-map.md) — the same workflow
applies.

## What this skill does not do

- It does not replace the docs. If a reference file here disagrees with a freshly-fetched
  doc page, the **doc page wins** — update the reference file and tell the user.
- It does not obtain App IDs, provision Vizbee console apps, or request Apple
  entitlements on the user's behalf — it tells them exactly what to request and where.
