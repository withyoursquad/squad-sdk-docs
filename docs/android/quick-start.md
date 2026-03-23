# Android Quick Start

## Installation

Add the Squad Sports SDK to your app's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.squadsports:sdk:1.3.1")
}
```

Or via project-level dependency on the SDK module.

## Basic Integration

```kotlin
import com.squadsports.sdk.SquadSportsSDK
import com.squadsports.sdk.SquadExperienceActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        lifecycleScope.launch {
            SquadSportsSDK.setup(
                context = this@MainActivity,
                partnerId = "acme-sports",
                apiKey = "sqk_live_...",
            )
            SquadExperienceActivity.launch(this@MainActivity)
            finish()
        }
    }
}
```

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

## Embedding in a Compose App

```kotlin
@Composable
fun MainScreen() {
    val context = LocalContext.current

    LaunchedEffect(Unit) {
        SquadSportsSDK.setup(
            context = context,
            partnerId = "acme-sports",
            apiKey = "sqk_live_...",
        )
        // Launch the experience activity
        SquadExperienceActivity.launch(context)
    }
}
```

## Requirements

- Android API 24+ (Android 7.0)
- Kotlin 1.9+
- Jetpack Compose BOM 2024.01+
