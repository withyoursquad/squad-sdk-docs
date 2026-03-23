# Overview

The Squad Sports SDK is a drop-in social experience for sports apps. It provides a complete, themed UI with community features that match your team's branding.

## Architecture

```
Your App
  └── SquadExperience (single component)
        ├── Auth (OTP, SSO, Partner Auth)
        ├── Home Feed (Freestyles, Polls)
        ├── Messaging (1:1, Audio)
        ├── Squad Line (Voice Calls)
        ├── Events & Attendance
        └── Wallet & Rewards
```

The SDK handles everything: authentication, navigation, real-time updates, offline queuing, and analytics. You provide a `partnerId` and `apiKey` — the SDK auto-resolves your community config, colors, features, and branding from our partner registry.

## Integration Levels

### Level 1: Drop-in (Recommended)

Just provide your partner ID and API key. The SDK resolves everything else automatically.

```tsx
<SquadExperience partnerId="your-id" apiKey="your-key" />
```

### Level 2: With SSO

Pass your existing auth token (Ticketmaster, OAuth, etc.) and users are auto-authenticated — no separate login required.

```tsx
<SquadExperience
  partnerId="your-id"
  apiKey="your-key"
  ssoToken={ticketmasterAccessToken}
  ssoProvider="ticketmaster"
/>
```

### Level 3: Partner Auth (Seamless)

Pass user data from your app. The SDK creates/syncs a Squad user automatically — users never see a login screen.

```tsx
<SquadExperience
  partnerId="your-id"
  apiKey="your-key"
  userData={{
    email: user.email,
    displayName: user.name,
    externalUserId: user.id,
  }}
/>
```

### Level 4: Full Control

Provide every config value manually. Use this if you need custom environments, storage, or base URLs.

```tsx
<SquadExperience
  config={{
    apiKey: "your-key",
    environment: "production",
    community: { id: "42", name: "My Team", primaryColor: "#002B5E" },
  }}
/>
```

## What Happens at Init

1. SDK calls `GET /v2/partners/:partnerId/provision` with your API key (sent via `X-Squad-API-Key` header)
2. Server returns community config, features, SSO settings, branding
3. SDK builds full config automatically
4. Session is restored from encrypted storage (if returning user)
5. If SSO token or user data provided, user is auto-authenticated
6. Analytics tracker is configured
7. Real-time SSE connection is established
8. UI renders with your team's colors and branding
