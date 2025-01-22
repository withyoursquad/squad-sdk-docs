# Troubleshooting Squad SDK for Android

This guide helps you diagnose and resolve common issues when integrating and using the Squad SDK in your Android application.

## Common Issues and Solutions

### SDK Initialization Issues

1. **SDK Not Initialized Error**

```kotlin
java.lang.IllegalStateException: Squad SDK is not initialized
```

**Solution:**

- Ensure SDK is initialized in Application class
- Check initialization order
- Verify credentials

```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        try {
            SquadSDK.Builder(this)
                .setOrganizationId("YOUR_ORG_ID")
                .setApiKey("YOUR_API_KEY")
                .build()
                .initialize()
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "Initialization failed", e)
        }
    }
}
```

2. **Invalid Credentials Error**

```kotlin
SquadSDKException: Invalid API key or Organization ID
```

**Solution:**

- Verify credentials in developer dashboard
- Check for environment mismatch
- Ensure proper credential format

### Authentication Issues

1. **User Authentication Failed**

```kotlin
UserInitResult.Error: Authentication failed
```

**Solution:**

- Check network connection
- Verify user credentials
- Ensure proper token format

```kotlin
// Implement retry logic
private fun retryAuthentication(maxAttempts: Int = 3) {
    var attempts = 0

    fun authenticate() {
        if (attempts >= maxAttempts) return

        squadSDK.initializeUser(token = userToken) { result ->
            when (result) {
                is UserInitResult.Success -> {
                    // Authentication successful
                }
                is UserInitResult.Error -> {
                    attempts++
                    Handler(Looper.getMainLooper()).postDelayed({
                        authenticate()
                    }, 1000 * attempts)
                }
            }
        }
    }

    authenticate()
}
```

2. **Token Expiration**

```kotlin
SquadError.SessionExpired: Token has expired
```

**Solution:**

- Implement token refresh
- Handle expiration gracefully
- Clear invalid tokens

### WebView Issues

1. **WebView Loading Failed**

```kotlin
WebViewError: Failed to load Squad experience
```

**Solution:**

- Check internet connection
- Verify WebView version
- Clear WebView cache

```kotlin
private fun handleWebViewError(error: WebViewError) {
    // Clear WebView cache
    squadSDK.clearWebViewCache()

    // Reset WebView state
    squadSDK.resetWebView()

    // Retry loading
    squadSDK.reloadWebView()
}
```

2. **WebView Performance Issues**

**Solution:**

- Optimize memory usage
- Implement efficient lifecycle management
- Clear resources properly

```kotlin
class SquadActivity : AppCompatActivity() {
    override fun onLowMemory() {
        super.onLowMemory()
        squadSDK.clearWebViewCache()
    }

    override fun onDestroy() {
        super.onDestroy()
        squadSDK.cleanup()
    }
}
```

### Voice Calling Issues

1. **Microphone Permission Denied**

```kotlin
SquadError.PermissionDenied: RECORD_AUDIO permission not granted
```

**Solution:**
