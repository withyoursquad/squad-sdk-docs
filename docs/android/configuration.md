# Android Configuration Guide

## Overview

This guide covers all configuration options available in the Squad SDK for Android, including initialization, security, networking, and feature-specific settings.

## Basic Configuration

### SDK Configuration

```kotlin
data class SquadConfig(
    val organizationId: String,
    val apiKey: String,
    val environment: Environment,
    val logLevel: LogLevel
)

// Using Builder pattern
SquadSDK.Builder(context)
    .setOrganizationId("YOUR_ORG_ID")
    .setApiKey("YOUR_API_KEY")
    .setEnvironment(Environment.PRODUCTION)
    .setLogLevel(LogLevel.INFO)
    .build()
    .initialize()
```

### Environment Configuration

```kotlin
enum class Environment {
    PRODUCTION,
    STAGING,
    DEVELOPMENT;

    data class Custom(val baseUrl: String)
}

// Custom environment setup
SquadSDK.Builder(context)
    .setEnvironment(Environment.Custom("https://dev-api.yourdomain.com"))
    .build()
```

## Security Configuration

### Certificate Pinning

```kotlin
val certificateConfig = CertificatePinningConfig.Builder()
    .addCertificate("api.withsquad.com", "sha256/XXXX")
    .addBackupCertificate("api.withsquad.com", "sha256/YYYY")
    .setIncludeDefaultCertificates(true)
    .build()

squadSDK.setCertificatePinning(certificateConfig)
```

### User Data Protection

```kotlin
val securityConfig = SecurityConfig.Builder()
    .setEncryption(EncryptionConfig.AES256)
    .setBiometricAuth(BiometricConfig.OPTIONAL)
    .setSecureStorage(StorageConfig.ENCRYPTED_SHARED_PREFS)
    .build()

squadSDK.setSecurityConfig(securityConfig)
```

## Network Configuration

### Timeouts and Retries

```kotlin
val networkConfig = NetworkConfig.Builder()
    .setConnectionTimeout(30_000) // 30 seconds
    .setReadTimeout(30_000)
    .setWriteTimeout(30_000)
    .setRetryPolicy(
        RetryPolicy(
            maxRetries = 3,
            backoffMultiplier = 1.5f,
            initialRetryDelayMs = 1000
        )
    )
    .build()

squadSDK.setNetworkConfig(networkConfig)
```

### Caching Strategy

```kotlin
val cacheConfig = CacheConfig.Builder()
    .setDiskCacheSize(50 * 1024 * 1024L) // 50 MB
    .setMemoryCacheSize(10 * 1024 * 1024L) // 10 MB
    .setTimeToLive(3600) // 1 hour
    .setDiskCacheDirectory(context.cacheDir)
    .build()

squadSDK.setCacheConfig(cacheConfig)
```

## WebView Configuration

### Basic Setup

```kotlin
val webViewConfig = WebViewConfig.Builder()
    .setMediaPlaybackRequiresUserGesture(false)
    .setDomStorageEnabled(true)
    .setJavaScriptEnabled(true)
    .setGeolocationEnabled(false)
    .build()

squadSDK.setWebViewConfig(webViewConfig)
```

### Content Configuration

```kotlin
val contentConfig = ContentConfig.Builder()
    .setAllowedContentTypes(setOf(ContentType.ALL))
    .setUserScalable(false)
    .setLoadWithOverviewMode(true)
    .setUseWideViewPort(true)
    .build()

webViewConfig.setContentConfig(contentConfig)
```

## Voice Call Configuration

### Audio Settings

```kotlin
val audioConfig = AudioConfig.Builder()
    .setAudioMode(AudioMode.VOICE_CHAT)
    .setEchoCancellation(true)
    .setNoiseSuppression(true)
    .setAutomaticGainControl(true)
    .build()

squadSDK.setAudioConfig(audioConfig)
```

### Call Quality

