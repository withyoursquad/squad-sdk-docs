# Squad SDK Initialization for iOS

The Squad SDK initialization process consists of three key steps:

1. SDK Initialization
2. User Initialization
3. WebView Initialization

## SDK Initialization

### Obtaining Credentials

1. Login to your developer account at [https://developer.withyoursquad.com](https://developer.withyoursquad.com)
2. Navigate to Organization Settings
3. Copy your Organization ID and API Key

### Initialization Code

```swift
import SquadSDK

class AppDelegate: UIResponder, UIApplicationDelegate {
    var squadSDK: SquadSDK?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        do {
            // SDK Initialization
            squadSDK = SquadSDK(
                orgId: "YOUR_ORGANIZATION_ID",
                apiKey: "YOUR_API_KEY"
            )

            try squadSDK?.initializeSDK()
        } catch {
            print("Squad SDK Initialization Error: \(error)")
        }

        return true
    }
}
```

## User Initialization

### Authentication Methods

The Squad SDK supports two primary user initialization methods:

1. Email Authentication

```swift
func initializeUser(email: String) {
    do {
        try squadSDK?.initializeUser(
            identifier: email,
            authType: .email
        )
    } catch {
        print("User Initialization Error: \(error)")
    }
}
```

2. Token Authentication

```swift
func initializeUser(token: String) {
    do {
        try squadSDK?.initializeUser(
            identifier: token,
            authType: .token
        )
    } catch {
        print("User Initialization Error: \(error)")
    }
}
```

## WebView Initialization

### Presenting the Squad Experience

```swift
class ViewController: UIViewController {
    func openSquadExperience() {
        do {
            // Ensure SDK and User are initialized first
            let webViewController = try squadSDK?.presentWebView(
                configuration: WebViewConfiguration(
                    features: [
                        .freestyles,
                        .polls,
                        .squadManagement,
                        .voiceCalling,
                        .voiceMessages
                    ]
                )
            )

            // Present the web view
            present(webViewController, animated: true)
        } catch {
            print("WebView Initialization Error: \(error)")
        }
    }
}
```

## Complete Initialization Example

```swift
class MyAppViewController: UIViewController {
    func setupSquadSDK() {
        // 1. SDK Initialization
        squadSDK = SquadSDK(
            orgId: "your_org_id",
            apiKey: "your_api_key"
        )

        // 2. User Initialization
        try squadSDK?.initializeUser(
            identifier: currentUser.email,
            authType: .email
        )

        // 3. WebView Initialization
        let squadWebView = try squadSDK?.presentWebView(
            configuration: WebViewConfiguration(
                features: [
                    .freestyles,
                    .polls,
                    .squadManagement,
                    .voiceCalling,
                    .voiceMessages
                ]
            )
        )

        // Add webView to your view hierarchy
        view.addSubview(squadWebView)
    }
}
```

## Error Handling

```swift
enum SquadSDKError: Error {
    case sdkInitializationFailed
    case userAuthenticationFailed
    case webViewPresentationFailed
}
```

## Best Practices

1. Initialize the SDK as early as possible in your app lifecycle
2. Securely manage user authentication tokens
3. Handle potential initialization errors gracefully
4. Ensure proper user authentication before presenting WebView
5. Keep the SDK updated to the latest version

## Troubleshooting

- Verify network connectivity
- Confirm API key and organization ID
- Ensure user is properly authenticated
- Check SDK initialization sequence
- Review error logs
- Contact Squad developer support if issues persist

## Support

- Documentation: [Squad Developer Docs](https://developer.withyoursquad.com)
- Support Email: support@withyoursquad.com

---

**Note:** Always refer to the latest documentation for the most up-to-date SDK initialization instructions.
