---
name: vizbee-ios
description: Integrate Vizbee into an iOS app, grounded in developer.vizbee.tv. Use when adding Vizbee Continuity casting, a cast icon/cast bar, cast videos, HomeSSO/TV sign-in, SmartPlay/SmartHelp, or the VizbeeKit SDK to an iOS (Swift/Objective-C, UIKit or SwiftUI) app, or when reviewing or continuing such an integration.
---

# Vizbee iOS Integration

Integrate a Vizbee **product** (Continuity, HomeSSO, …) into an **iOS** app by reading the
**canonical, always-current steps from developer.vizbee.tv** and applying them to the app
in front of you.

This skill is the *orchestration + guardrail* layer. The authoritative steps, SDK versions
and API names live in the docs — **always fetch them**; never rely on memory for versions,
symbols or URLs, which change between VizbeeKit releases.

## Operating rules

1. **Confirm the product.** Usually Continuity (casting). If it's TV sign-in, it's HomeSSO.
   The product picks the doc section — see [references/doc-map.md](references/doc-map.md).
2. **Read the canonical docs before editing.** The docs site is a client-rendered SPA, but
   every page returns real HTML at the **`.html` suffix** — fetch
   `https://developer.vizbee.tv/<path>.html` (the bare path returns a "Loading …" shell).
3. **Follow the integration order:** Setup → SDK Initialization → Cast Icon → Cast Bar →
   Cast Videos → Smart Prompt → Analytics. Do the smallest slice the user asked for; don't
   pull in later steps unprompted. The first shippable slice is **Setup + Init + Cast Icon**.
4. **Gather required inputs first** — above all the **Vizbee App ID** (`vzbNNNNNNN`, from
   the Vizbee console). Never invent one. If the user doesn't have it, say where to get it
   and stop at the point init needs it.
5. **Keep edits clean and minimal.** This is a customer codebase — match its style, and add
   only short, developer-friendly comments where they genuinely help. Do **not** wrap
   insertions in `[Vizbee Begin]/[Vizbee End]` marker comments or leave long explanatory
   blocks; rely on git to show what changed.
6. **Satisfy required protocols honestly.** `VZBAppAdapterDelegate` has four required
   methods; during an init/cast-icon slice, stub the video ones with the failure callback
   and a `// [Vizbee] TODO (cast-videos slice)` note — never leave the protocol unsatisfied,
   never fake success.
7. **Verify.** Build, launch, confirm no crash on `Vizbee.start` and the cast icon renders;
   then run [references/verification.md](references/verification.md). Be explicit that
   device discovery/casting needs a real device on the same Wi-Fi as a Vizbee TV — a
   simulator can't discover.
8. **Don't commit or push** unless asked.

## Fast path

iOS · Continuity steps are distilled (verified against **VizbeeKit 6.9.3**) in
[references/ios-continuity.md](references/ios-continuity.md): SPM/CocoaPods setup,
entitlements, `Vizbee.start(withAppID:andAppAdapterDelegate:)`, the `VizbeeAppAdapter`, and
the cast icon APIs. Still open the live `*.html` page to confirm the current version and
exact symbols before editing — if the doc disagrees with the reference, the **doc wins**;
apply from the doc and update the reference in the same change.

For **custom card UI** (a customer wants Vizbee's connect/cast flow in their own card
layout/branding instead of the built-in cards), use **VZBCards** (`VizbeeCardsKit`). This is
not yet on developer.vizbee.tv, so the reference carries the knowledge:
[references/ios-custom-cards.md](references/ios-custom-cards.md) — SDK + version coupling,
`VizbeeCards.register(card:)`, the `VZBCard<ViewModel>` pattern, and theming via `setUIConfig`.

The full step loop (discover → read docs → gather inputs → apply slice → verify → report)
is in [references/workflow.md](references/workflow.md).
