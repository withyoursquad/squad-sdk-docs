# WebView Integration

The Squad SDK uses WebViews to display the Squad social features within your app. WebViews provide a seamless way to embed web content into native mobile applications, allowing you to leverage Squad's web-based UI components and functionality.

## Presenting the Squad WebView

To present the Squad WebView in your app, follow these steps:

### iOS

```swift
do {
    try squadSDK.openSquadWebView()
    print("Squad WebView presented successfully")
} catch {
    print("Failed to present Squad WebView: \(error)")
}
```

### Android

```kotlin
try {
    squadSDK.openSquadWebView()
    println("Squad WebView presented successfully")
} catch (e: Exception) {
    println("Failed to present Squad WebView: $e")
}
```

### React Native

```javascript
try {
  await squadSDK.openSquadWebView();
  console.log("Squad WebView presented successfully");
} catch (error) {
  console.error("Failed to present Squad WebView:", error);
}
```

Before calling the `openSquadWebView` method, ensure that you have initialized the SDK and authenticated the user.

## WebView Events

The Squad SDK emits events to notify your app about important WebView-related actions, such as:

- WebView loaded
- WebView failed to load
- WebView dismissed

You can listen for these events to perform actions or update your app's UI accordingly. For example, you might want to show a loading indicator while the WebView is loading or handle errors gracefully if the WebView fails to load.

To learn more about handling WebView events, refer to the platform-specific guides:

- [iOS WebView Events Guide](ios/webview-events.md)
- [Android WebView Events Guide](android/webview-events.md)
- [React Native WebView Events Guide](react-native/webview-events.md)

By leveraging WebViews, the Squad SDK makes it easy to integrate powerful social features into your app without the need for complex native implementations. With just a few lines of code, you can provide your users with a rich and engaging social experience powered by Squad.
