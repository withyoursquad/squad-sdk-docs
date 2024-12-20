# Getting Started with Squad SDK for iOS

This guide will walk you through the process of integrating the Squad SDK into your iOS app, allowing you to quickly add social features and voice calling capabilities to your app.

## Prerequisites

Before you begin, ensure that you have the following:

- A Squad developer account
- Xcode 13.0 or later
- iOS 13.0 or later
- Swift 5.3 or later

## Installation

You can install the Squad SDK using either Swift Package Manager or CocoaPods.

### Swift Package Manager

1. In Xcode, select "File" > "Swift Packages" > "Add Package Dependency"
2. Enter the following URL: `https://github.com/withyoursquad/squad-sports-ios.git`
3. Choose the latest version or a specific version of the SDK
4. Click "Next" and select the target for your app
5. Click "Finish"

### CocoaPods

1. Add the following line to your Podfile:

```ruby
pod 'SquadSDK', '~> 1.0.0'
```

2. Run `pod install` to install the SDK.

## Initialization

To start using the Squad SDK, you need to initialize it with your organization credentials.

1. Import the Squad SDK in your app delegate:

```swift
import SquadSDK
```

2. Initialize the SDK with your organization ID and API key:

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    let squadSDK = SquadSDK(organizationId: "YOUR_ORGANIZATION_ID", apiKey: "YOUR_API_KEY")
    do {
        try squadSDK.initialize()
        print("Squad SDK initialized successfully")
    } catch {
        print("Failed to initialize Squad SDK: \(error)")
    }
    return true
}
```

Replace `"YOUR_ORGANIZATION_ID"` and `"YOUR_API_KEY"` with your actual credentials.

## User Authentication

To access Squad features, users need to be authenticated. You can authenticate users using either email or access tokens.

For detailed information on user authentication, refer to the [User Authentication Guide](user-auth.md).

## Presenting the Squad UI

Once the SDK is initialized and the user is authenticated, you can present the Squad UI by calling the `openSquadWebView` method:

```swift
do {
    try squadSDK.openSquadWebView()
    print("Squad WebView presented successfully")
} catch {
    print("Failed to present Squad WebView: \(error)")
}
```

The Squad UI will be presented modally, providing access to social features, voice calling, and more.

## Next Steps

- Learn more about [User Authentication](user-auth.md)
- Handle [WebView Events](webview-events.md)
- Explore [Squad Line](../squad-line.md) capabilities

If you have any questions or need further assistance, please visit our [Support Center](https://support.withyoursquad.com) or contact us at support@withyoursquad.com.
