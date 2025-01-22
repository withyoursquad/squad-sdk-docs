# Android Installation & Setup

## Requirements

- Android API level 21 (Android 5.0) or higher
- Android Studio Arctic Fox (2020.3.1) or newer
- Gradle 7.0 or higher
- Java 8 or higher

## Installation

### Gradle Setup

1. Add the Maven Central repository to your project's `settings.gradle`:

```groovy
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
}
```

2. Add the dependency to your app's `build.gradle`:

```groovy
dependencies {
    implementation 'com.withsquad.sdk:squadline:1.0.0'
}
```

### Manual AAR Integration (Alternative)

If you prefer manual integration:

1. Download the AAR from our [releases page](https://github.com/withyoursquad/squad-sports-android/releases)
2. Add the AAR to your project's `libs` folder
3. Add the dependency in your app's `build.gradle`:

```groovy
dependencies {
    implementation files('libs/squadline-sdk-1.0.0.aar')
}
```

## Project Setup

### 1. Required Permissions

Add the following to your `AndroidManifest.xml`:

```xml
<!-- Required permissions -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />

<!-- Optional permissions -->
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"
    android:maxSdkVersion="32" />
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
```

### 2. Network Security

Add network security configuration to `res/xml/network_security_config.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">withsquad.com</domain>
        <pin-set>
            <!-- Squad API certificate pins -->
            <pin digest="SHA-256">base64-encoded-pin-here</pin>
            <!-- Backup pin -->
            <pin digest="SHA-256">backup-pin-here</pin>
        </pin-set>
    </domain-config>
</network-security-config>
```

Reference it in your `AndroidManifest.xml`:

```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ...>
```

### 3. ProGuard Configuration

If you use ProGuard, add these rules to your `proguard-rules.pro`:

```proguard
# Squad SDK
-keep class com.withsquad.sdk.** { *; }
-keepclassmembers class com.withsquad.sdk.** { *; }

# WebView
-keepclassmembers class * extends android.webkit.WebChromeClient {
    public void openFileChooser(...);
    public boolean onShowFileChooser(...);
}

# Keep Squad JavaScript interface
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
}
```

## Post-Installation Steps

### 1. Initialize the SDK

In your Application class:

```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        try {
            SquadSDK.initialize(
                context = this,
                config = SquadConfig.Builder()
                    .setOrganizationId("YOUR_ORG_ID")
                    .setApiKey("YOUR_API_KEY")
                    .build()
            )
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "Initialization failed", e)
        }
    }
}
```

Don't forget to register your Application class in `AndroidManifest.xml`:

```xml
<application
    android:name=".MyApplication"
    ...>
```

### 2. Verify Installation

Add a test implementation:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Verify SDK version
        Log.d("SquadSDK", "Version: ${SquadSDK.version}")

        // Check initialization status
        if (SquadSDK.isInitialized) {
            Log.d("SquadSDK", "SDK initialized successfully")
        }
    }
}
```

## Troubleshooting

### Common Issues

1. **Gradle Sync Failed**

   - Check Gradle version compatibility
   - Update Gradle plugin version
   - Sync project with Gradle files

2. **Manifest Merge Failed**

   - Check for permission conflicts
   - Resolve duplicate declarations
   - Update manifest merger rules

3. **ProGuard Issues**
   - Verify ProGuard rules
   - Check mapping file
   - Enable ProGuard debugging

### Support

For installation issues:

- Check our [troubleshooting guide](../troubleshooting.md)
- Visit our [support center](https://support.squadforsports.com)
- Contact support at support@squadforsports.com

## Next Steps

- Configure the SDK using the [Configuration Guide](configuration.md)
- Set up user authentication with [User Auth Guide](user-auth.md)
- Integrate WebView using [WebView Management](webview.md)
