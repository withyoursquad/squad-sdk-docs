# Handling WebView Events with Squad SDK for Android

When using the Squad SDK for Android, you can listen for various events emitted by the WebView to perform actions or update your app's UI accordingly. This guide will walk you through the process of handling WebView events in your Android app.

## Available Events

The Squad SDK for Android emits the following WebView events:

- `onWebViewLoaded`: Triggered when the WebView finishes loading
- `onWebViewError`: Triggered when the WebView encounters an error
- `onWebViewDismissed`: Triggered when the WebView is dismissed
- `onWebViewStateChanged`: Triggered when the WebView state changes

## Implementing the Event Listener

To listen for WebView events, implement the `SquadWebViewListener` interface:

```kotlin
class MainActivity : AppCompatActivity(), SquadWebViewListener {
    private lateinit var squadSDK: SquadSDK

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Set the WebView listener
        squadSDK.setWebViewListener(this)
    }

    override fun onWebViewLoaded() {
        Log.d("SquadSDK", "WebView loaded successfully")
        // Handle WebView loaded event
    }

    override fun onWebViewError(error: SquadError) {
        Log.e("SquadSDK", "WebView error: ${error.message}")
        // Handle WebView error
    }

    override fun onWebViewDismissed() {
        Log.d("SquadSDK", "WebView dismissed")
        // Handle WebView dismissed
    }

    override fun onWebViewStateChanged(state: WebViewState) {
        Log.d("SquadSDK", "WebView state changed: $state")
        // Handle state change
    }
}
```

## Handling Specific Events

### WebView Loaded

```kotlin
override fun onWebViewLoaded() {
    // Hide loading indicator
    loadingProgressBar.visibility = View.GONE

    // Enable interaction
    webViewContainer.isEnabled = true

    // Update UI state
    updateUIState(SquadUIState.READY)
}
```

### WebView Error

```kotlin
override fun onWebViewError(error: SquadError) {
    when (error) {
        is SquadError.NetworkError -> {
            showErrorDialog(
                title = "Connection Error",
                message = "Please check your internet connection"
            )
        }
        is SquadError.LoadError -> {
            showErrorDialog(
                title = "Loading Error",
                message = error.message
            )
        }
        else -> {
            showErrorDialog(
                title = "Error",
                message = "An unexpected error occurred"
            )
        }
    }
}

private fun showErrorDialog(title: String, message: String) {
    MaterialAlertDialogBuilder(this)
        .setTitle(title)
        .setMessage(message)
        .setPositiveButton("Retry") { _, _ ->
            reloadWebView()
        }
        .setNegativeButton("Cancel") { _, _ ->
            finish()
        }
        .show()
}
```

### WebView Dismissed

```kotlin
override fun onWebViewDismissed() {
    // Clean up resources
    cleanupResources()

    // Navigate back or update UI
    supportFragmentManager.popBackStack()
}

private fun cleanupResources() {
    // Perform cleanup tasks
    squadSDK.clearWebViewCache()
    // Release any held resources
}
```

### WebView State Changed

```kotlin
override fun onWebViewStateChanged(state: WebViewState) {
    when (state) {
        WebViewState.LOADING -> {
            showLoading()
        }
        WebViewState.READY -> {
            hideLoading()
        }
        WebViewState.ERROR -> {
            showError()
        }
    }
}

private fun showLoading() {
    loadingProgressBar.visibility = View.VISIBLE
    contentContainer.visibility = View.GONE
}

private fun hideLoading() {
    loadingProgressBar.visibility = View.GONE
    contentContainer.visibility = View.VISIBLE
}

private fun showError() {
    errorView.visibility = View.VISIBLE
    contentContainer.visibility = View.GONE
}
```

## Configuration Options

You can customize WebView behavior through configuration:

```kotlin
val config = WebViewConfiguration.Builder()
    .setLoadTimeout(timeoutMs = 10000)
    .setRetryCount(maxRetries = 3)
    .setErrorHandling(strategy = ErrorHandlingStrategy.RETRY)
    .build()

squadSDK.presentWebView(
    activity = this,
    configuration = config
)
```

## Lifecycle Management

Proper lifecycle management is crucial for WebView events:

```kotlin
class SquadActivity : AppCompatActivity(), SquadWebViewListener {
    override fun onResume() {
        super.onResume()
        squadSDK.setWebViewListener(this)
    }

    override fun onPause() {
        super.onPause()
        squadSDK.removeWebViewListener()
    }

    override fun onDestroy() {
        super.onDestroy()
        squadSDK.cleanup()
    }
}
```

## Error Recovery

Implement recovery strategies for WebView errors:

```kotlin
private fun handleWebViewRecovery() {
    val recoveryStrategy = WebViewRecoveryStrategy.Builder()
        .setMaxRetries(3)
        .setRetryDelay(1000)
        .setProgressiveDelay(true)
        .build()

    squadSDK.setRecoveryStrategy(recoveryStrategy)
}
```

## Best Practices

1. Always handle all possible WebView events
2. Implement proper error recovery mechanisms
3. Manage lifecycle events correctly
4. Clean up resources when WebView is dismissed
5. Provide clear feedback to users during state changes

## Next Steps

- Explore [Squad Line](../squad-line.md) capabilities
- Review [Troubleshooting](troubleshooting.md) guide

## Support

If you have any questions or need assistance:

- Visit our [Support Center](https://support.squadforsports.com)
- Contact us at support@squadforsports.com