```kotlin
val qualityConfig = CallQualityConfig.Builder()
    .setBitrateMode(BitrateMode.ADAPTIVE)
    .setMinBitrate(8000)
    .setMaxBitrate(32000)
    .setOpusMode(OpusMode.VOIP)
    .build()

squadSDK.setCallQualityConfig(qualityConfig)
```

## Analytics Configuration

```kotlin
val analyticsConfig = AnalyticsConfig.Builder()
    .setEnabled(true)
    .setTrackingEvents(
        setOf(
            TrackingEvent.CALLS,
            TrackingEvent.ERRORS,
            TrackingEvent.PERFORMANCE
        )
    )
    .addCustomDimension("app_version", BuildConfig.VERSION_NAME)
    .addCustomDimension("environment", "production")
    .build()

squadSDK.setAnalyticsConfig(analyticsConfig)
```

## Error Handling Configuration

```kotlin
val errorConfig = ErrorConfig.Builder()
    .setRetryableErrors(setOf(ErrorType.NETWORK, ErrorType.TIMEOUT))
    .setMaxRetries(3)
    .setErrorCallback { error ->
        // Handle errors
        Log.e("SquadSDK", "Error occurred: $error")
    }
    .build()

squadSDK.setErrorConfig(errorConfig)
```

## Resource Management

### Memory Management

```kotlin
val memoryConfig = MemoryConfig.Builder()
    .setLowMemoryThreshold(50 * 1024 * 1024L) // 50 MB
    .setCriticalMemoryThreshold(25 * 1024 * 1024L) // 25 MB
    .setCleanupCallback {
        // Perform cleanup
    }
    .build()

squadSDK.setMemoryConfig(memoryConfig)
```

### Background Tasks

```kotlin
val backgroundConfig = BackgroundConfig.Builder()
    .setAllowedTasks(setOf(BackgroundTask.VOICE_CALLS, BackgroundTask.MESSAGE_DELIVERY))
    .setTimeout(30_000)
    .setCompletionCallback { result ->
        // Handle background task completion
    }
    .build()

squadSDK.setBackgroundConfig(backgroundConfig)
```

## Advanced Configuration

### Custom Event Handling

```kotlin
val eventConfig = EventConfig.Builder()
    .setEventHandler { event ->
        when (event) {
            is SquadEvent.CallStarted -> {
                // Handle call start
            }
            is SquadEvent.CallEnded -> {
                // Handle call end
            }
            is SquadEvent.Error -> {
                // Handle error
            }
        }
    }
    .build()

squadSDK.setEventConfig(eventConfig)
```

### Debug Configuration

```kotlin
if (BuildConfig.DEBUG) {
    val debugConfig = DebugConfig.Builder()
        .setLogLevel(LogLevel.DEBUG)
        .setNetworkLogging(true)
        .setPerformanceMetricsEnabled(true)
        .setScreenshotEnabled(true)
        .build()

    squadSDK.setDebugConfig(debugConfig)
}
```

## ProGuard Configuration

Add these rules to your `proguard-rules.pro`:

```proguard
# Squad SDK
-keep class com.withsquad.sdk.** { *; }
-keepclassmembers class com.withsquad.sdk.** { *; }

# WebView JavaScript Interface
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
}
```

## Best Practices

1. **Security**

   - Enable certificate pinning in production
   - Use encrypted shared preferences for sensitive data
   - Configure appropriate timeouts
   - Implement proper ProGuard rules

2. **Performance**

   - Configure appropriate cache sizes
   - Implement memory management
   - Handle lifecycle events properly
   - Use appropriate background task handling

3. **Error Handling**

   - Configure comprehensive error handling
   - Implement appropriate retry strategies
   - Use proper logging levels
   - Handle configuration changes

4. **Testing**
   - Use staging environment for testing
   - Enable debug logging in development
   - Test with various network conditions
   - Verify ProGuard configurations

## Related Documentation

- [Installation Guide](installation.md)
- [WebView Management](webview.md)
- [Troubleshooting](../troubleshooting.md)
