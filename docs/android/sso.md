# Android SSO Integration

## Ticketmaster

```kotlin
val tmToken = TicketmasterAuth.getAccessToken()

SquadSportsSDK.setup(
    context = this,
    partnerId = "yinzcam-dc-united",
    apiKey = "sqk_live_...",
    ssoToken = tmToken,
    ssoProvider = SSOProvider.TICKETMASTER,
)
```

## OAuth2

```kotlin
SquadSportsSDK.setup(
    context = this,
    partnerId = "your-id",
    apiKey = "your-key",
    ssoToken = oauthToken,
    ssoProvider = SSOProvider.OAUTH2,
)
```
