# iOS Troubleshooting Guide

This guide addresses iOS-specific issues and solutions when integrating the Squad SDK.

## Installation Issues

### Swift Package Manager

#### Issue: Package Resolution Failures

```
Error: Unable to resolve package dependencies
```

**Solutions:**

1. Clear package caches:

```bash
rm -rf ~/Library/Caches/org.swift.swiftpm/
```

2. Reset package resolution:

```bash
File > Packages > Reset Package Caches
```

### CocoaPods

#### Issue: Pod Installation Failures

```
Error: Unable to find a specification for 'SquadSDK'
```

**Solutions:**

1. Update CocoaPods repo:

```bash
pod repo update
```

2. Clear CocoaPods cache:

```bash
pod cache clean 'SquadSDK'
```

## Build Issues

### Bitcode Errors

#### Issue: Bitcode Compatibility

```
Error: Failed to build with bitcode enabled
```

**Solutions:**

1. Disable Bitcode in build settings:

```
ENABLE_BITCODE = NO
```

2. Update project configuration:

```ruby
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['ENABLE_BITCODE'] = 'NO'
    end
  end
end
```

### Architecture Issues

#### Issue: Architecture Mismatch

```
Error: Building for iOS Simulator, but linking against dylib built for iOS
```

**Solutions:**

1. Check target architectures:

```
VALID_ARCHS = arm64 x86_64
```

2. Configure build settings:

```
ONLY_ACTIVE_ARCH = YES # Debug
ONLY_ACTIVE_ARCH = NO  # Release
```

## WebView Issues

### WKWebView Loading

#### Issue: Content Loading Failures

```
Error: WKWebView failed to load content
```

**Solutions:**

1. Configure WKWebView properly:

```swift
let config = WKWebViewConfiguration()
config.allowsInlineMediaPlayback = true
config.mediaTypesRequiringUserActionForPlayback = []

let webView = WKWebView(frame: .zero, configuration: config)
webView.navigationDelegate = self
```

2. Handle load errors:

```swift
func webView(_ webView: WKWebView, didFailProvisionalNavigation: WKNavigation!, withError error: Error) {
    switch (error as NSError).code {
    case NSURLErrorNotConnectedToInternet:
        handleOfflineState()
    case NSURLErrorTimedOut:
        retryLoading()
    default:
        handleGenericError(error)
    }
}
```

### JavaScript Bridge

#### Issue: Bridge Communication Failures

```
Error: Unable to communicate with JavaScript bridge
```

**Solutions:**

1. Verify bridge setup:

```swift
let userContentController = WKUserContentController()
userContentController.add(self, name: "squadBridge")

let config = WKWebViewConfiguration()
config.userContentController = userContentController
```

2. Debug bridge messages:

```swift
func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {
    print("Bridge message received: \(message.body)")
    // Handle message
}
```

## Audio Issues

### AVAudioSession

#### Issue: Audio Session Configuration

```
Error: Failed to activate audio session
```

**Solutions:**

1. Configure audio session:

```swift
do {
    try AVAudioSession.sharedInstance().setCategory(
        .playAndRecord,
        mode: .voiceChat,
        options: [.allowBluetooth, .allowBluetoothA2DP]
    )
    try AVAudioSession.sharedInstance().setActive(true)
} catch {
    print("Audio session configuration failed: \(error)")
}
```

2. Handle interruptions:

```swift
NotificationCenter.default.addObserver(
    self,
    selector: #selector(handleAudioInterruption),
    name: AVAudioSession.interruptionNotification,
    object: nil
)
```

### Permissions

#### Issue: Microphone Access

```
Error: Microphone access denied
```

**Solutions:**

1. Check permission status:

```swift
AVAudioSession.sharedInstance().requestRecordPermission { granted in
    if granted {
        // Handle granted permission
    } else {
        // Handle denied permission
    }
}
```

2. Add usage description to Info.plist:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Squad needs microphone access for voice calls</string>
```

## Memory Management

### Memory Warnings

#### Issue: Excessive Memory Usage

```
Error: Received memory warning
```

**Solutions:**

1. Implement memory warning handler:

```swift
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()

    // Clear image caches
    URLCache.shared.removeAllCachedResponses()

    // Clear WebView cache
    WKWebsiteDataStore.default().removeData(
        ofTypes: [.memoryCache],
        modifiedSince: .distantPast
    ) { }
}
```

2. Monitor memory usage:

```swift
class MemoryMonitor {
    static func logMemoryUsage() {
        var info = mach_task_basic_info()
        var count = mach_msg_type_number_t(MemoryLayout<mach_task_basic_info>.size)/4

        let kerr: kern_return_t = withUnsafeMutablePointer(to: &info) {
            $0.withMemoryRebound(to: integer_t.self, capacity: 1) {
                task_info(mach_task_self_, task_flavor_t(MACH_TASK_BASIC_INFO), $0, &count)
            }
        }

        if kerr == KERN_SUCCESS {
            let memoryUsedMB = Double(info.resident_size) / 1024.0 / 1024.0
            print("Memory used: \(memoryUsedMB) MB")
        }
    }
}
```

## Debug Tools

### Network Debugging

1. Enable network logging:

```swift
SquadSDK.enableNetworkDebugging(true)
```

2. Monitor requests:

```swift
URLSession.shared.configuration.protocolClasses = [DebugProtocol.self]
```

### Console Logging

Configure logging levels:

```swift
SquadSDK.setLogLevel(
    .debug,
    categories: [.network, .webView, .audio]
)
```

## Related Resources

- [General Troubleshooting Guide](../troubleshooting.md)
- [iOS Configuration Guide](configuration.md)
- [iOS WebView Management](webview.md)
- [iOS Build Optimization](optimization.md)

## Support

When contacting support, provide:

- Xcode version
- iOS version
- Device model
- SDK version
- Error logs
- Steps to reproduce
- Sample project (if possible)
