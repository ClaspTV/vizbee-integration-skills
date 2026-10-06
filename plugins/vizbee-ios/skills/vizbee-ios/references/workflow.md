# Integration workflow

A repeatable loop for any Vizbee product × platform. Keep each pass to the smallest slice
the user asked for.

## 1. Discover the target app
- Platform & toolchain: iOS (`.xcodeproj`/`.xcworkspace`, SPM vs CocoaPods vs Carthage),
  Android (Gradle), RN, etc.
- Composition root / app entry: `AppDelegate`/`SceneDelegate` (iOS), `Application`
  (Android), `App.tsx` (RN). The SDK init goes here.
- Where the home / primary screen's navigation bar is built (the cast icon goes there).
- Deployment target / min OS, and existing dependency list (avoid version clashes).

## 2. Read the canonical docs (never skip)
- From [doc-map.md](doc-map.md), pick the product/platform pages for the slice.
- Fetch each with the **`.html` suffix**. Read **Setup** and **Initialization** at minimum.
- Note the **current SDK version** and the **exact API names** from the page — these
  change between releases; the page is the source of truth, not any snapshot in this repo.

## 3. Gather required inputs
- **Vizbee App ID** (`vzbNNNNNNN`) — mandatory for init. From the Vizbee console. If the
  user can't supply it, stop at init and tell them where to get it.
- Platform entitlements/permissions the setup doc calls out (iOS: Access WiFi Information +
  multicast request; Android: permissions/services in the manifest).
- Any app-specific objects the adapter must map (the app's video/media model) — only for
  the cast-videos slice.

## 4. Apply the slice
- Add the SDK dependency exactly as the setup doc specifies for this app's dependency
  manager.
- Make the code edits for the slice. Keep them clean and minimal — match the app's style
  and add only short comments where they help; **no `[Vizbee Begin]/[Vizbee End]` markers or
  long explanatory blocks** (they look out of place in a customer codebase; git shows the diff).
- For required-but-not-yet-implemented delegate methods (e.g. the iOS
  `VZBAppAdapterDelegate` video methods during an init-only slice), implement them as
  honest stubs that call the failure callback — never leave the protocol unsatisfied, and
  never fake success.

## 5. Verify
- Build. Fix compile/link errors against the doc (often a missing entitlement, a manual
  GoogleCast step, or a wrong symbol from a stale memory).
- Launch and run the checks in [verification.md](verification.md) relevant to the slice.
- Be explicit about what you could verify here (build, launch, icon visible) vs. what
  needs a physical device or the Vizbee console (actual device discovery/casting).

## 6. Report
- List the files changed and why, the doc pages you followed (with URLs), what you
  verified, and the exact next inputs/steps needed (e.g. "paste real App ID", "request
  multicast entitlement", "do the next slice: Cast Bar").
- Don't commit or push unless asked.

## When updating this repo's reference files
If a freshly-fetched doc page contradicts a reference file here, the doc wins: apply from
the doc, then update the reference file in the same change and note it, so the distilled
fast-path stays trustworthy.
