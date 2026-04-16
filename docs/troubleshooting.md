# Troubleshooting

## Common Issues

### "API_KEY_REQUIRED" on init

Your API key is missing or invalid. Ensure you pass it to the SDK:

```tsx
<SquadExperience partnerId="your-id" apiKey="sqk_live_..." />
```

If you don't have an API key, contact your Squad partner manager.

### "PARTNER_NOT_FOUND" on init

Your partner ID doesn't match any registered partner. Check:

1. Spelling is correct (case-sensitive)
2. Your partner account is active — contact your Squad partner manager if unsure
3. You're using the correct environment (production vs staging)

### "PARTNER_MISMATCH" on provision

Your API key belongs to a different partner than the one you specified. Each API key is scoped to exactly one partner ID.

### SDK hangs on initialization

Check network connectivity. The SDK makes a provision request at init. If the network is unavailable:

- The request will timeout after 15 seconds
- `onError` callback will fire with a timeout error
- The SDK will display an error message with a "Try Again" button
- The `onError` callback is also fired so host apps can handle errors programmatically

### 429 Rate Limit Exceeded

The SDK automatically handles 429 responses by reading the `Retry-After` header and retrying. If you're seeing persistent 429s, your app may be making too many requests. Default limit is 600 requests/minute.

### Audio permission denied

Squad Line and audio messages require microphone access. Ensure your app declares the permission:

- iOS: Add `NSMicrophoneUsageDescription` to `Info.plist`
- Android: Add `RECORD_AUDIO` to `AndroidManifest.xml`

### React Native navigation conflicts

The SDK uses its own `NavigationContainer` with `independent={true}`. If you're seeing navigation issues, ensure you're not nesting `SquadExperience` inside another `NavigationContainer` without `independent` mode.

### ProGuard / R8 stripping SDK classes

Add to your `proguard-rules.pro`:

```
-keep class com.squadsports.sdk.** { *; }
-keep class withyoursquad.v2.** { *; }
```

## Debugging

### Enable debug logging

=== "React Native"

    ```tsx
    import { Logger } from '@squad-sports/core';
    Logger.shared.configure({ minLevel: 'debug' });
    ```

=== "iOS"

    ```swift
    SquadLogger.shared.minLevel = .debug
    ```

=== "Android"

    ```kotlin
    SquadLogger.minLevel = SquadLogLevel.DEBUG
    ```

### Check SDK version

The SDK sends its version on every request via the `X-Squad-SDK-Version` header. Check your network inspector to verify.

### Verify API connectivity

```bash
curl https://api-release.withyoursquad.com/health
```

Should return `{"status":"ok","checks":{"postgres":"ok","redis":"ok"}}`.

## Support

For integration support, contact your Squad partner manager or email support@squadforsports.com.
