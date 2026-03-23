# Analytics

The SDK automatically tracks lifecycle events and sends them to the partner analytics endpoint. You can also pipe events to your own analytics provider.

## Auto-Tracked Events

| Event | When |
|-------|------|
| `sdk_initialized` | After setup() completes |
| `session_restored` | Returning user's session is valid |
| `partner_sync_success` | Partner auth succeeded |
| `partner_sync_failed` | Partner auth failed |
| `sso_login` | SSO token exchange succeeded |
| `screen_view` | User navigates to a new screen |
| `freestyle_created` | User posts an audio freestyle |
| `message_sent` | User sends a message |
| `poll_voted` | User votes on a poll |
| `call_started` | Voice call begins |
| `call_ended` | Voice call ends |
| `invite_shared` | User shares an invite |
| `coupon_redeemed` | User redeems a coupon |
| `profile_updated` | User updates their profile |

## Custom Analytics Adapter

=== "React Native"

    ```tsx
    import { AnalyticsTracker } from '@squad-sports/core';

    AnalyticsTracker.shared.configure({
      customAdapter: (event) => {
        mixpanel.track(event.name, event.properties);
      },
    });
    ```

=== "iOS"

    ```swift
    SquadAnalytics.shared.customHandler = { name, props in
        Mixpanel.track(name, properties: props)
    }
    ```

=== "Android"

    ```kotlin
    SquadAnalytics.customHandler = { name, props ->
        Mixpanel.track(name, props)
    }
    ```

## Event Properties

Every event includes:

| Property | Description |
|----------|-------------|
| `userId` | Squad user ID |
| `partnerId` | Your partner ID |
| `timestamp` | Unix timestamp (ms) |

Plus event-specific properties (screen name, call duration, etc.).

## Flush Behavior

- Events are batched and flushed every 30 seconds
- Auto-flush when batch reaches 20 events
- Flush on app background
- Failed flushes are re-queued
