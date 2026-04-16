# Squad Sports SDK

The Squad Sports SDK provides a complete, production-ready social experience for sports apps. Add community features, messaging, voice calls, polls, events, and rewards to your app in minutes — fully themed to your team's branding.

**Current version: iOS 1.6.0 · Android 1.6.0** | [Changelog](changelog.md)

## Quick Start

=== "React Native"

    ```tsx
    import { SquadExperience } from '@squad-sports/react-native';

    <SquadExperience
      partnerId="your-partner-id"
      apiKey="your-api-key"
    />
    ```

=== "iOS (Swift)"

    ```swift
    import SquadSportsSDK

    try await SquadSDK.setup(
        partnerId: "your-partner-id",
        apiKey: "your-api-key"
    )
    let vc = SquadSDK.shared.createExperienceViewController()
    present(vc, animated: true)
    ```

=== "Android (Kotlin)"

    ```kotlin
    import com.squadsports.sdk.SquadSportsSDK

    SquadSportsSDK.setup(
        context = this,
        partnerId = "your-partner-id",
        apiKey = "your-api-key",
    )
    SquadExperienceActivity.launch(this)
    ```

## Features

| Feature | Description |
|---------|-------------|
| **Community Feed** | Freestyles (audio posts), reactions, community engagement |
| **Messaging** | 1:1 messaging with audio messages and reactions |
| **[Squad Line](features/squad-line.md)** | Patented interactive voice calls with custom titles and live emoji reactions |
| **Polls** | Interactive polls with live results and nudges |
| **Events** | Game-day attendance and check-ins |
| **Wallet** | Rewards, coupons, and loyalty points |
| **Partner Auth** | Seamless authentication from your existing user base |
| **SSO** | Ticketmaster, OAuth2, and custom provider support |

## Platform Support

| Platform | Version | Package |
|----------|---------|---------|
| React Native | 0.72+ | `@squad-sports/react-native` |
| iOS | 16.0+ | `SquadSportsSDK` (Swift Package) |
| Android | API 24+ | `com.github.withyoursquad:squad-sports-android` |

## Integration Guides

- [Getting Started](getting-started/overview.md) — architecture and integration levels
- [React Native](react-native/quick-start.md) — install, configure, embed
- [iOS](ios/quick-start.md) — Swift Package Manager, UIKit, SwiftUI
- [Android](android/quick-start.md) — Gradle, Compose, Activity embedding
- [Authentication](getting-started/authentication.md) — OTP, SSO, Partner Auth
- [Security](security.md) — token storage, transport, partner isolation

## Getting Your Credentials

The Squad Sports SDK is distributed to partners through a direct onboarding process — there is no self-serve signup. Your partner ID, API key, and webhook signing secret are issued by your Squad partner manager.

To start an integration or request new credentials, email [support@squadforsports.com](mailto:support@squadforsports.com) or contact your Squad partner manager directly.
