# User Authentication with Squad SDK for iOS

To access Squad features within your app, users need to be authenticated. The Squad SDK for iOS supports two methods of user authentication: email and access token.

## Email Authentication

If your app collects user email addresses, you can use them to authenticate users with the Squad SDK.

To authenticate a user using their email address:

```swift
do {
    try squadSDK.initializeUser(email: "user@example.com")
    print("User initialized successfully")
} catch {
    print("Failed to initialize user: \(error)")
}
```

Replace `"user@example.com"` with the actual email address of the user you want to authenticate.

## Access Token Authentication

If your app already has an authentication system in place, you can use access tokens to authenticate users with the Squad SDK.

To authenticate a user using an access token:

```swift
do {
    try squadSDK.initializeUser(token: "YOUR_ACCESS_TOKEN")
    print("User initialized successfully")
} catch {
    print("Failed to initialize user: \(error)")
}
```

Replace `"YOUR_ACCESS_TOKEN"` with the actual access token obtained from your app's authentication system.

## User Management

The Squad SDK handles user sessions automatically, so you don't need to manage session state yourself. Once a user is initialized, they can access the Squad features seamlessly.

If you need to log out a user or switch to a different user, simply call the `logoutUser` method:

```swift
squadSDK.logoutUser()
```

After logging out a user, you can initialize a new user by calling the `initializeUser` method again with the appropriate email or access token.

## Error Handling

When authenticating users, be sure to handle any errors that may occur. The SDK throws errors in case of invalid credentials, network issues, or other authentication failures.

You can catch and handle these errors using a `do-catch` block:

```swift
do {
    try squadSDK.initializeUser(email: "user@example.com")
    print("User initialized successfully")
} catch let error as SquadSDKError {
    switch error {
    case .invalidCredentials:
        print("Invalid email or access token")
    case .networkError:
        print("Network error occurred")
    case .unknownError:
        print("Unknown error occurred")
    }
} catch {
    print("Unexpected error: \(error)")
}
```

## Next Steps

- Present the [Squad WebView](webview-integration.md) to display social features
- Handle [WebView Events](webview-events.md)
- Explore [Squad Line](squad-line.md) capabilities

If you have any questions or need further assistance, please visit our [Support Center](https://support.withyoursquad.com) or contact us at support@withyoursquad.com.
