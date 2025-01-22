# SDK Initialization

Before you can start using the Squad SDK in your app, you need to initialize it with your organization credentials. This process involves obtaining an organization ID and API key from your Squad developer account and passing them to the SDK during initialization.

## Obtaining Credentials

To get your organization ID and API key:

1. Sign up for a Squad developer account at [https://developer.squadforsports.com](https://developer.squadforsports.com)
2. Once logged in, navigate to the "Organization Settings" page
3. Copy your "Organization ID" and "API Key" from the settings page

Make sure to keep your API key secure and never expose it in client-side code or public repositories.

## Initializing the SDK

The process of initializing the Squad SDK varies slightly depending on the platform you're using. Here are the basic steps for each platform:

### iOS

```swift
import SquadSDK

let organizationId = "YOUR_ORGANIZATION_ID"
let apiKey = "YOUR_API_KEY"

let squadSDK = SquadSDK(organizationId: organizationId, apiKey: apiKey)

do {
    try squadSDK.initialize()
    print("Squad SDK initialized successfully")
} catch {
    print("Failed to initialize Squad SDK: \(error)")
}
```

### Android

```kotlin
import com.withyoursquad.sdk.SquadSDK

val organizationId = "YOUR_ORGANIZATION_ID"
val apiKey = "YOUR_API_KEY"

val squadSDK = SquadSDK(context, organizationId, apiKey)

try {
    squadSDK.initialize()
    println("Squad SDK initialized successfully")
} catch (e: Exception) {
    println("Failed to initialize Squad SDK: $e")
}
```

### React Native

```javascript
import { SquadSDK } from "@withyoursquad/react-native-sdk";

const organizationId = "YOUR_ORGANIZATION_ID";
const apiKey = "YOUR_API_KEY";

const squadSDK = new SquadSDK(organizationId, apiKey);

try {
  await squadSDK.initialize();
  console.log("Squad SDK initialized successfully");
} catch (error) {
  console.error("Failed to initialize Squad SDK:", error);
}
```

After initializing the SDK, you can proceed with authenticating users and presenting the Squad WebView to display the social features in your app.

For more detailed information on SDK initialization, refer to the platform-specific guides:

- [iOS SDK Initialization Guide](ios/sdk-init.md)
- [Android SDK Initialization Guide](android/sdk-init.md)
- [React Native SDK Initialization Guide](react-native/sdk-init.md)
