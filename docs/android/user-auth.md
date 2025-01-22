# User Authentication with Squad SDK for Android

To access Squad features within your app, users need to be authenticated. The Squad SDK for Android supports two methods of user authentication: email and access token.

## Email Authentication

If your app collects user email addresses, you can use them to authenticate users with the Squad SDK.

```kotlin
squadSDK.initializeUser(
    identifier = "user@example.com",
    authType = AuthType.EMAIL
) { result ->
    when (result) {
        is UserInitResult.Success -> {
            // User initialized successfully
            val user = result.user
            Log.d("SquadSDK", "User authenticated: ${user.id}")
        }
        is UserInitResult.Error -> {
            // Handle authentication error
            Log.e("SquadSDK", "Authentication failed: ${result.error.message}")
        }
    }
}
```

## Access Token Authentication

If your app already has an authentication system in place, you can use access tokens to authenticate users with the Squad SDK.

```kotlin
squadSDK.initializeUser(
    identifier = "YOUR_ACCESS_TOKEN",
    authType = AuthType.TOKEN
) { result ->
    when (result) {
        is UserInitResult.Success -> {
            // User initialized successfully
            val user = result.user
            Log.d("SquadSDK", "User authenticated with token: ${user.id}")
        }
        is UserInitResult.Error -> {
            // Handle authentication error
            Log.e("SquadSDK", "Token authentication failed: ${result.error.message}")
        }
    }
}
```

## User Management

The Squad SDK provides methods to manage user sessions and authentication states.

### Checking Authentication State

```kotlin
if (squadSDK.isUserAuthenticated()) {
    // User is authenticated
    val currentUser = squadSDK.getCurrentUser()
    Log.d("SquadSDK", "Current user: ${currentUser.id}")
} else {
    // User needs to authenticate
    // Proceed with authentication flow
}
```

### Logging Out

To log out the current user:

```kotlin
squadSDK.logoutUser { result ->
    when (result) {
        is LogoutResult.Success -> {
            Log.d("SquadSDK", "User logged out successfully")
            // Proceed to login screen
        }
        is LogoutResult.Error -> {
            Log.e("SquadSDK", "Logout failed: ${result.error.message}")
            // Handle logout error
        }
    }
}
```

## Error Handling

The SDK provides detailed error information through the `SquadError` class:

```kotlin
sealed class SquadError {
    data class InvalidCredentials(val message: String) : SquadError()
    data class NetworkError(val message: String) : SquadError()
    data class SessionExpired(val message: String) : SquadError()
    data class Unknown(val message: String) : SquadError()
}

// Example error handling
squadSDK.initializeUser(email = "user@example.com", authType = AuthType.EMAIL) { result ->
    when (result) {
        is UserInitResult.Success -> {
            // Handle success
        }
        is UserInitResult.Error -> {
            when (val error = result.error) {
                is SquadError.InvalidCredentials -> {
                    showError("Invalid email address")
                }
                is SquadError.NetworkError -> {
                    showError("Network error: ${error.message}")
                }
                is SquadError.SessionExpired -> {
                    // Handle session expiration
                    refreshAuthentication()
                }
                is SquadError.Unknown -> {
                    showError("An unexpected error occurred")
                }
            }
        }
    }
}
```

## Best Practices

1. **Error Handling**

   - Always handle authentication errors gracefully
   - Provide clear feedback to users
   - Implement proper retry mechanisms

2. **Token Management**

   - Store tokens securely using Android KeyStore
   - Implement token refresh mechanism
   - Handle token expiration properly

3. **Session Management**
   - Monitor authentication state changes
   - Handle background/foreground transitions
   - Implement proper logout cleanup

## Security Considerations

1. **Secure Storage**

```kotlin
class SecureTokenManager(private val context: Context) {
    private val keyStore = KeyStore.getInstance("AndroidKeyStore")

    fun storeToken(token: String) {
        // Encrypt and store token securely
    }

    fun retrieveToken(): String? {
        // Decrypt and retrieve token
    }
}
```

2. **Token Encryption**

```kotlin
@RequiresApi(Build.VERSION_CODES.M)
private fun encryptToken(token: String): ByteArray {
    // Implement encryption using Android Keystore
}
```

## Next Steps

- Present the [Squad WebView](../webview-integration.md)
- Handle [WebView Events](webview-events.md)
- Explore [Squad Line](../squad-line.md) capabilities
- [Troubleshooting](troubleshooting.md)

## Support

If you have any questions or need assistance:

- Visit our [Support Center](https://support.squadforsports.com)
- Contact us at support@squadforsports.com
