# Android WebView Management

## Overview

The Squad SDK uses Android WebView to provide seamless integration of Squad features into your Android application. This guide covers WebView setup, management, and optimization.

## WebView Setup

### Basic Implementation

```kotlin
class SquadActivity : AppCompatActivity() {
    private lateinit var webView: WebView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setupWebView()
    }

    private fun setupWebView() {
        webView = WebView(this).apply {
            layoutParams = ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.MATCH_PARENT
            )

            settings.apply {
                javaScriptEnabled = true
                domStorageEnabled = true
                mediaPlaybackRequiresUserGesture = false
                mixedContentMode = WebSettings.MIXED_CONTENT_NEVER_ALLOW
            }

            webViewClient = SquadWebViewClient()
            webChromeClient = SquadWebChromeClient()
        }

        setContentView(webView)
    }
}
```

### Custom Configuration

```kotlin
class SquadWebViewClient : WebViewClient() {
    override fun shouldOverrideUrlLoading(
        view: WebView,
        request: WebResourceRequest
    ): Boolean {
        // Handle URL loading
        return if (request.url.host?.endsWith("withsquad.com") == true) {
            false // Let WebView handle Squad URLs
        } else {
            handleExternalUrl(request.url)
            true
        }
    }

    override fun onReceivedSslError(
        view: WebView,
        handler: SslErrorHandler,
        error: SslError
    ) {
        if (BuildConfig.DEBUG) {
            handler.proceed() // Only in debug builds
        } else {
            handler.cancel()
        }
    }
}

class SquadWebChromeClient : WebChromeClient() {
    override fun onPermissionRequest(request: PermissionRequest) {
        // Handle permissions for camera/microphone
        request.grant(request.resources)
    }
}
```

## Bridge Communication

### JavaScript Interface Setup

```kotlin
class SquadJavaScriptInterface(
    private val context: Context,
    private val messageHandler: BridgeMessageHandler
) {
    @JavascriptInterface
    fun postMessage(message: String) {
        try {
            val jsonMessage = JSONObject(message)
            messageHandler.handleMessage(jsonMessage)
        } catch (e: JSONException) {
            Log.e("SquadSDK", "Invalid bridge message", e)
        }
    }
}

// Setting up the interface
webView.addJavascriptInterface(
    SquadJavaScriptInterface(context, messageHandler),
    "SquadAndroid"
)
```

### Message Handling

```kotlin
class BridgeMessageHandler {
    fun handleMessage(message: JSONObject) {
        when (message.optString("type")) {
            "call" -> handleCallEvent(message)
            "navigation" -> handleNavigation(message)
            "error" -> handleError(message)
            else -> Log.w("SquadSDK", "Unknown message type")
        }
    }

    fun sendToBridge(message: JSONObject) {
        val jsonString = message.toString()
        webView.post {
            webView.evaluateJavascript(
                "window.squadBridge.handleNativeMessage($jsonString)"
            ) { result ->
                // Handle result if needed
            }
        }
    }
}
```

## State Management

### WebView State Tracking

```kotlin
sealed class WebViewState {
    object Loading : WebViewState()
    object Ready : WebViewState()
    data class Error(val error: Throwable) : WebViewState()
}

class SquadWebViewClient : WebViewClient() {
    private val _state = MutableStateFlow<WebViewState>(WebViewState.Loading)
    val state: StateFlow<WebViewState> = _state.asStateFlow()

    override fun onPageStarted(view: WebView, url: String, favicon: Bitmap?) {
        super.onPageStarted(view, url, favicon)
        _state.value = WebViewState.Loading
    }

    override fun onPageFinished(view: WebView, url: String) {
        super.onPageFinished(view, url)
        _state.value = WebViewState.Ready
    }

    override fun onReceivedError(
        view: WebView,
        request: WebResourceRequest,
        error: WebResourceError
    ) {
        super.onReceivedError(view, request, error)
        _state.value = WebViewState.Error(WebViewException(error))
    }
}
```

### Session Management

```kotlin
class SessionManager(private val context: Context) {
    fun clearSession() {
        WebStorage.getInstance().deleteAllData()

        context.deleteDatabase("webview.db")
        context.deleteDatabase("webviewCache.db")

        CookieManager.getInstance().removeAllCookies(null)
    }

    fun persistSession() {
        CookieManager.getInstance().flush()
    }
}
```

