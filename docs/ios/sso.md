# iOS SSO Integration

## Ticketmaster

```swift
let tmToken = try await TicketmasterAuth.shared.getAccessToken()

try await SquadSportsSDK.setup(
    partnerId: "acme-sports",
    apiKey: "sqk_live_...",
    ssoToken: tmToken,
    ssoProvider: .ticketmaster
)
```

## OAuth2

```swift
try await SquadSportsSDK.setup(
    partnerId: "your-id",
    apiKey: "your-key",
    ssoToken: oauthToken,
    ssoProvider: .oauth2
)
```

## Post-Init SSO

```swift
let success = await SquadSportsSDK.shared.authenticateWithSSO(
    provider: .ticketmaster,
    token: tmToken
)
```
