# ProGuard Configuration Guide

## Overview

This guide covers the ProGuard configuration required for the Squad SDK, ensuring proper code optimization while maintaining functionality.

## Basic Configuration

Add these rules to your `proguard-rules.pro`:

```proguard
# Squad SDK Core
-keep class com.withsquad.sdk.** { *; }
-keepclassmembers class com.withsquad.sdk.** { *; }
-keepnames class com.withsquad.sdk.** { *; }

# Keep SDK interfaces
-keep interface com.withsquad.sdk.** { *; }

# Keep SDK enums
-keepclassmembers enum com.withsquad.sdk.** { *; }
```

## WebView Configuration

```proguard
# JavaScript Interface
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
}

# WebView Bridge
-keepclassmembers class com.withsquad.sdk.bridge.** {
    public *;
}

# WebView Chrome Client
-keepclassmembers class * extends android.webkit.WebChromeClient {
    public void openFileChooser(...);
    public boolean onShowFileChooser(...);
}
```

## Voice Calling Components

```proguard
# Twilio Voice SDK
-keep class com.twilio.** { *; }
-keep class tvo.webrtc.** { *; }
-keep class org.webrtc.** { *; }

# Audio Components
-keep class com.withsquad.sdk.audio.** { *; }
-keep class * implements com.withsquad.sdk.audio.AudioManager { *; }
```

## Data Models

```proguard
# Keep all Squad SDK models
-keep class com.withsquad.sdk.models.** { *; }
-keepclassmembers class com.withsquad.sdk.models.** {
    <init>(...);
    <fields>;
}

# Keep Serializable/Parcelable implementation
-keepclassmembers class * implements android.os.Parcelable {
    public static final android.os.Parcelable$Creator *;
}
-keepclassmembers class * implements java.io.Serializable {
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}
```

## Network Components

```proguard
# OkHttp
-dontwarn okhttp3.**
-dontwarn okio.**
-keepnames class okhttp3.internal.publicsuffix.PublicSuffixDatabase

# Retrofit
-keepattributes Signature
-keepattributes Exceptions
-keepclasseswithmembers class * {
    @retrofit2.http.* <methods>;
}
```

## Event Handling

```proguard
# Keep event classes
-keep class com.withsquad.sdk.events.** { *; }
-keepclassmembers class * {
    @com.withsquad.sdk.events.Subscribe <methods>;
}
```

## Security Components

```proguard
# Cryptography
-keepclassmembers class * extends java.security.Provider {
    <init>(...);
}

# Certificate Pinning
-keepclassmembers class com.withsquad.sdk.security.** {
    public static final byte[];
}
```

## Debug Configuration

Add these rules for debug builds:

```proguard
# Keep source file names and line numbers for stack traces
-keepattributes SourceFile,LineNumberTable

# If you want to get stack traces with obfuscated class names
-renamesourcefileattribute SourceFile
```

## R8 Full Mode Configuration

If using R8 in full mode:

```proguard
# Enable R8 full mode
-allowaccessmodification
-repackageclasses 'com.withsquad.sdk'

# Keep important SDK components
-keep class com.withsquad.sdk.SquadSDK {
    public protected *;
}
```

## Troubleshooting Common Issues

### 1. Missing Classes

If you encounter `ClassNotFoundException`:

```proguard
# Keep factory classes
-keepnames class * implements com.withsquad.sdk.internal.Factory

# Keep classes accessed via reflection
-keepclassmembers class * {
    @com.withsquad.sdk.annotations.Keep *;
}
```

### 2. JavaScript Interface Issues

If WebView JavaScript interface stops working:

```proguard
# Keep all JavaScript interface methods
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
    public *; # Keep all public methods
}
```

### 3. Serialization Issues

If you encounter serialization problems:

```proguard
# Keep all fields for serialization
-keepclassmembers class * {
    @com.google.gson.annotations.SerializedName <fields>;
}
```

## Verification

To verify your ProGuard configuration:

1. Build your release variant
2. Check `build/outputs/mapping/release/mapping.txt`
3. Test all SDK features in release build
4. Verify WebView functionality
5. Test voice calling features

## Size Impact

The Squad SDK ProGuard rules typically result in:

- Minimal impact on app size (~100-200KB increase)
- Maintained functionality with optimization
- Secured code through obfuscation

## Best Practices

1. **Testing**

   - Always test release builds
   - Verify all SDK features
   - Check crash reporting

2. **Maintenance**

   - Update rules with SDK updates
   - Monitor mapping files
   - Keep backup of mapping files

3. **Optimization**
   - Use R8 when possible
   - Enable optimization
   - Monitor size impact

## Related Documentation

- [Installation Guide](installation.md)
- [Configuration Guide](configuration.md)
- [Troubleshooting](troubleshooting.md)
