# Squad Line

Squad Line is a patented interactive voice calling system built into the Squad Sports SDK. It enables real-time voice conversations between fans with unique interactive features that go beyond a standard voice call.

**Patent Status**: Squad Line's interactive calling system — including custom call titles, real-time floating emoji reactions, and contextual call experiences — is covered by pending patent applications.

## How It Works

Squad Line allows fans to call each other directly within the app. Unlike standard VoIP, Squad Line calls include:

- **Custom Call Titles** — Callers set a topic or title for the call (e.g., "Did you see that play?!", "Pre-game predictions"). The title is displayed to the recipient before they answer.
- **Floating Emoji Reactions** — During a call, both participants can send real-time emoji reactions that animate across the screen. This creates a visual, interactive layer on top of voice.
- **Call Context** — Calls are tied to the community context (your team), so the UI is themed to match.

## Architecture

```
Caller                          Recipient
  │                                │
  │  1. Set call title             │
  │  2. Initiate call ─────────>   │  3. Incoming call overlay
  │                                │     (shows title + caller)
  │                                │  4. Accept / Decline
  │  <──── Voice connected ─────>  │
  │                                │
  │  5. Send emoji ──────────────> │  6. Emoji animates on screen
  │  <───────────── Send emoji ──  │
  │                                │
  │  7. End call                   │
```

The voice infrastructure uses Twilio under the hood for reliable, low-latency audio. The interactive layer (titles, emoji reactions, call state) is managed by Squad's own real-time system.

## Integration

Squad Line is included in the SDK by default. No additional setup is required beyond the standard SDK initialization.

### Permissions

Squad Line requires microphone access. See [Requirements](../getting-started/requirements.md#required-permissions) for the required permission declarations.

### Push Notifications

Incoming call notifications are delivered via push. When a call comes in while the app is backgrounded, the SDK displays a system notification. See [Push Notifications](../getting-started/push-notifications.md) for setup.

## Call Lifecycle Events

The following analytics events are tracked automatically:

| Event | When |
|-------|------|
| `call_started` | Call is connected |
| `call_ended` | Call ends (includes duration) |
| `reaction_sent` | Emoji reaction sent during call |

These events are available in your partner analytics dashboard and via the custom analytics adapter.
