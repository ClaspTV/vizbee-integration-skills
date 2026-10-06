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
- **GoogleCast** is **not** on SPM — download `GoogleCastSDK-ios-no-bluetooth-<ver>_dynamic`
  from the link in the setup doc, embed it, and add a Run Script build phase running
  `strip_unused_archs.sh` (ships in `GoogleCast/Tools`).

CocoaPods alternative: add spec repo `https://git.vizbee.tv/Vizbee/Specs.git` + the
CocoaPods spec repo, then `pod 'VizbeeKit'`.

**Entitlements (Signing & Capabilities):**
- Add **Access WiFi Information**.
- Request the **multicast networking entitlement** from Apple
  (`https://developer.apple.com/contact/request/networking-multicast`) — the setup doc
  gives ready-made answers for the request form. Casting discovery needs local-network /
  multicast; iOS 14+ also prompts the user for Local Network access at runtime.

## 2. SDK Initialization (`.../integration-guide/initialization.html`)

In `AppDelegate.application(_:didFinishLaunchingWithOptions:)`, early:

```swift
import VizbeeKit

// [Vizbee Begin] - SDK init
let myVizbeeID = "vzbNNNNNNN" // real App ID from the Vizbee console
Vizbee.start(withAppID: myVizbeeID, andAppAdapterDelegate: VizbeeAppAdapter())
// [Vizbee End] - SDK init
```

### VizbeeAppAdapter (`VZBAppAdapterDelegate`)
Four **@required** methods (ObjC names → Swift). For an init/cast-icon-only slice, stub
the video ones with the failure callback; implement them for real in the Cast Videos slice.

```swift
import UIKit
import VizbeeKit

final class VizbeeAppAdapter: NSObject, VZBAppAdapterDelegate {

    // Fetch the app's video object for a GUID (your mediaID).
    func getVideoInfo(byGUID guid: String,
                      onSuccess successCallback: @escaping (Any) -> Void,
                      onFailure failureCallback: @escaping (Error) -> Void) {
        // [Vizbee] TODO (cast-videos slice): look up the app's video model by guid.
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    // Map the app's video object → VZBVideoMetadata.
    func getVZBMetadata(fromVideo appVideoObject: Any,
                        onSuccess successCallback: @escaping (VZBVideoMetadata) -> Void,
                        onFailure failureCallback: @escaping (Error) -> Void) {
        // [Vizbee] TODO (cast-videos slice): map metadata (guid,title,imageURL,isLive,...).
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    // Map the app's video object → VZBVideoStreamInfo for a target screen type.
    func getVZBStreamInfo(fromVideo appVideoObject: Any,
                          forScreen screenType: VZBScreenType,
                          onSuccess successCallback: @escaping (VZBVideoStreamInfo) -> Void,
                          onFailure failureCallback: @escaping (Error) -> Void) {
        // [Vizbee] TODO (cast-videos slice): provide the stream URL/DRM for screenType.
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }

    // Deep link: return the VC that should present/play a GUID.
    func goToViewController(forGUID guid: String,
                            onSuccess successCallback: @escaping (UIViewController) -> Void,
                            onFailure failureCallback: @escaping (Error) -> Void) {
        // [Vizbee] TODO (deeplink slice): route to the app's player VC for guid.
        failureCallback(NSError(domain: "VizbeeAppAdapter", code: -1))
    }
}
```

> Note the exact labels: `getVZBStreamInfo` uses **`forScreen screenType: VZBScreenType`**
> (ObjC `forScreen:`), and `getVideoInfo` is **`byGUID`**, `goToViewController` is
> **`forGUID`**. These are the Swift bridgings of the ObjC selectors — a wrong label is the
> most common compile error here.

## 3. Cast Icon (`.../integration-guide/cast-icon.html`)

Required by Google's guidelines on the home screen. Two options:

**A. Convenience nav-bar icon** — in the home view controller's `viewWillAppear`:
```swift
// [Vizbee Begin] - Cast Icon
Vizbee.addCastIcon(toNavigationItem: navigationItem, withViewController: self)
// [Vizbee End] - Cast Icon
```

**B. Build the button yourself** (e.g. to size it or place it in a SwiftUI nav bar):
```swift
// [Vizbee Begin] - Cast Icon
let castButton = Vizbee.createCastButton()
castButton.frame = CGRect(x: 0, y: 0, width: 24, height: 24)
let item = UIBarButtonItem(customView: castButton)
navigationItem.rightBarButtonItems = [item]
// [Vizbee End] - Cast Icon
```

**SwiftUI host:** wrap `Vizbee.createCastButton()` (a `VZBCastButton : UIButton`) in a
`UIViewRepresentable` and place it in the toolbar, or add it via a UIKit
`UINavigationItem` if the screen is UIKit-hosted. If the app can't put an icon on the home
screen, it must integrate the **Cast Bar** instead (next slice).

## Verify (this slice)
- App compiles and launches with the SDK linked.
- No crash on `Vizbee.start` (wrong/empty App ID is the usual cause).
- Cast icon renders; on a real device on the same network as a Vizbee-enabled TV it turns
  active and opens the cast flow. Simulator can't discover devices — build/launch/icon
  presence is the most you can confirm there.
