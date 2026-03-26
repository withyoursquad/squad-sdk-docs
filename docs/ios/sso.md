# iOS SSO Integration

## Ticketmaster

```swift
let tmToken = try await TicketmasterAuth.shared.getAccessToken()

try await SquadSDK.setup(
    partnerId: "acme-sports",
    apiKey: "sqk_live_...",
    ssoToken: tmToken,
    ssoProvider: .ticketmaster
)
```

## OAuth2

```swift
try await SquadSDK.setup(
    partnerId: "your-id",
    apiKey: "your-key",
    ssoToken: oauthToken,
    ssoProvider: .oauth2
)
```

## Post-Init SSO

```swift
let success = await SquadSDK.shared.authenticateWithSSO(
    provider: .ticketmaster,
    token: tmToken
)
```
