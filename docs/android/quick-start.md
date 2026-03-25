# Android Quick Start

## Installation

Add the JitPack repository to your `settings.gradle.kts` (Maven Central support coming soon):

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven("https://jitpack.io")
    }
}
```

Then add the SDK to your module's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.github.withyoursquad:squad-sports-android:1.5.0")
}
```

## Basic Integration

Two lines of code. No coroutines, no lifecycle management — the SDK handles it.

```kotlin
// In Application.onCreate() or any Activity:
SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
    apiKey = "sqk_live_...",
)

// Launch the experience from any click handler:
SquadExperienceActivity.launch(context)
```

The SDK registers its own Activity in the manifest automatically — no manifest changes needed in your app.

## With Partner Auth (No Login Screen)

```kotlin
SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
    apiKey = "sqk_live_...",
    userData = PartnerUserData(
        email = currentUser.email,
        displayName = currentUser.name,
        externalUserId = currentUser.id,
    ),
)
```

## With Ticketmaster SSO

```kotlin
SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
    apiKey = "sqk_live_...",
    ssoToken = tmAccessToken,
    ssoProvider = SSOProvider.TICKETMASTER,
)
```

## Push Notifications

Pass the FCM token at setup:

```kotlin
SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
    apiKey = "sqk_live_...",
    pushToken = FirebaseMessaging.getInstance().token.await(),
)
```

## Requirements

- Android API 24+ (Android 7.0)
- Kotlin 1.9+
- Jetpack Compose BOM 2024.01+
