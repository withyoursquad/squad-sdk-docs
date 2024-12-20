# Handling WebView Events with Squad SDK for iOS

When using the Squad SDK for iOS, you can listen for various events emitted by the WebView to perform actions or update your app's UI accordingly. This guide will walk you through the process of handling WebView events in your iOS app.

## Available Events

The Squad SDK for iOS emits the following WebView events:

- `webViewDidLoad`: Triggered when the WebView finishes loading.
- `webViewDidFailToLoad`: Triggered when the WebView fails to load due to an error.
- `webViewDidDismiss`: Triggered when the WebView is dismissed.

## Listening for Events

To listen for WebView events, you need to conform to the `SquadSDKDelegate` protocol in your view controller or any other class responsible for managing the Squad SDK.

1. Declare conformance to the `SquadSDKDelegate` protocol:

```swift
class ViewController: UIViewController, SquadSDKDelegate {
    // ...
}
```

2. Set the `delegate` property of the `SquadSDK` instance to `self`:

```swift
squadSDK.delegate = self
```

3. Implement the desired event handling methods from the `SquadSDKDelegate` protocol:

```swift
func squadSDKWebViewDidLoad(_ squadSDK: SquadSDK) {
    print("WebView loaded successfully")
}

func squadSDKWebViewDidFailToLoad(_ squadSDK: SquadSDK, error: Error) {
    print("WebView failed to load with error: \(error)")
}

func squadSDKWebViewDidDismiss(_ squadSDK: SquadSDK) {
    print("WebView dismissed")
}
```

## Handling Events

### WebView Loaded

The `squadSDKWebViewDidLoad` method is called when the WebView finishes loading. You can use this event to hide any loading indicators or perform other actions related to the successful loading of the WebView.

```swift
func squadSDKWebViewDidLoad(_ squadSDK: SquadSDK) {
    // Hide loading indicator
    loadingIndicator.stopAnimating()

    // Perform other actions
    // ...
}
```

### WebView Failed to Load

The `squadSDKWebViewDidFailToLoad` method is called when the WebView fails to load due to an error. You can use this event to display an error message to the user or perform error handling logic.

```swift
func squadSDKWebViewDidFailToLoad(_ squadSDK: SquadSDK, error: Error) {
    // Display error message
    let alertController = UIAlertController(title: "Error", message: "Failed to load Squad WebView: \(error)", preferredStyle: .alert)
    alertController.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
    present(alertController, animated: true, completion: nil)

    // Perform error handling
    // ...
}
```

### WebView Dismissed

The `squadSDKWebViewDidDismiss` method is called when the WebView is dismissed. You can use this event to perform any necessary cleanup or navigation actions.

```swift
func squadSDKWebViewDidDismiss(_ squadSDK: SquadSDK) {
    // Perform cleanup
    // ...

    // Navigate back to the previous screen
    navigationController?.popViewController(animated: true)
}
```

## Next Steps

- Explore [Squad Line](squad-line.md) capabilities
- [Troubleshooting](troubleshooting.md)

If you have any questions or need further assistance, please visit our [Support Center](https://support.withyoursquad.com) or contact us at support@withyoursquad.com.