## Security Implementation

### Content Security Policy

```kotlin
class SecurityConfig {
    private val cspRules = """
        default-src 'self' https://*.withsquad.com;
        script-src 'self' 'unsafe-inline' https://*.withsquad.com;
        style-src 'self' 'unsafe-inline' https://*.withsquad.com;
        img-src 'self' data: https://*.withsquad.com;
    """.trimIndent()

    fun injectCSP(webView: WebView) {
        webView.evaluateJavascript("""
            const meta = document.createElement('meta');
            meta.httpEquiv = 'Content-Security-Policy';
            meta.content = '$cspRules';
            document.head.appendChild(meta);
        """.trimIndent(), null)
    }
}
```

### Safe Browsing

```kotlin
class SafeBrowsingConfig(context: Context) {
    init {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O_MR1) {
            WebView.startSafeBrowsing(context) { success ->
                Log.d("SquadSDK", "Safe Browsing initialization: $success")
            }
        }
    }

    fun configureSafeBrowsing(webView: WebView) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O_MR1) {
            webView.settings.safeBrowsingEnabled = true
        }
    }
}
```

## Performance Optimization

### Memory Management

```kotlin
class MemoryManager {
    fun optimizeMemoryUsage(webView: WebView) {
        // Clear cache when low on memory
        webView.clearCache(true)

        // Trim memory when needed
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
            webView.setRendererPriorityPolicy(
                WebView.RENDERER_PRIORITY_BOUND,
                true
            )
        }
    }
}

class SquadActivity : AppCompatActivity() {
    override fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)
        if (level >= ComponentCallbacks2.TRIM_MEMORY_MODERATE) {
            memoryManager.optimizeMemoryUsage(webView)
        }
    }
}
```

### Resource Loading Optimization

```kotlin
class ResourceOptimizer {
    fun configureResourceLoading(webView: WebView) {
        webView.settings.apply {
            // Enable caching
            cacheMode = WebSettings.LOAD_DEFAULT

            // Enable app cache
            setAppCacheEnabled(true)
            setAppCachePath(webView.context.cacheDir.absolutePath)

            // Enable compression
            loadsImagesAutomatically = true
            blockNetworkImage = false

            // Enable local storage
            databaseEnabled = true
            domStorageEnabled = true
        }
    }
}
```

## Error Handling

```kotlin
class ErrorHandler {
    fun handleWebViewError(error: WebResourceError, request: WebResourceRequest) {
        when (error.errorCode) {
            ERROR_HOST_LOOKUP -> showOfflineView()
            ERROR_TIMEOUT -> retryLoading()
            else -> showErrorView(error.description.toString())
        }
    }

    private fun retryLoading() {
        // Implement retry logic
        webView.reload()
    }
}
```

## Lifecycle Management

```kotlin
class SquadActivity : AppCompatActivity() {
    override fun onResume() {
        super.onResume()
        webView.onResume()
        webView.resumeTimers()
    }

    override fun onPause() {
        webView.pauseTimers()
        webView.onPause()
        super.onPause()
    }

    override fun onDestroy() {
        webView.stopLoading()
        webView.clearHistory()
        webView.clearCache(true)
        webView.destroy()
        super.onDestroy()
    }

    override fun onConfigurationChanged(newConfig: Configuration) {
        super.onConfigurationChanged(newConfig)
        // Handle configuration changes
    }
}
```

## Best Practices

1. **Security**

   - Enable Safe Browsing
   - Implement proper CSP
   - Handle SSL errors correctly
   - Validate all URLs

2. **Performance**

   - Handle memory constraints
   - Implement efficient caching
   - Optimize resource loading
   - Handle configuration changes

3. **User Experience**

   - Handle offline state
   - Provide loading indicators
   - Implement error recovery
   - Maintain state during rotations

4. **Debugging**
   - Enable WebView debugging
   - Monitor memory usage
   - Track performance metrics
   - Log bridge communications

## Related Documentation

- [Configuration Guide](configuration.md)
- [Troubleshooting](../troubleshooting.md)
