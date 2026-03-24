# Requirements & Permissions

## SDK Size

| Platform | Total Size | Notes |
|----------|-----------|-------|
| React Native | ~1.4 MB | Core + RN bridge. Twilio via optional peer dep. |
| iOS | ~13.8 MB | Includes SwiftProtobuf + TwilioVoice |
| Android | ~9.8 MB | Includes OkHttp + Protobuf + Twilio Voice |

Sizes are incremental app size increase (compressed, release build). The Squad experience includes all features — community feed, messaging, Squad Line voice calls, polls, events, and wallet.

!!! note "All features are included"
    The full Squad experience ships with all features enabled. Feature flags exist for cases where a specific partner agreement excludes a feature — they are not intended for ad-hoc toggling by integrators.

## Required Permissions

The SDK requires the following device permissions. All permissions must be declared — the full Squad experience uses microphone (voice calls, audio messages, freestyles), camera (QR invite scanning), and push notifications (real-time alerts).

### iOS (`Info.plist`)

| Permission | Key | Required | Feature |
|------------|-----|----------|---------|
| Microphone | `NSMicrophoneUsageDescription` | **Yes** | Squad Line, audio freestyles, audio messages |
| Camera | `NSCameraUsageDescription` | **Yes** | QR code invite scanning |
| Push Notifications | via `UNUserNotificationCenter` | **Yes** | Message, call, and poll notifications |

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Squad needs microphone access for voice calls and audio messages.</string>
<key>NSCameraUsageDescription</key>
<string>Squad uses the camera to scan invite QR codes.</string>
```

### Android (`AndroidManifest.xml`)

| Permission | Manifest Entry | Required | Feature |
|------------|---------------|----------|---------|
| Internet | `android.permission.INTERNET` | **Yes** | All API communication |
| Microphone | `android.permission.RECORD_AUDIO` | **Yes** | Squad Line, audio messages, freestyles |
| Camera | `android.permission.CAMERA` | **Yes** | QR code invite scanning |
| Vibrate | `android.permission.VIBRATE` | **Yes** | Haptic feedback on calls |
| Post Notifications | `android.permission.POST_NOTIFICATIONS` | **Yes** (Android 13+) | Real-time notifications |

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.VIBRATE" />
<!-- Android 13+ -->
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

### React Native

Permissions are declared in the native layer. Follow the iOS and Android guidance above.

For Expo managed projects, add to `app.json`:

```json
{
  "expo": {
    "ios": {
      "infoPlist": {
        "NSMicrophoneUsageDescription": "Squad needs microphone access for voice calls and audio messages.",
        "NSCameraUsageDescription": "Squad uses the camera to scan invite QR codes."
      }
    },
    "android": {
      "permissions": [
        "RECORD_AUDIO",
        "CAMERA",
        "VIBRATE",
        "POST_NOTIFICATIONS"
      ]
    }
  }
}
```

## Required Dependencies

### React Native

All peer dependencies are required for the full Squad experience:

```bash
# Core SDK
yarn add @squad-sports/core @squad-sports/react-native

# Required peer dependencies
yarn add @react-navigation/native @react-navigation/native-stack \
  react-native-screens react-native-safe-area-context \
  react-native-gesture-handler react-native-reanimated \
  @react-native-async-storage/async-storage \
  @gorhom/bottom-sheet recoil \
  expo-av expo-image expo-secure-store

# Squad Line (voice calls)
yarn add @twilio/voice-react-native-sdk
```

### iOS

Add via Swift Package Manager:

```swift
dependencies: [
    .package(url: "https://github.com/withyoursquad/squad-sports-sdk.git", branch: "main"),
]
```

This includes SwiftProtobuf and TwilioVoice as transitive dependencies.

### Android

```kotlin
dependencies {
    implementation("com.squadforsports:squad-sports-sdk:1.3.1")
}
```

This includes OkHttp, Protobuf, Twilio Voice, and AndroidX Security as transitive dependencies.

## Privacy & Data Collection

### Data the SDK collects

| Data Type | Collected | Purpose | Shared with 3rd parties |
|-----------|-----------|---------|------------------------|
| Email / Phone | Yes | Account creation, verification | No |
| Display Name | Yes | User profile | No |
| Device Info | Yes | Push notifications, analytics | No |
| Usage Analytics | Yes | SDK lifecycle events, screen views | Only to partner analytics endpoint |
| Audio Recordings | Yes (user-initiated) | Freestyles, voice messages, Squad Line calls | No (stored on Squad servers) |
| IP Address | Yes (server-side) | Rate limiting, security | No |

### iOS Privacy Manifest

The SDK ships with a `PrivacyInfo.xcprivacy` file declaring:

- **NSPrivacyTracking**: `false`
- **NSPrivacyCollectedDataTypes**: email, phone, name (app functionality), audio (user-initiated), device ID (analytics)
- **NSPrivacyAccessedAPITypes**: UserDefaults (app preferences)

### Google Play Data Safety

For your Play Store listing, declare under "Data collected":

- **Personal info**: Name, email, phone (required for account)
- **Audio**: Voice recordings (user-initiated only)
- **App activity**: Screen views, feature usage
- **Device info**: Push notification tokens

All data is encrypted in transit (TLS 1.2+) and sensitive data is encrypted at rest.

### Data Deletion

Users can request account deletion from Settings within the Squad experience. Partners can also request user data deletion via the partner API:

```
DELETE /v2/partners/:partnerId/users/:userId
```

Requires your API key. Verifies the user belongs to your community. Queues a deletion request that removes all user data within 30 days, in compliance with GDPR and CCPA requirements.

## Third-Party Dependencies

### iOS
| Dependency | Version | Purpose | License |
|------------|---------|---------|---------|
| SwiftProtobuf | 1.25+ | API serialization | Apache 2.0 |
| TwilioVoice | 6.10+ | Voice calls (Squad Line) | Twilio ToS |

### Android
| Dependency | Version | Purpose | License |
|------------|---------|---------|---------|
| OkHttp | 4.12+ | HTTP networking | Apache 2.0 |
| Protobuf Kotlin | 3.25+ | API serialization | BSD 3-Clause |
| Twilio Voice | 6.5+ | Voice calls (Squad Line) | Twilio ToS |
| AndroidX Security | 1.1+ | Encrypted token storage | Apache 2.0 |

### React Native
| Dependency | Version | Purpose | License |
|------------|---------|---------|---------|
| axios | 1.8+ | HTTP networking | MIT |
| expo-secure-store | 14+ | Encrypted token storage | MIT |
| expo-av | 15+ | Audio playback/recording | MIT |
| react-native-reanimated | 3+ | UI animations | MIT |
| @twilio/voice-react-native-sdk | 1.0+ | Voice calls (Squad Line) | Twilio ToS |
