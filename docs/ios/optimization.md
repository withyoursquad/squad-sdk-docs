# iOS Build Optimization Guide

This guide covers optimization techniques for reducing app size and improving performance when integrating the Squad SDK.

## App Size Optimization

### App Thinning

Enable App Thinning in Xcode to reduce the app's size for different devices:

1. **Slicing**

```swift
// In your project settings:
// Build Settings > Build Options
ENABLE_BITCODE = NO // Since Xcode 14
STRIP_STYLE = all
DEPLOYMENT_POSTPROCESSING = YES
```

2. **On-Demand Resources**
   Configure in Xcode:

```xml
<!-- Info.plist -->
<key>NSBundleResourceRequest</key>
<dict>
    <key>NSBundleResourceRequestTag</key>
    <string>initial-resources</string>
</dict>
```

3. **Asset Catalog Optimization**

```swift
// Enable optimization in Build Settings
ASSETCATALOG_COMPILER_OPTIMIZATION = space
COMPRESS_PNG_FILES = YES
STRIP_PNG_TEXT = YES
```

### Framework Optimization

1. **Link Squad SDK Dynamically**

```ruby
# Podfile
use_frameworks! :linkage => :dynamic
```

2. **Enable Link-Time Optimization**

```swift
// Build Settings
LLVM_LTO = YES // Aggressive
```

## Build Settings Optimization

### Release Configuration

```swift
// Build Settings for Release
SWIFT_COMPILATION_MODE = wholemodule
SWIFT_OPTIMIZATION_LEVEL = -O
GCC_OPTIMIZATION_LEVEL = s
```

### Dead Code Stripping

```swift
// Enable dead code stripping
DEAD_CODE_STRIPPING = YES
STRIP_SWIFT_SYMBOLS = YES
```

## Resource Optimization

### Image Optimization

1. **Asset Catalog Settings**

```swift
// Enable compression
COMPRESS_PNG_FILES = YES
STRIP_PNG_TEXT = YES

// Enable on-demand resources
ENABLE_ON_DEMAND_RESOURCES = YES
```

2. **SVG Asset Usage**

```swift
// Use PDF vectors when possible for resolution independence
// Asset Catalog: Preserve Vector Data = YES
```

### String Optimization

```swift
// Enable string encryption
SWIFT_OBFUSCATE_STRINGS = YES // Custom build setting

// Enable constant folding
SWIFT_OPTIMIZATION_LEVEL = -O
```

## Memory Optimization

### WebView Memory Management

```swift
class SquadViewController: UIViewController {
    private func configureWebView() {
        let config = WKWebViewConfiguration()
        config.processPool = WKProcessPool()
        config.websiteDataStore = .nonPersistent()

        // Configure cache
        let cache = URLCache(
            memoryCapacity: 10 * 1024 * 1024,  // 10MB
            diskCapacity: 50 * 1024 * 1024,    // 50MB
            directory: nil
        )
        URLCache.shared = cache
    }
}
```

### Memory Warning Handling

```swift
extension SquadViewController {
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
}
```

## Network Optimization

### Caching Strategy

```swift
let cachingConfig = URLSessionConfiguration.default
cachingConfig.requestCachePolicy = .returnCacheDataElseLoad
cachingConfig.urlCache = URLCache(
    memoryCapacity: 10 * 1024 * 1024,
    diskCapacity: 50 * 1024 * 1024
)
```

### Image Loading

```swift
class ImageOptimizer {
    static func optimizedImage(_ image: UIImage) -> UIImage? {
        let data = image.jpegData(compressionQuality: 0.7)
        return data.flatMap(UIImage.init)
    }
}
```

## Debug Optimization

### Development vs Release

```swift
#if DEBUG
// Development-only code
#else
// Release-only optimizations
#endif
```

### Logging Control

```swift
struct OptimizedLogger {
    static func log(_ message: String) {
        #if DEBUG
        print(message)
        #endif
    }
}
```

## Measurement & Verification

### Size Analysis

1. Check app size:

```bash
xcrun devicespacecraft -size "App.ipa"
```

2. Analyze binary:

```bash
xcrun size -m "YourApp.app/YourApp"
```

### Performance Metrics

```swift
class PerformanceMonitor {
    static func measureMemory() {
        var info = mach_task_basic_info()
        var count = mach_msg_type_number_t(MemoryLayout<mach_task_basic_info>.size)/4

        let kerr: kern_return_t = withUnsafeMutablePointer(to: &info) {
            $0.withMemoryRebound(to: integer_t.self, capacity: 1) {
                task_info(
                    mach_task_self_,
                    task_flavor_t(MACH_TASK_BASIC_INFO),
                    $0,
                    &count
                )
            }
        }

        if kerr == KERN_SUCCESS {
            print("Memory used: \(info.resident_size / 1024 / 1024) MB")
        }
    }
}
```

## Best Practices

1. **Build Settings**

   - Enable optimization flags
   - Configure proper architecture settings
   - Enable dead code stripping

2. **Resource Management**

   - Optimize image assets
   - Enable App Thinning
   - Configure caching properly

3. **Memory Management**

   - Handle memory warnings
   - Implement proper cleanup
   - Monitor memory usage

4. **Network Optimization**
   - Configure proper caching
   - Optimize request handling
   - Monitor network usage

## Troubleshooting

### Common Issues

1. **Large App Size**

   - Check embedded frameworks
   - Analyze resource usage
   - Verify optimization settings

2. **Memory Issues**
   - Monitor memory warnings
   - Check for leaks
   - Verify cleanup implementation

## Related Documentation

- [Installation Guide](installation.md)
- [Configuration Guide](configuration.md)
- [WebView Management](webview.md)
