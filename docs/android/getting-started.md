# Getting Started with Squad SDK for Android

This guide will help you integrate the Squad SDK into your Android app, enabling social features and voice calling capabilities.

## Prerequisites

Before you begin, ensure you have:

- A Squad developer account and credentials
- Android Studio Arctic Fox (2020.3.1) or newer
- Minimum SDK level 21 (Android 5.0)
- Kotlin 1.5.0 or later
- Gradle 7.0 or higher

## Installation

### Gradle Setup

1. Add Maven Central repository in `settings.gradle`:

```gradle
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
}
```

2. Add SDK dependency in app's `build.gradle`:

```gradle
dependencies {
    implementation 'com.withyoursquad.sdk:squadsdk:1.0.0'
}
```

For detailed installation options and troubleshooting, see our [Installation Guide](installation.md).

## Quick Start Guide

### 1. SDK Initialization

Initialize the SDK in your Application class:

```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        val config = SquadConfig.Builder()
            .setOrganizationId("YOUR_ORG_ID")
            .setApiKey("YOUR_API_KEY")
            .setEnvironment(Environment.PRODUCTION)
            .build()

        try {
            SquadSDK.initialize(this, config)
            Log.d("SquadSDK", "Initialized successfully")
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "Initialization failed", e)
        }
    }
}
```

Register in AndroidManifest.xml:

```xml
<application
    android:name=".MyApplication"
    ...>
```

For advanced configuration options, see our [Configuration Guide](configuration.md).

### 2. User Authentication

Authenticate users using email or token:

```kotlin
squadSDK.authenticateUser(
    identifier = "user@example.com",
    authType = AuthType.EMAIL
) { result ->
    when (result) {
        is AuthResult.Success -> {
            Log.d("SquadSDK", "User authenticated: ${result.user.id}")
        }
        is AuthResult.Error -> {
            Log.e("SquadSDK", "Authentication failed", result.error)
        }
    }
}
```

Learn more in our [User Authentication Guide](user-auth.md).

### 3. WebView Integration

Present the Squad experience:

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var squadSDK: SquadSDK

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val webViewConfig = WebViewConfig.Builder()
            .setFeatures(setOf(
                Feature.VOICE_CALLS,
                Feature.FREESTYLES,
                Feature.POLLS
            ))
            .build()

        try {
            val webView = squadSDK.presentWebView(this, webViewConfig)
            setContentView(webView)
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "WebView presentation failed", e)
        }
    }
}
```

For comprehensive WebView management, see our [WebView Management Guide](webview.md).

## Required Permissions

Add to AndroidManifest.xml:

```xml
<!-- Required permissions -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />

<!-- Optional permissions -->
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.BLUETOOTH" />
```

## Integration Guides

- [Installation & Setup](installation.md) - Detailed installation steps
- [Configuration Guide](configuration.md) - Advanced SDK configuration
- [WebView Management](webview.md) - WebView integration and management
- [User Authentication](user-auth.md) - Authentication implementation
- [WebView Events](webview-events.md) - Event handling guide
- [ProGuard Configuration](proguard.md) - ProGuard setup

## Best Practices

1. **Initialization**

   - Initialize in Application class
   - Handle configuration changes
   - Manage lifecycle properly

2. **Security**

   - Implement proper permissions
   - Configure ProGuard rules
   - Set up certificate pinning

3. **Performance**

   - Memory management
   - Resource optimization
   - Background handling

4. **User Experience**
   - Loading states
   - Error handling
   - Offline support

## Troubleshooting

For common issues and solutions, see our [Troubleshooting Guide](troubleshooting.md).

## Additional Resources

- [Sample Projects](https://github.com/withyoursquad/android-samples)

## Support

Need help? Our support team is ready to assist:

- Support Center: [https://support.squadforsports.com](https://support.squadforsports.com)
- Email: support@squadforsports.com
- Documentation: [https://docs.squadforsports.com](https://docs.squadforsports.com)
