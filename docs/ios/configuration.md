# iOS Configuration Guide

## Overview

This guide covers all configuration options available in the Squad SDK for iOS, including initialization, security, networking, and feature-specific settings.

## Basic Configuration

### SDK Configuration

```swift
struct SquadConfig {
    let organizationId: String
    let apiKey: String
    let environment: Environment
    let logLevel: LogLevel
}

let config = SquadConfig(
    organizationId: "YOUR_ORG_ID",
    apiKey: "YOUR_API_KEY",
    environment: .production,
    logLevel: .info
)

try SquadSDK.initialize(with: config)
```

### Environment Configuration

```swift
enum Environment {
    case production
    case staging
    case development(url: URL)
}

// Custom environment setup
let customConfig = SquadConfig(
    organizationId: "YOUR_ORG_ID",
    apiKey: "YOUR_API_KEY",
    environment: .development(url: URL(string: "https://dev-api.yourdomain.com")!),
    logLevel: .debug
)
```

## Security Configuration

### Certificate Pinning

```swift
let certificateConfig = CertificatePinningConfiguration(
    certificates: ["sha256/XXXX", "sha256/YYYY"],
    includeDefaultCertificates: true
)

SquadSDK.shared.setCertificatePinning(config: certificateConfig)
```

### User Data Protection

```swift
let securityConfig = SecurityConfiguration(
    dataEncryption: .enabled,
    biometricAuth: .optional,
    secureStorage: .keychain
)

SquadSDK.shared.setSecurityConfiguration(securityConfig)
```

## Network Configuration

### Timeouts and Retries

```swift
let networkConfig = NetworkConfiguration(
    connectionTimeout: 30,
    readTimeout: 30,
    writeTimeout: 30,
    maxRetries: 3,
    retryBackoff: .exponential(initial: 1, multiplier: 2)
)

SquadSDK.shared.setNetworkConfiguration(networkConfig)
```

### Caching Strategy

```swift
let cacheConfig = CacheConfiguration(
    diskCapacity: 50 * 1024 * 1024, // 50 MB
    memoryCapacity: 10 * 1024 * 1024, // 10 MB
    timeToLive: 3600 // 1 hour
)

SquadSDK.shared.setCacheConfiguration(cacheConfig)
```

## WebView Configuration

### Basic Setup

```swift
let webViewConfig = WebViewConfiguration(
    allowsInlineMediaPlayback: true,
    mediaPlaybackRequiresUserAction: false,
    dataDetectorTypes: [.link, .phoneNumber]
)

SquadSDK.shared.setWebViewConfiguration(webViewConfig)
```

### Content Configuration

```swift
let contentConfig = ContentConfiguration(
    allowedContentTypes: [.all],
    javascriptEnabled: true,
    domStorageEnabled: true,
    userScalable: false
)

webViewConfig.setContentConfiguration(contentConfig)
```

## Voice Call Configuration

### Audio Settings

```swift
let audioConfig = AudioConfiguration(
    audioMode: .voiceChat,
    acousticEchoCancellation: true,
    noiseSuppression: true,
    automaticGainControl: true
)

SquadSDK.shared.setAudioConfiguration(audioConfig)
```

### Call Quality

```swift
let qualityConfig = CallQualityConfiguration(
    bitrateMode: .adaptive,
    minimumBitrate: 8000,
    maximumBitrate: 32000,
    opusMode: .voip
)

SquadSDK.shared.setCallQualityConfiguration(qualityConfig)
```

## Analytics Configuration

```swift
let analyticsConfig = AnalyticsConfiguration(
    enabled: true,
    trackingEvents: [
        .calls,
        .errors,
        .performance
    ],
    customDimensions: [
        "app_version": Bundle.main.version,
        "environment": "production"
    ]
)

SquadSDK.shared.setAnalyticsConfiguration(analyticsConfig)
```

## Error Handling Configuration

```swift
let errorConfig = ErrorConfiguration(
    retryableErrors: [.network, .timeout],
    maxRetries: 3,
    errorCallback: { error in
        // Handle errors
        Logger.error("Squad SDK Error: \(error)")
    }
)

SquadSDK.shared.setErrorConfiguration(errorConfig)
```

## Resource Management

### Memory Management

```swift
let memoryConfig = MemoryConfiguration(
    lowMemoryThreshold: 50 * 1024 * 1024, // 50 MB
    criticalMemoryThreshold: 25 * 1024 * 1024, // 25 MB
    cleanupCallback: {
        // Perform cleanup
    }
)

SquadSDK.shared.setMemoryConfiguration(memoryConfig)
```

### Background Tasks

```swift
let backgroundConfig = BackgroundConfiguration(
    allowedTasks: [.voiceCalls, .messageDelivery],
    timeoutInterval: 30,
    completionHandler: { result in
        // Handle background task completion
    }
)

SquadSDK.shared.setBackgroundConfiguration(backgroundConfig)
```

## Advanced Configuration

### Custom Event Handling

```swift
let eventConfig = EventConfiguration(
    customEventHandler: { event in
        switch event {
        case .callStarted(let callId):
            // Handle call start
        case .callEnded(let callId, let duration):
            // Handle call end
        case .error(let error):
            // Handle error
        }
    }
)

SquadSDK.shared.setEventConfiguration(eventConfig)
```

### Debug Configuration

```swift
#if DEBUG
let debugConfig = DebugConfiguration(
    logLevel: .debug,
    networkLogging: true,
    performanceMetrics: true,
    screenshotEnabled: true
)

SquadSDK.shared.setDebugConfiguration(debugConfig)
#endif
```

## Best Practices

1. **Security**

   - Always enable certificate pinning in production
   - Use secure storage for sensitive data
   - Configure appropriate timeout values

2. **Performance**

   - Configure appropriate cache sizes
   - Implement memory management
   - Handle background tasks efficiently

3. **Error Handling**

   - Configure comprehensive error handling
   - Implement appropriate retry strategies
   - Log errors appropriately

4. **Testing**
   - Use staging environment for testing
   - Enable debug logging in development
   - Test with various network conditions

## Related Documentation

- [Installation Guide](installation.md)
- [WebView Management](webview.md)
- [Troubleshooting](../troubleshooting.md)
