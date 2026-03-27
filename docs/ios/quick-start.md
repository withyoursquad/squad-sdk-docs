# iOS Quick Start

## Installation

Add the Squad Sports SDK via Swift Package Manager:

```
File > Add Package Dependencies...
URL: https://github.com/withyoursquad/squad-sports-ios.git
Version: 1.6.0
```

Or in `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/withyoursquad/squad-sports-ios.git", from: "1.6.0"),
]
```

## Basic Integration

```swift
import UIKit
import SquadSportsSDK

class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    func application(_ application: UIApplication,
                     didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        window = UIWindow(frame: UIScreen.main.bounds)

        Task {
            try await SquadSDK.setup(
                partnerId: "acme-sports",
                apiKey: "sqk_live_...",
                pushToken: deviceToken
            )

            await MainActor.run {
                let vc = SquadSDK.shared.createExperienceViewController()
                window?.rootViewController = vc
                window?.makeKeyAndVisible()
            }
        }

        return true
    }
}
```

## With Partner Auth (No Login Screen)

```swift
try await SquadSDK.setup(
    partnerId: "acme-sports",
    apiKey: "sqk_live_...",
    userData: PartnerUserData(
        email: currentUser.email,
        displayName: currentUser.name,
        externalUserId: currentUser.id
    )
)
```

## With Ticketmaster SSO

```swift
try await SquadSDK.setup(
    partnerId: "acme-sports",
    apiKey: "sqk_live_...",
    ssoToken: tmAccessToken,
    ssoProvider: .ticketmaster
)
```

## Embedding in a Tab

```swift
let squadVC = SquadSDK.shared.createExperienceViewController()
tabBarController.viewControllers = [
    homeVC,
    squadVC,  // Squad as a tab
    settingsVC,
]
```

## SwiftUI

```swift
import SwiftUI
import SquadSportsSDK

struct ContentView: View {
    var body: some View {
        SquadExperienceView()
            .task {
                try? await SquadSDK.setup(
                    partnerId: "acme-sports",
                    apiKey: "sqk_live_..."
                )
            }
    }
}
```

## Requirements

- iOS 16.0+
- Xcode 15+
- Swift 5.9+
