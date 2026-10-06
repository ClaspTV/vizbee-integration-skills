# iOS · Continuity — distilled steps

Fast path distilled from `https://developer.vizbee.tv/continuity/ios/...`. **Always open
the live `*.html` pages to confirm the current SDK version and exact API before editing** —
symbols below were verified against **VizbeeKit 6.9.3** and may change.

Order: **Setup → SDK Initialization → Cast Icon → Cast Bar → Cast Videos → Smart Prompt →
Analytics**. The first shippable slice is Setup + Init + Cast Icon.

## 1. Setup — add the SDK (`.../integration-guide/setup.html`)

**Swift Package Manager (preferred for a plain `.xcodeproj`/`.xcworkspace`):**
- VizbeeKit → `https://github.com/ClaspTV/vizbee-ios-sdk.git` (product `VizbeeKit`)
- VizbeeHomeOSKit → `https://github.com/ClaspTV/vizbee-homeos-sdk.git` (product `VizbeeHomeOSKit`)
- **GoogleCast:** with the SPM distribution of **VizbeeKit 6.9.3** the Cast SDK is already
  vendored — adding just the two packages above builds and links, no separate GoogleCast
  step. (The manual `GoogleCastSDK-ios-no-bluetooth` embed + `strip_unused_archs.sh` the
  docs describe is for the CocoaPods/Carthage/manual paths or older versions; confirm from
  the current setup doc for the version you pin.)

CocoaPods alternative: add spec repo `https://git.vizbee.tv/Vizbee/Specs.git` + the
CocoaPods spec repo, then `pod 'VizbeeKit'` (GoogleCast `~> 4.8.0`,
`google-cast-sdk-no-bluetooth-dynamic`).

**Entitlements (Signing & Capabilities):**
- Add **Access WiFi Information**.
- Request the **multicast networking entitlement** from Apple
  (`https://developer.apple.com/contact/request/networking-multicast`) — the setup doc
  gives ready-made answers for the request form. Casting discovery needs local-network /
  multicast; iOS 14+ also prompts the user for Local Network access at runtime.

## 2. SDK Initialization (`.../integration-guide/initialization.html`)

In `AppDelegate.application(_:didFinishLaunchingWithOptions:)`, early (a pure-SwiftUI app
has no AppDelegate — add one with `@UIApplicationDelegateAdaptor` just for this):

```swift
import VizbeeKit

// real App ID from the Vizbee console
Vizbee.start(withAppID: "vzbNNNNNNN", andAppAdapterDelegate: VizbeeAppAdapter())
```

### VizbeeAppAdapter (`VZBAppAdapterDelegate`)
Four **@required** methods. For an init/cast-icon-only slice, stub the video ones with the
failure callback; implement them for real in the Cast Videos slice. Keep comments minimal.

```swift
import UIKit
import VizbeeKit

final class VizbeeAppAdapter: NSObject, VZBAppAdapterDelegate {

    func getVideoInfo(byGUID guid: String,
                      onSuccess successCallback: @escaping (Any) -> Void,
                      onFailure failureCallback: @escaping (Error) -> Void) {
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    func getVZBMetadata(fromVideo appVideoObject: Any,
                        onSuccess successCallback: @escaping (VZBVideoMetadata) -> Void,
                        onFailure failureCallback: @escaping (Error) -> Void) {
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    func getVZBStreamInfo(fromVideo appVideoObject: Any,
                          for screenType: VZBScreenType,
                          onSuccess successCallback: @escaping (VZBVideoStreamInfo) -> Void,
                          onFailure failureCallback: @escaping (Error) -> Void) {
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    func goToViewController(forGUID guid: String,
                            onSuccess successCallback: @escaping (UIViewController) -> Void,
                            onFailure failureCallback: @escaping (Error) -> Void) {
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }
}
```

> **Exact Swift labels (verified against the SDK — a wrong label is the usual compile error):**
> `getVideoInfo(byGUID:…)`, `getVZBMetadata(fromVideo:…)`,
> `getVZBStreamInfo(fromVideo:` **`for`** `screenType:…)` — the stream-info screen label is
> **`for`**, not `forScreen` — and `goToViewController(forGUID:…)`.

## 3. Cast Icon (`.../integration-guide/cast-icon.html`)

Required by Google's guidelines on the home screen. Two options:

**A. Convenience nav-bar icon** — in the home view controller's `viewWillAppear`:
```swift
Vizbee.addCastIcon(toNavigationItem: navigationItem, withViewController: self)
```

**B. Build the button yourself** (e.g. to size it, or to keep an existing bar button) —
place the cast icon beside the app's existing nav button:
```swift
let castButton = Vizbee.createCastButton()
castButton.frame = CGRect(x: 0, y: 0, width: 24, height: 24)
let castItem = UIBarButtonItem(customView: castButton)
navigationItem.rightBarButtonItems = [existingItem, castItem]
```

**SwiftUI host:** wrap `Vizbee.createCastButton()` (a `VZBCastButton : UIButton`) in a
`UIViewRepresentable` and place it in the toolbar, or add it via a UIKit
`UINavigationItem` if the screen is UIKit-hosted. If the app can't put an icon on the home
screen, it must integrate the **Cast Bar** instead (next slice).

Also add a cast button to the **player screen** (same `createCastButton()` call), so a user
watching on the phone can hand off to the TV.

## Start casting a video — SmartPlay (`.../integration-guide/cast-videos.html`)

SmartPlay is the standard way to start a video from a selection: it shows the device picker
when needed and decides TV vs phone. Build a `VZBRequest` from the app's video object and its
GUID, then call `Vizbee.smartPlay`:

```swift
let request = VZBRequest(appVideo: appVideo, guid: guid, startPosition: 0)
request.didPlay(onTV:    { screen in /* now playing on the TV */ })
request.doPlay(onPhone:  { status in /* keep playing on the phone */ })
Vizbee.smartPlay(request, presenting: presentingViewController)
```

For this to start the video on the TV, the app adapter's `getVZBStreamInfo(...)` must return a
real `VZBVideoStreamInfo` (stream URL, DRM, …). Returning failure there tells the SDK **not**
to start playback — only do that if the app starts TV playback by some other means of its own.

## SmartHelp (`.../integration-guide/smart-prompt.html`)

SmartHelp surfaces contextual connect/help prompts. The no-options form just takes a
presenting view controller, typically called from the home screen's `viewDidAppear`:

```swift
Vizbee.smartHelp(presentingViewController)   // returns Bool; the SDK decides when to show
```

## Verify (this slice)
- App compiles and launches with the SDK linked.
- No crash on `Vizbee.start` (wrong/empty App ID is the usual cause).
- Cast icon renders; on a real device on the same network as a Vizbee-enabled TV it turns
  active and opens the cast flow. Simulator can't discover devices — build/launch/icon
  presence is the most you can confirm there.
