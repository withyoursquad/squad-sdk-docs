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
    community: { id: "64", name: "DC United", primaryColor: "#ef3e42" },
    storage: {
      getItem: (key) => MySecureStore.get(key),
      setItem: (key, value) => MySecureStore.set(key, value),
      removeItem: (key) => MySecureStore.remove(key),
    },
  }}
/>
```

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

Features are controlled by your partner tier. To override for testing:

```tsx
<SquadExperience
  config={{
    apiKey: "...",
    environment: "production",
    community: { ... },
    features: {
      squadLine: true,
      freestyle: true,
      messaging: true,
      polls: true,
      events: false,   // disable events
      wallet: false,    // disable wallet
    },
  }}
/>
```
