# iOS · VZBCards — custom card UI (VizbeeCardsKit)

**When to use:** a customer wants Vizbee's connect/cast flow to render in *their own* card
UI (fully custom layout, branding, animations) instead of the SDK's built-in cards. VZBCards
is the supported way to do that. **This is not yet on developer.vizbee.tv**, so this file is
the reference — keep it generic and verify symbols against the SDK version you pin.

VZBCards lets the app **replace a card's view** while the SDK keeps driving the flow: each
card binds to a typed view model the SDK supplies, and is composed from themed SDK pieces.

## 1. Add the SDK

SPM: `https://github.com/ClaspTV/vizbee-cards-sdk.git`, product **`VizbeeCardsKit`**.

> **Version coupling (critical):** VizbeeCardsKit is a binary built against a specific
> **VizbeeKit** version — its Package.swift declares no VizbeeKit dependency, so SPM will
> **not** warn you about a mismatch; it fails at build/runtime instead. Pin VizbeeKit to the
> version that Cards release was built against (e.g. VizbeeCardsKit **1.0.0** pairs with
> VizbeeKit **6.8.100**). Confirm the pairing for your versions before upgrading either.

## 2. Register the custom cards

Once, right after `Vizbee.start(...)`:

```swift
import VizbeeKit
import VizbeeCardsKit

VizbeeCards.setup()
VizbeeCards.register(card: .deviceStatus)    { MyDeviceStatusCard() }
VizbeeCards.register(card: .appInstall)      { MyAppInstallCard() }
VizbeeCards.register(card: .manualAppInstall){ MyManualAppInstallCard() }
VizbeeCards.register(card: .pairing)         { MyPairingCard() }
```

Register only the cards you're customizing; any you don't register keep the SDK's built-in
card. Card registration must happen before the flow that shows them.

## 3. Implement a card

Each custom card subclasses `VZBCard<ViewModel>` and overrides `bind(with:)`. The view model
is the SDK's typed state for that card; read it to populate your views and to drive actions.

```swift
import UIKit
import VizbeeKit
import VizbeeCardsKit

class MyDeviceStatusCard: VizbeeCardsKit.VZBCard<VZBDeviceStatusCardViewModel> {
    override func bind(with viewModel: VZBDeviceStatusCardViewModel) {
        // Build/update your UIView hierarchy from viewModel.
        // Wire buttons to the view model's actions (e.g. disconnect) via VZBActionButton.
    }
}
```

Card type → view model (verify against your SDK version):
`.deviceStatus` → `VZBDeviceStatusCardViewModel` · `.appInstall` → `VZBAppInstallCardViewModel`
· `.manualAppInstall` → `VZBManualAppInstallCardViewModel` · `.pairing` → `VZBPairingCardViewModel`.
Actions are driven through `VZBActionButton` (role/appearance) exposed on the view model.

## 4. Theme the SDK pieces

Cards compose from themeable SDK components. Apply a declarative theme with
`Vizbee.setUIConfig(_:)` (a `[String: Any]` tree of `references` (brand tokens), `ids`, and
`classes`), typically light/dark variants layered on the SDK's `LightTheme`/`DarkTheme`
bases. Keep the brand's colors, fonts and metrics in one tokens file so re-skinning touches
only that file. (Older SDKs also expose `setUIConfig(_:layouts:)` taking a `VZBLayoutsConfig`;
use the single-argument form unless a layouts object is required for your version.)

## 5. Assets & fonts

Custom cards usually reference image assets and brand fonts by name. Add the imagesets to the
app's asset catalog and register fonts in `Info.plist` `UIAppFonts` — but if the app already
bundles a font, reuse it rather than adding a duplicate (two files with the same PostScript
name conflict).

## Verify

- Builds and links with the paired VizbeeKit + VizbeeCardsKit versions.
- App launches without crashing on `VizbeeCards.setup()` / registration / `setUIConfig`.
- The custom cards actually render **during the connect/cast flow on a real device** — a
  simulator can't discover devices, so launch-without-crash is the most you can confirm there.
