# Getting Started with Squad SDK for iOS

This guide will help you integrate the Squad SDK into your iOS app, enabling social features and voice calling capabilities.

## Prerequisites

Before you begin, ensure you have:

- A Squad developer account and credentials
- Xcode 13.0 or later
- iOS 13.0 or later
- Swift 5.3 or later

## Installation Methods

### Swift Package Manager (Recommended)

1. In Xcode, select **File** > **Add Package Dependencies...**
2. Enter the repository URL:

```
https://github.com/withyoursquad/squad-sports-ios.git
```

3. Select version settings:
   - Rules: Version - Up to Next Major
   - Version: 1.0.0 or later

### CocoaPods

1. Add to your Podfile:

```ruby
pod 'SquadSDK', '~> 1.0.0'
```

2. Install the dependency:

```bash
pod install
```

For detailed installation options and troubleshooting, see our [Installation Guide](installation.md).

## Quick Start Guide

### 1. SDK Initialization

Import and initialize the SDK:

```swift
import SquadSDK

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    do {
        let config = SquadConfig(
            organizationId: "YOUR_ORG_ID",
            apiKey: "YOUR_API_KEY",
            environment: .production
        )
        try SquadSDK.initialize(with: config)
        print("Squad SDK initialized successfully")
    } catch {
        print("Failed to initialize Squad SDK: \(error)")
    }
    return true
}
```

For advanced configuration options, see our [Configuration Guide](configuration.md).

### 2. User Authentication

Authenticate users using email or token:

```swift
try squadSDK.authenticateUser(
    identifier: "user@example.com",
    authType: .email
) { result in
    switch result {
    case .success(let user):
        print("User authenticated: \(user.id)")
    case .failure(let error):
        print("Authentication failed: \(error)")
    }
}
```

Learn more in our [User Authentication Guide](user-auth.md).

### 3. WebView Integration

Present the Squad experience:

```swift
class ViewController: UIViewController {
    func showSquadExperience() {
        do {
            let webView = try squadSDK.presentWebView(
                configuration: WebViewConfig(
                    features: [.voiceCalls, .freestyles, .polls]
                )
            )
            view.addSubview(webView)
            setupWebViewConstraints(webView)
        } catch {
            print("Failed to present Squad WebView: \(error)")
        }
    }
}
```

For comprehensive WebView management, see our [WebView Management Guide](webview.md).

## Integration Guides

- [Installation & Setup](installation.md) - Detailed installation steps
- [Configuration Guide](configuration.md) - Advanced SDK configuration
- [WebView Management](webview.md) - WebView integration and management
- [User Authentication](user-auth.md) - Authentication implementation
- [WebView Events](webview-events.md) - Event handling guide

## Troubleshooting

For common issues and solutions, see our [Troubleshooting Guide](troubleshooting.md).

## Additional Resources

- [Sample Projects](https://github.com/withyoursquad/ios-samples)

## Support

Need help? Our support team is ready to assist:

- Support Center: [https://support.squadforsports.com](https://support.squadforsports.com)
- Email: support@squadforsports.com
- Documentation: [https://docs.squadforsports.com](https://docs.squadforsports.com)
