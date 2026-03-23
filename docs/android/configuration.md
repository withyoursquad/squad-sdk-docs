# Android Configuration

## Custom Analytics

```kotlin
SquadAnalytics.customHandler = { eventName, properties ->
    Analytics.track(eventName, properties)
}
```

## Custom Logger

```kotlin
SquadLogger.handler = { level, tag, message ->
    when (level) {
        SquadLogLevel.ERROR -> Sentry.captureMessage("[$tag] $message")
        else -> Log.d(tag, message)
    }
}
SquadLogger.minLevel = SquadLogLevel.WARN
```

## Token Storage

Tokens are stored in `EncryptedSharedPreferences` (Android Keystore-backed). No configuration needed.

## ProGuard / R8

If using minification, add to `proguard-rules.pro`:

```
-keep class com.squadsports.sdk.** { *; }
-keep class withyoursquad.v2.** { *; }
```

## Cleanup

```kotlin
SquadSportsSDK.reset()
```
