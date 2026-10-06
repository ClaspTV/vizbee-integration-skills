# vizbee-integration-skills

Reusable **Agent Skills** for integrating Vizbee into a host app, distributed as a Claude
Code plugin marketplace. The skill reads the **canonical, always-current integration steps
from [developer.vizbee.tv](https://developer.vizbee.tv)** for the chosen product and
platform, then applies them to the app in front of it — so one skill works across many apps
and stays correct as the SDKs evolve.

| Plugin | Skill | Covers |
|---|---|---|
| `vizbee-integration` | `vizbee-integration` | Adding a Vizbee product (Continuity, HomeSSO, …) to an app on any platform (iOS, Android, React Native, Fire TV / Android TV, Roku, Smart TV, Chromecast), driven by the docs |

This is the *integration* companion to
[`vizbee-ai-skills`](https://github.com/ClaspTV/vizbee-ai-skills) (analytics / reporting).

## How it works

The skill is an orchestration + guardrail layer, not a copy of the docs:

1. Identify the **product** and **platform**.
2. Fetch the exact doc pages (the site renders real HTML at the **`.html` suffix**).
3. Gather required inputs — above all the **Vizbee App ID** (`vzbNNNNNNN`).
4. Apply the smallest requested slice (for Continuity: Setup → Init → Cast Icon → Cast Bar
   → Cast Videos → Smart Prompt → Analytics), wrapping every edit in
   `// [Vizbee Begin] … // [Vizbee End]`.
5. Build, verify, and report what still needs a device or the Vizbee console.

iOS · Continuity is distilled into a fast-path reference (verified against VizbeeKit 6.9.3);
other product/platform combos drive straight from the docs. See the skill's `references/`.

## Install

```bash
claude plugin marketplace add ClaspTV/vizbee-integration-skills
claude plugin install vizbee-integration@vizbee-integration
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
  "enabledPlugins": ["vizbee-integration@vizbee-integration"]
}
```

Then just ask, e.g. *"Integrate Vizbee Continuity casting into this iOS app"* — the skill
triggers, reads the docs, and does the slice.

## Repo layout

```
.claude-plugin/marketplace.json          # marketplace definition
plugins/vizbee-integration/
  .claude-plugin/plugin.json             # plugin manifest
  skills/vizbee-integration/
    SKILL.md                             # orchestrator
    references/
      doc-map.md                         # product × platform → developer.vizbee.tv URLs
      workflow.md                        # discover → read docs → apply → verify → report
      ios-continuity.md                  # iOS Continuity fast path (verified symbols)
      android-continuity.md              # Android Continuity (drives from docs)
      verification.md                    # what to check; device/console caveats
```

## Contributing

When you complete an integration for a product/platform not yet distilled here, add a
`references/<platform>-<product>.md` mirroring `ios-continuity.md` with the **verified**
symbols and SDK version, so the next app gets the fast path. If a live doc page ever
disagrees with a reference here, the **doc wins** — fix the reference in the same change.
