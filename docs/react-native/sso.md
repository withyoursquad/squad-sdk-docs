# React Native SSO Integration

## Ticketmaster SSO

If your app uses Ticketmaster authentication, pass the TM access token and Squad handles the rest.

```tsx
// Get the TM access token from your Ticketmaster SDK
const tmToken = await TicketmasterAuth.getAccessToken();

<SquadExperience
  partnerId="acme-sports"
  apiKey="sqk_live_..."
  ssoToken={tmToken}
  ssoProvider="ticketmaster"
/>
```

The SDK exchanges the TM token for a Squad session via `POST /v2/auth/sso/ticketmaster`.

## OAuth2 SSO

For generic OAuth2 providers:

```tsx
<SquadExperience
  partnerId="your-id"
  apiKey="your-key"
  ssoToken={oauthAccessToken}
  ssoProvider="oauth2"
/>
```

## Post-Init SSO

If the SSO token isn't available at mount time, authenticate after init:

```tsx
import { SquadSportsSDK } from '@squad-sports/react-native';

// Later, when token becomes available:
const success = await SquadSportsSDK.shared.authenticateWithSSO(
  'ticketmaster',
  tmAccessToken
);
```

## SSO + Partner Auth

You can combine SSO with partner user data for the richest experience:

```tsx
<SquadExperience
  partnerId="acme-sports"
  apiKey="sqk_live_..."
  ssoToken={tmToken}
  ssoProvider="ticketmaster"
  userData={{
    email: user.email,
    displayName: user.name,
    avatarUrl: user.avatar,
  }}
/>
```

This auto-authenticates via SSO and enriches the Squad profile with your user data.
