# User Initialization

After initializing the Squad SDK with your organization credentials, the next step is to authenticate users to access the Squad features within your app. The SDK supports two methods of user authentication: email and access token.

## Email Authentication

To authenticate a user using their email address:

### iOS

```swift
do {
    try squadSDK.initializeUser(email: "user@example.com")
    print("User initialized successfully")
} catch {
    print("Failed to initialize user: \(error)")
}
```

### Android

```kotlin
try {
    squadSDK.initializeUser("user@example.com")
    println("User initialized successfully")
} catch (e: Exception) {
    println("Failed to initialize user: $e")
}
```

### React Native

```javascript
try {
  await squadSDK.initializeUser("user@example.com");
  console.log("User initialized successfully");
} catch (error) {
  console.error("Failed to initialize user:", error);
}
```

## Access Token Authentication

If your app already has an authentication system in place, you can use access tokens to authenticate users with the Squad SDK. To authenticate a user using an access token:

### iOS

```swift
do {
    try squadSDK.initializeUser(token: "YOUR_ACCESS_TOKEN")
    print("User initialized successfully")
} catch {
    print("Failed to initialize user: \(error)")
}
```

### Android

```kotlin
try {
    squadSDK.initializeUser("YOUR_ACCESS_TOKEN")
    println("User initialized successfully")
} catch (e: Exception) {
    println("Failed to initialize user: $e")
}
```

### React Native

```javascript
try {
  await squadSDK.initializeUser("YOUR_ACCESS_TOKEN");
  console.log("User initialized successfully");
} catch (error) {
  console.error("Failed to initialize user:", error);
}
```

Make sure to replace `"YOUR_ACCESS_TOKEN"` with the actual access token obtained from your app's authentication system.

## User Management

The Squad SDK handles user sessions automatically, so you don't need to manage session state yourself. Once a user is initialized, they can access the Squad features seamlessly.

If you need to logout a user or switch to a different user, simply call the `logoutUser` method:

### iOS

```swift
squadSDK.logoutUser()
```

### Android

```kotlin
squadSDK.logoutUser()
```

### React Native

```javascript
squadSDK.logoutUser();
```

After logging out a user, you can initialize a new user by calling the `initializeUser` method again with the appropriate email or access token.

For more information on user authentication and management, refer to the platform-specific guides:

- [iOS User Authentication Guide](ios/user-auth.md)
- [Android User Authentication Guide](android/user-auth.md)
- [React Native User Authentication Guide](react-native/user-auth.md)
