# vizbee-integration-skills

Reusable **Agent Skills** for integrating Vizbee into a host app, distributed as a Claude
Code plugin marketplace. Each skill reads the **canonical, always-current integration steps
from [developer.vizbee.tv](https://developer.vizbee.tv)** for its platform and the chosen
product, then applies them to the app in front of it — so one skill works across many apps
and stays correct as the SDKs evolve.

**One plugin per platform.** **iOS is the focus and the only fully-distilled skill** today
(verified symbols + fast path). The other platforms ship as **doc-driven starters** — they
carry the same workflow and guardrails and drive straight from developer.vizbee.tv, ready
for their respective devs to flesh out and distill (see *Extending* below).

| Plugin | Platform | Status |
|---|---|---|
| `vizbee-ios` | iOS (Swift/Obj-C, UIKit/SwiftUI) | ✅ distilled fast path |
| `vizbee-android` | Android (Kotlin/Java) | 🟡 doc-driven starter |
| `vizbee-js` | React Native / JavaScript | 🟡 doc-driven starter |
| `vizbee-roku` | Roku (BrightScript/SceneGraph) | 🟡 doc-driven starter |
| `vizbee-androidtv-firetv` | Android TV / Fire TV | 🟡 doc-driven starter |

This is the *integration* companion to
[`vizbee-ai-skills`](https://github.com/ClaspTV/vizbee-ai-skills) (analytics / reporting).

## How a skill works

The skill is an orchestration + guardrail layer, not a copy of the docs:

1. Identify the **product** (Continuity, HomeSSO, …) for that platform.
2. Fetch the exact doc pages — the site renders real HTML at the **`.html` suffix**.
3. Gather required inputs — above all the **Vizbee App ID** (`vzbNNNNNNN`).
4. Apply the smallest requested slice (Continuity order: Setup → Init → Cast Icon → Cast
   Bar → Cast Videos → Smart Prompt → Analytics) with clean, minimal edits that match the
   app's style — no marker comments or verbose blocks.
5. Build, verify, and report what still needs a device or the Vizbee console.

The `vizbee-ios` skill additionally ships a **distilled fast path** (verified against
VizbeeKit 6.9.3): SPM/CocoaPods setup, entitlements, `Vizbee.start(...)`, the
`VizbeeAppAdapter`, and the cast-icon APIs. See `plugins/vizbee-ios/skills/vizbee-ios/references/`.

## Install

```bash
claude plugin marketplace add ClaspTV/vizbee-integration-skills
claude plugin install vizbee-ios@vizbee-integration
```

For a team, commit this marketplace to a repo everyone opens, and they get it on trusting
the folder:

```json
{
  "extraKnownMarketplaces": {
    "vizbee-integration": {
      "source": { "source": "github", "repo": "ClaspTV/vizbee-integration-skills" }
    }
  },
  "enabledPlugins": ["vizbee-ios@vizbee-integration"]
}
```

Then just ask, e.g. *"Integrate Vizbee Continuity casting into this iOS app"* — the skill
triggers, reads the docs, and does the slice.

## Repo layout

```
.claude-plugin/marketplace.json              # marketplace definition (lists the platform plugins)
plugins/
  vizbee-ios/
    .claude-plugin/plugin.json               # plugin manifest
    skills/vizbee-ios/
      SKILL.md                               # orchestrator (iOS)
      references/
        doc-map.md                           # iOS product → developer.vizbee.tv URLs
        workflow.md                          # discover → read docs → apply → verify → report
        ios-continuity.md                    # iOS Continuity fast path (verified symbols)
        verification.md                      # what to check; device/console caveats
```

## Extending to another platform

Add a sibling plugin (e.g. `plugins/vizbee-android/`) with the same shape:

1. `plugins/<name>/.claude-plugin/plugin.json` — manifest (`name`, `description`, author,
   homepage).
2. `plugins/<name>/skills/<name>/SKILL.md` — a platform-focused orchestrator. Keep the same
   operating rules as `vizbee-ios` (read docs via the `.html` suffix, gather the App ID
   first, follow the Setup→…→Analytics order, keep edits clean and minimal with no marker
   comments, satisfy required callbacks honestly, build & verify, don't commit).
3. `plugins/<name>/skills/<name>/references/doc-map.md` — the platform's doc paths on
   developer.vizbee.tv.
4. Register the plugin in `.claude-plugin/marketplace.json`.
5. As you complete real integrations, distill verified symbols + SDK version into a
   `references/<platform>-continuity.md` fast path (mirror `ios-continuity.md`).

**Rule for every platform:** if a live doc page disagrees with a reference file here, the
**doc wins** — apply from the doc and update the reference in the same change.
