# iOS Configuration

## Custom Analytics

```swift
SquadAnalytics.shared.customHandler = { eventName, properties in
    Analytics.track(eventName, properties: properties)
}
```

## Custom Logger

```swift
SquadLogger.shared.sink = MyCustomLogSink()
SquadLogger.shared.minLevel = .warn
```

Implement `SquadLogSink`:

```swift
class MyCustomLogSink: SquadLogSink {
    func log(level: SquadLogLevel, message: String, context: [String: Any]) {
        Sentry.addBreadcrumb(message, level: level.rawValue, data: context)
    }
}
```

## Token Storage

Tokens are stored in the iOS Keychain by default. No configuration needed.

## Error Handling

```swift
do {
    try await SquadSportsSDK.setup(partnerId: "your-id", apiKey: "your-key")
} catch SquadSDKError.partnerNotFound(let id) {
    print("Partner \(id) not found")
} catch SquadSDKError.invalidPartnerID {
    print("Invalid partner ID")
} catch {
    print("Setup failed: \(error)")
}
```

## Cleanup

```swift
SquadSportsSDK.shared = nil
```
