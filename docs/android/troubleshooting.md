# Android Troubleshooting Guide

This guide addresses Android-specific issues and solutions when integrating the Squad SDK.

## Installation Issues

### Gradle Integration

#### Issue: Dependency Resolution

```
Error: Failed to resolve: com.withsquad.sdk:squadline:1.0.0
```

**Solutions:**

1. Check repositories in settings.gradle:

```gradle
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
}
```

2. Force dependency refresh:

```bash
./gradlew clean build --refresh-dependencies
```

### Version Conflicts

#### Issue: Dependency Version Conflicts

```
Error: Duplicate class found
```

**Solutions:**

1. Add resolution strategy:

```gradle
configurations.all {
    resolutionStrategy {
        force 'com.withsquad.sdk:squadline:1.0.0'
    }
}
```

2. Check dependency tree:

```bash
./gradlew app:dependencies
```

## Build Issues

### ProGuard/R8

#### Issue: ProGuard Optimization Failures

```
Error: Class not found after ProGuard optimization
```

**Solutions:**

1. Add ProGuard rules:

```proguard
-keep class com.withsquad.sdk.** { *; }
-keepclassmembers class com.withsquad.sdk.** { *; }
```

2. Check mapping file:

```bash
build/outputs/mapping/release/mapping.txt
```

### Architecture Issues

#### Issue: ABI Compatibility

```
Error: Unable to load native library
```

**Solutions:**

1. Configure ABI filters:

```gradle
android {
    defaultConfig {
        ndk {
            abiFilters 'armeabi-v7a', 'arm64-v8a', 'x86', 'x86_64'
        }
    }
}
```

## WebView Issues

### WebView Loading

#### Issue: Content Loading Failures

```
Error: WebView failed to load content
```

**Solutions:**

1. Configure WebView properly:

```kotlin
webView.settings.apply {
    javaScriptEnabled = true
    domStorageEnabled = true
    mediaPlaybackRequiresUserGesture = false
}

webView.webViewClient = object : WebViewClient() {
    override fun onReceivedError(
        view: WebView?,
        request: WebResourceRequest?,
        error: WebResourceError?
    ) {
        // Handle error
    }
}
```

2. Handle SSL errors:

```kotlin
webView.webViewClient = object : WebViewClient() {
    override fun onReceivedSslError(
        view: WebView?,
        handler: SslErrorHandler?,
        error: SslError?
    ) {
        if (BuildConfig.DEBUG) {
            handler?.proceed()
        } else {
            handler?.cancel()
        }
    }
}
```

### JavaScript Bridge

#### Issue: Bridge Communication Failures

```
Error: JavaScript interface not working
```

**Solutions:**

1. Verify bridge setup:

```kotlin
class SquadJSInterface(private val context: Context) {
    @JavascriptInterface
    fun postMessage(message: String) {
        // Handle message
    }
}

webView.addJavascriptInterface(SquadJSInterface(context), "SquadAndroid")
```

2. Debug bridge messages:

```kotlin
webView.evaluateJavascript(
    "(function() { return window.SquadBridge != null; })();"
) { result ->
    Log.d("SquadSDK", "Bridge available: $result")
}
```

## Audio Issues

### AudioManager

#### Issue: Audio Configuration

```
Error: Failed to initialize audio system
```

**Solutions:**

1. Configure audio settings:

```kotlin
val audioManager = context.getSystemService(Context.AUDIO_SERVICE) as AudioManager
audioManager.mode = AudioManager.MODE_IN_COMMUNICATION
audioManager.isSpeakerphoneOn = false
```

2. Handle audio focus:

```kotlin
private val audioFocusRequest = AudioFocusRequest.Builder(
    AudioManager.AUDIOFOCUS_GAIN
).run {
    setAudioAttributes(AudioAttributes.Builder().run {
        setUsage(AudioAttributes.USAGE_VOICE_COMMUNICATION)
        setContentType(AudioAttributes.CONTENT_TYPE_SPEECH)
        build()
    })
    setAcceptsDelayedFocusGain(true)
    setOnAudioFocusChangeListener { focusChange ->
        // Handle focus change
    }
    build()
}
```

### Permissions

#### Issue: Microphone Access

```
Error: Missing RECORD_AUDIO permission
```

**Solutions:**

1. Check permissions:

```kotlin
private fun checkAudioPermission() {
    if (ContextCompat.checkSelfPermission(
        context,
        Manifest.permission.RECORD_AUDIO
    ) != PackageManager.PERMISSION_GRANTED) {
        requestAudioPermission()
    }
}

private fun requestAudioPermission() {
    ActivityCompat.requestPermissions(
        activity,
        arrayOf(Manifest.permission.RECORD_AUDIO),
        PERMISSION_REQUEST_CODE
    )
}
```

## Memory Management

### Memory Leaks

#### Issue: Memory Leaks

```
Error: Memory leak detected
```

**Solutions:**

1. Handle activity lifecycle:

```kotlin
override fun onDestroy() {
    webView.clearCache(true)
    webView.clearHistory()
    webView.destroy()
    super.onDestroy()
}
```

2. Monitor memory:

```kotlin
class MemoryMonitor {
    fun logMemoryInfo(context: Context) {
        val runtime = Runtime.getRuntime()
        val usedMemInMB = (runtime.totalMemory() - runtime.freeMemory()) / 1048576L
        Log.d("MemoryMonitor", "Used memory: $usedMemInMB MB")
    }
}
```

## Debug Tools

### Logging

1. Enable SDK logging:

```kotlin
SquadSDK.setLogLevel(LogLevel.DEBUG)
```

2. Add debug interceptor:

```kotlin
class DebugInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request()
        Log.d("SquadSDK", "Request: ${request.url}")
        return chain.proceed(request)
    }
}
```

### Performance Monitoring

Monitor WebView performance:

```kotlin
webView.setWebViewClient(object : WebViewClient() {
    override fun onPageStarted(view: WebView, url: String, favicon: Bitmap?) {
        startTime = System.currentTimeMillis()
    }

    override fun onPageFinished(view: WebView, url: String) {
        val loadTime = System.currentTimeMillis() - startTime
        Log.d("SquadSDK", "Page load time: $loadTime ms")
    }
})
```

## Configuration Changes

Handle configuration changes properly:

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
    // Handle configuration change
    webView.requestLayout()
}
```

## Related Resources

- [General Troubleshooting Guide](../troubleshooting.md)
- [Android Configuration Guide](configuration.md)
- [Android WebView Management](webview.md)
- [ProGuard Configuration](proguard.md)

## Support

When contacting support, provide:

- Android Studio version
- Android OS version
- Device model
- SDK version
- Error logs
- Steps to reproduce
- Sample project (if possible)
- ProGuard mapping file (if applicable)
