# Squad Sports SDK

Drop-in social features for sports apps. Squad adds community, messaging, voice calls, polls, events, and more to your existing app with a single component.

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

    try await SquadSportsSDK.setup(
        partnerId: "your-partner-id",
        apiKey: "your-api-key"
    )
    let vc = SquadSportsSDK.shared.createExperienceViewController()
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

## What You Get

| Feature | Description |
|---------|-------------|
| **Community Feed** | Freestyles (audio posts), reactions, community engagement |
| **Messaging** | 1:1 and group messaging with audio messages |
| **Squad Line** | Real-time voice calls between fans |
| **Polls** | Interactive polls with live results |
| **Events** | Game-day attendance and check-ins |
| **Wallet** | Rewards, coupons, and loyalty points |

## Platform Guides

- [React Native Quick Start](react-native/quick-start.md)
- [iOS Quick Start](ios/quick-start.md)
- [Android Quick Start](android/quick-start.md)

## Requirements

| Platform | Minimum Version |
|----------|----------------|
| React Native | 0.72+ |
| iOS | 15.0+ |
| Android | API 24 (Android 7.0+) |

## Getting Your Credentials

Contact your Squad partner manager or visit [partners.withyoursquad.com](https://partners.withyoursquad.com) to get your `partnerId` and `apiKey`.
