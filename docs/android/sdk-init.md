# Squad SDK Initialization for Android

The Squad SDK initialization process consists of three key steps:

1. SDK Initialization
2. User Initialization
3. WebView Initialization

## SDK Initialization

### Obtaining Credentials

1. Login to your developer account at [https://developer.squadforsports.com](https://developer.squadforsports.com)
2. Navigate to Organization Settings
3. Copy your Organization ID and API Key

### Initialization Code

```kotlin
import com.withsquad.sdk.SquadSDK
import com.withsquad.sdk.config.SquadConfig

class MyApplication : Application() {
    lateinit var squadSDK: SquadSDK

    override fun onCreate() {
        super.onCreate()

        // SDK Initialization
        squadSDK = SquadSDK.Builder(this)
            .setOrganizationId("YOUR_ORGANIZATION_ID")
            .setApiKey("YOUR_API_KEY")
            .setEnvironment(SquadEnvironment.PRODUCTION) // or STAGING
            .build()

        try {
            squadSDK.initialize()
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "Initialization Error: ${e.message}")
        }
    }
}
```

## User Initialization

### Authentication Methods

The Squad SDK supports two primary user initialization methods:

1. Email Authentication

```kotlin
fun initializeUser(email: String) {
    squadSDK.initializeUser(
        identifier = email,
        authType = AuthType.EMAIL
    ) { result ->
        when (result) {
            is UserInitResult.Success -> {
                Log.d("SquadSDK", "User initialized successfully")
            }
            is UserInitResult.Error -> {
                Log.e("SquadSDK", "User initialization failed: ${result.error}")
            }
        }
    }
}
```

2. Token Authentication

```kotlin
fun initializeUser(token: String) {
    squadSDK.initializeUser(
        identifier = token,
        authType = AuthType.TOKEN
    ) { result ->
        when (result) {
            is UserInitResult.Success -> {
                Log.d("SquadSDK", "User initialized successfully")
            }
            is UserInitResult.Error -> {
                Log.e("SquadSDK", "User initialization failed: ${result.error}")
            }
        }
    }
}
```

## WebView Initialization

### Presenting the Squad Experience

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var squadSDK: SquadSDK

    fun openSquadExperience() {
        val config = WebViewConfiguration.Builder()
            .setFeatures(
                setOf(
                    SquadFeature.FREESTYLES,
                    SquadFeature.POLLS,
                    SquadFeature.SQUAD_MANAGEMENT,
                    SquadFeature.VOICE_CALLING,
                    SquadFeature.VOICE_MESSAGES
                )
            )
            .build()

        try {
            squadSDK.presentWebView(
                activity = this,
                configuration = config
            )
        } catch (e: SquadSDKException) {
            Log.e("SquadSDK", "WebView presentation failed: ${e.message}")
        }
    }
}
```

## Complete Initialization Example

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var squadSDK: SquadSDK

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // 1. Get SDK instance
        squadSDK = (application as MyApplication).squadSDK

        // 2. Initialize user
        squadSDK.initializeUser(
            identifier = "user@example.com",
            authType = AuthType.EMAIL
        ) { result ->
            when (result) {
                is UserInitResult.Success -> {
                    // 3. Present WebView
                    openSquadExperience()
                }
                is UserInitResult.Error -> {
                    // Handle error
                    showError(result.error)
                }
            }
        }
    }

    private fun showError(error: SquadError) {
        AlertDialog.Builder(this)
            .setTitle("Error")
            .setMessage(error.message)
            .setPositiveButton("OK", null)
            .show()
    }
}
```

## Error Handling

```kotlin
sealed class SquadSDKException : Exception() {
    class InitializationError(message: String) : SquadSDKException()
    class AuthenticationError(message: String) : SquadSDKException()
    class WebViewError(message: String) : SquadSDKException()
}
```

## Best Practices

1. Initialize the SDK in your Application class
2. Handle configuration changes properly
3. Implement proper lifecycle management
4. Secure credential storage
5. Handle permissions appropriately
6. Maintain WebView state
7. Keep the SDK updated

## Troubleshooting

Common issues and solutions:

1. Initialization Failures

   - Verify network connectivity
   - Check API credentials
   - Ensure proper manifest setup

2. WebView Issues

   - Check WebView version
   - Verify permissions
   - Review lifecycle management

3. Authentication Problems
   - Validate credentials
   - Check token expiration
   - Review error logs

## Support

- Documentation: [Squad Developer Docs](https://docs.squadforsports.com)
- Support Email: support@squadforsports.com

---

**Note:** Always refer to the latest documentation for the most up-to-date SDK initialization instructions.
