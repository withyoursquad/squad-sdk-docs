# Installation & Setup

This guide provides comprehensive installation and setup instructions for the Squad SDK across supported platforms.

## Platform Requirements

### iOS Requirements

- iOS 13.0 or later
- Xcode 13.0 or later
- Swift 5.3 or later
- Valid Apple Developer account
- Minimum deployment target iOS 13.0

### Android Requirements

- Android API level 21 (Android 5.0) or higher
- Android Studio Arctic Fox (2020.3.1) or newer
- Kotlin 1.5.0 or later
- Gradle 7.0 or higher
- Java 8 or higher

## Prerequisites

Before starting the installation:

1. **Developer Account Setup**

   - Create a Squad developer account at [developer.withsquad.com](https://developer.squadforsports.com)
   - Generate your organization ID and API key
   - Note your credentials for SDK initialization

2. **Project Preparation**
   - Ensure your project meets minimum platform requirements
   - Verify you have necessary development environment setup
   - Check network connectivity for package downloads

## Platform-Specific Installation

### iOS Installation

#### Swift Package Manager (Recommended)

```swift
// In Xcode:
// File > Add Package Dependencies
// Enter: https://github.com/withyoursquad/squad-sports-ios.git
```

#### CocoaPods

```ruby
pod 'SquadSDK', '~> 1.0.0'
```

[Detailed iOS Installation Guide →](ios/installation.md)

### Android Installation

#### Gradle Setup

```gradle
// Project build.gradle
repositories {
    mavenCentral()
}

// App build.gradle
dependencies {
    implementation 'com.withyoursquad.sdk:squadline:1.0.0'
}
```

[Detailed Android Installation Guide →](android/installation.md)

## Basic Integration Steps

1. **SDK Initialization**

```swift
// iOS
import SquadSDK

SquadSDK.initialize(
    organizationId: "YOUR_ORG_ID",
    apiKey: "YOUR_API_KEY"
)
```

```kotlin
// Android
import com.withyoursquad.sdk.SquadSDK

SquadSDK.Builder(context)
    .setOrganizationId("YOUR_ORG_ID")
    .setApiKey("YOUR_API_KEY")
    .build()
    .initialize()
```

2. **User Authentication**

```swift
// iOS
try squadSDK.authenticateUser(
    identifier: "user@example.com",
    authType: .email
)
```

```kotlin
// Android
squadSDK.authenticateUser(
    identifier = "user@example.com",
    authType = AuthType.EMAIL
)
```

3. **WebView Integration**

```swift
// iOS
let webView = try squadSDK.presentWebView()
view.addSubview(webView)
```

```kotlin
// Android
val webView = squadSDK.presentWebView(this)
setContentView(webView)
```

## Required Permissions

### iOS

Add to Info.plist:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Squad needs microphone access for voice calls</string>
```

### Android

Add to AndroidManifest.xml:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## Verification Steps

1. **Check SDK Installation**

   - Verify build success
   - Check for any dependency conflicts
   - Ensure proper permissions setup

2. **Verify Initialization**

   - Test SDK initialization
   - Validate API credentials
   - Check debug logs

3. **Test Basic Features**
   - Verify WebView loading
   - Test user authentication
   - Check voice permissions

## Common Issues

### iOS Common Issues

1. **CocoaPods Integration**

   - Run `pod repo update`
   - Check Podfile syntax
   - Verify minimum iOS version

2. **SPM Integration**
   - Clear derived data
   - Check package resolution
   - Verify Xcode version

### Android Common Issues

1. **Gradle Sync**

   - Check Gradle version
   - Verify repository access
   - Update Gradle plugin

2. **ProGuard Configuration**
   - Add required rules
   - Check mapping file
   - Verify release build

## Next Steps

1. **Configuration**

   - [iOS Configuration Guide](ios/configuration.md)
   - [Android Configuration Guide](android/configuration.md)

2. **WebView Setup**

   - [iOS WebView Management](ios/webview.md)
   - [Android WebView Management](android/webview.md)

3. **Authentication**
   - [User Authentication Guide](user-auth.md)

## Additional Resources

- [Sample Projects](resources/samples.md)
- [Troubleshooting Guide](troubleshooting.md)

## Support

If you encounter any issues during installation:

- Review our [Troubleshooting Guide](troubleshooting.md)
- Visit [Support Center](https://support.squadforsports.com)
- Contact support at support@squadforsports.com
- Check [Documentation](https://docs.squadforsports.com)

Remember to reference platform-specific guides for detailed setup instructions and advanced configurations.
