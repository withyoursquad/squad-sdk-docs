# React Native Configuration

## Custom Analytics

Pipe SDK analytics events to your own analytics provider:

```tsx
import { AnalyticsTracker } from '@squad-sports/core';

// After SDK initializes, add your adapter
AnalyticsTracker.shared.configure({
  customAdapter: (event) => {
    // Forward to Mixpanel, Amplitude, Firebase Analytics, etc.
    mixpanel.track(event.name, event.properties);
  },
});
```

## Custom Logger

Replace the default console logger with your own:

```tsx
import { Logger } from '@squad-sports/core';

Logger.shared.configure({
  minLevel: 'warn', // Only log warnings and errors
  sink: (level, message, context) => {
    // Forward to Sentry, Datadog, etc.
    Sentry.addBreadcrumb({ message, level, data: context });
  },
});
```

## Custom Storage

Provide your own encrypted storage adapter:

```tsx
<SquadExperience
  config={{
    apiKey: "...",
    environment: "production",
    community: { id: "42", name: "My Team", primaryColor: "#002B5E" },
    storage: {
      getItem: (key) => MySecureStore.get(key),
      setItem: (key, value) => MySecureStore.set(key, value),
      removeItem: (key) => MySecureStore.remove(key),
      // Optional batch operations for performance:
      multiSet: (entries) => MySecureStore.multiSet(entries),
      multiRemove: (keys) => MySecureStore.multiRemove(keys),
    },
  }}
/>
```

By default, the SDK uses `SecureStorageAdapter` which routes auth tokens through `expo-secure-store` and non-sensitive data through `AsyncStorage`.

## Environment Selection

```tsx
// Production (default)
<SquadExperience partnerId="..." apiKey="..." />

// Staging (for testing)
<SquadExperience
  config={{
    apiKey: "...",
    environment: "staging",
    community: { ... },
  }}
/>
```

## Feature Flags

All features are enabled by default and included in the standard Squad experience. Feature flags are only used when a specific partner agreement excludes a feature:

```tsx
// Only override per partner agreement — all features are on by default
<SquadExperience
  config={{
    apiKey: "...",
    environment: "production",
    community: { ... },
    features: {
      squadLine: true,    // Patented voice calls
      freestyle: true,    // Audio posts
      messaging: true,    // 1:1 messaging
      polls: true,        // Interactive polls
      events: true,       // Event attendance
      wallet: true,       // Rewards & coupons
    },
  }}
/>
```
