# React Native Quick Start

## Installation

```bash
# Core SDK
yarn add @squad-sports/core @squad-sports/react-native

# Required peer dependencies
yarn add @react-navigation/native @react-navigation/native-stack \
  react-native-screens react-native-safe-area-context \
  react-native-gesture-handler react-native-reanimated \
  @react-native-async-storage/async-storage \
  @gorhom/bottom-sheet recoil \
  expo-av expo-image

# Recommended: encrypted token storage
yarn add expo-secure-store
```

## Basic Integration

```tsx
import { SquadExperience } from '@squad-sports/react-native';

export default function App() {
  return (
    <SquadExperience
      partnerId="yinzcam-dc-united"
      apiKey="sqk_live_..."
      onReady={() => console.log('Squad ready')}
      onError={(err) => console.error('Squad error:', err)}
    />
  );
}
```

That's it. The SDK handles navigation, auth, theming, and all features.

## With Partner Auth (No Login Screen)

```tsx
<SquadExperience
  partnerId="yinzcam-dc-united"
  apiKey="sqk_live_..."
  userData={{
    email: currentUser.email,
    displayName: currentUser.name,
    externalUserId: currentUser.id,
  }}
/>
```

## With Ticketmaster SSO

```tsx
<SquadExperience
  partnerId="yinzcam-dc-united"
  apiKey="sqk_live_..."
  ssoToken={ticketmasterAccessToken}
  ssoProvider="ticketmaster"
/>
```

## Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `partnerId` | string | yes* | Your partner ID |
| `apiKey` | string | yes* | Your API key |
| `userData` | PartnerUserData | no | User data for seamless auth |
| `ssoToken` | string | no | External SSO token |
| `ssoProvider` | string | no | `"ticketmaster"`, `"oauth2"`, `"custom"` |
| `config` | SquadSDKConfig | yes* | Full config (alternative to partnerId) |
| `onReady` | () => void | no | Called when SDK is initialized |
| `onError` | (error: Error) => void | no | Called on init failure |

*Either `partnerId` + `apiKey` or `config` is required.

## Embedding in an Existing App

If Squad is one tab in your app (not the root):

```tsx
import { SquadExperience } from '@squad-sports/react-native';

function SquadTab() {
  return (
    <SquadExperience
      partnerId="yinzcam-dc-united"
      apiKey="sqk_live_..."
      userData={{ email: user.email, displayName: user.name }}
    />
  );
}

// In your tab navigator:
<Tab.Screen name="Squad" component={SquadTab} />
```

The SDK uses its own `NavigationContainer` with `independent={true}`, so it won't conflict with your app's navigation.
