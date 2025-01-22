# iOS Installation & Setup

## Requirements

- iOS 13.0 or later
- Xcode 13.0 or later
- Swift 5.3 or later

## Installation Methods

### Swift Package Manager (Recommended)

1. In Xcode, select **File** > **Add Package Dependencies...**
2. Enter the Squad SDK repository URL:

```
https://github.com/withyoursquad/squad-sports-ios.git
```

3. Select version settings:
   - Rules: Version - Up to Next Major
   - Version: 1.0.0 or later

### CocoaPods

1. Add the following to your Podfile:

```ruby
platform :ios, '13.0'
use_frameworks!

target 'YourApp' do
  pod 'SquadSDK', '~> 1.0.0'
end
```

2. Install the dependency:

```bash
pod install
```

## Project Setup

### 1. Required Permissions

Add the following to your `Info.plist`:

```xml
<!-- Microphone permission for voice calls -->
<key>NSMicrophoneUsageDescription</key>
<string>Squad needs access to your microphone for voice calls.</string>

<!-- Camera permission for profile photos (optional) -->
<key>NSCameraUsageDescription</key>
<string>Squad needs access to your camera for profile photos.</string>
```

### 2. Background Modes

Enable the following capabilities in Xcode:

1. Go to your target's **Signing & Capabilities**
2. Click **+ Capability**
3. Add **Background Modes**
4. Enable:
   - Voice over IP
   - Audio, AirPlay, and Picture in Picture
   - Background fetch

### 3. Network Configuration

Add App Transport Security settings to `Info.plist`:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <false/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>withsquad.com</key>
        <dict>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <false/>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <true/>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.3</string>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

### 4. Framework Dependencies

The Squad SDK requires the following frameworks:

- WebKit.framework
- AVFoundation.framework
- Network.framework

These are automatically linked when using package managers.

## Post-Installation Steps

### 1. Import the SDK

Add the import statement to your source files:

```swift
import SquadSDK
```

### 2. Initialize the SDK

In your `AppDelegate` or `SceneDelegate`:

```swift
func application(_ application: UIApplication,
                didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Initialize Squad SDK
    do {
        try SquadSDK.initialize(
            organizationId: "YOUR_ORG_ID",
            apiKey: "YOUR_API_KEY"
        )
    } catch {
        print("Squad SDK initialization failed: \(error)")
    }
    return true
}
```

### 3. Verify Installation

Add a test implementation:

```swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()

        // Verify SDK version
        print("Squad SDK Version: \(SquadSDK.version)")

        // Check initialization status
        if SquadSDK.shared.isInitialized {
            print("Squad SDK initialized successfully")
        }
    }
}
```

## Troubleshooting

### Common Issues

1. **Pod Installation Failed**

   - Run `pod repo update`
   - Delete Podfile.lock and run `pod install` again
   - Check minimum iOS version in Podfile

2. **SPM Installation Failed**

   - Check Xcode version compatibility
   - Clear derived data
   - Update to latest Xcode version

3. **Framework Not Found**
   - Clean build folder (Cmd + Shift + K)
   - Clean build cache
   - Re-run package manager installation

### Support

For installation issues:

- Check our [troubleshooting guide](../troubleshooting.md)
- Visit our [support center](https://support.squadforsports.com)
- Contact support at support@squadforsports.com

## Next Steps

- Configure the SDK using the [Configuration Guide](configuration.md)
- Set up user authentication with [User Auth Guide](user-auth.md)
- Integrate WebView using [WebView Management](webview.md)
