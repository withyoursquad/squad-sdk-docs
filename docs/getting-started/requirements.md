# Requirements & Permissions

## SDK Size

| Platform | Size Impact | Notes |
|----------|-------------|-------|
| React Native | ~2.8 MB | Includes core + RN bridge. Tree-shakes unused features. |
| iOS | ~3.2 MB | Universal binary (arm64). Includes SwiftProtobuf + TwilioVoice. |
| Android | ~2.5 MB | AAR with ProGuard rules. Includes OkHttp + Protobuf. |

Sizes measured as incremental app size increase (compressed, release build).

## Required Permissions

The SDK requests the following device permissions. Declare these in your app's manifest / Info.plist.

### iOS (`Info.plist`)

| Permission | Key | Required | Feature |
|------------|-----|----------|---------|
| Microphone | `NSMicrophoneUsageDescription` | Yes (if Squad Line or audio messages enabled) | Voice calls, audio freestyles, audio messages |
| Camera | `NSCameraUsageDescription` | No (optional) | QR code invite scanning |
| Push Notifications | via `UNUserNotificationCenter` | Recommended | Real-time message and call notifications |

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Squad needs microphone access for voice calls and audio messages.</string>
<key>NSCameraUsageDescription</key>
<string>Squad uses the camera to scan invite QR codes.</string>
```

### Android (`AndroidManifest.xml`)

| Permission | Manifest Entry | Required | Feature |
|------------|---------------|----------|---------|
| Internet | `android.permission.INTERNET` | Yes | All API communication |
| Microphone | `android.permission.RECORD_AUDIO` | Yes (if Squad Line enabled) | Voice calls, audio messages |
| Camera | `android.permission.CAMERA` | No (optional) | QR code invite scanning |
| Vibrate | `android.permission.VIBRATE` | No | Haptic feedback on calls |

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

### React Native

Permissions are declared in the native layer. Follow the iOS and Android guidance above for your Expo or bare RN project.

For Expo managed projects, add to `app.json`:

```json
{
  "expo": {
    "ios": {
      "infoPlist": {
        "NSMicrophoneUsageDescription": "Squad needs microphone access for voice calls and audio messages."
      }
    },
    "android": {
      "permissions": ["RECORD_AUDIO"]
    }
  }
}
```

## Privacy & Data Collection

### Data the SDK collects

| Data Type | Collected | Purpose | Shared with 3rd parties |
|-----------|-----------|---------|------------------------|
| Email / Phone | Yes (auth) | Account creation, verification | No |
| Display Name | Yes | User profile | No |
| Device Info | Yes | Push notifications, analytics | No |
| Usage Analytics | Yes | SDK lifecycle events, screen views | Only to partner analytics endpoint |
| Audio Recordings | Yes (user-initiated) | Freestyles, voice messages | No (stored on Squad servers) |
| IP Address | Yes (server-side) | Rate limiting, security | No |

### iOS Privacy Manifest

The SDK ships with a `PrivacyInfo.xcprivacy` file declaring:

- **NSPrivacyTracking**: `false`
- **NSPrivacyCollectedDataTypes**: email, phone, name (for app functionality)
- **NSPrivacyAccessedAPITypes**: UserDefaults (for non-sensitive preferences)

### Google Play Data Safety

For your Play Store listing, declare under "Data collected":

- **Personal info**: Name, email, phone (required for account)
- **Audio**: Voice recordings (user-initiated only)
- **App activity**: Screen views, feature usage

All data is encrypted in transit (TLS 1.2+) and sensitive data is encrypted at rest.

### Data Deletion

Users can request account deletion from Settings within the Squad experience. Partners can also request user data deletion via the API:

```
POST /v2/users/rpc/deleteUser
```

This queues a deletion request that removes all user data within 30 days, in compliance with GDPR and CCPA requirements.

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
