# Android SSO Integration

## Ticketmaster

```kotlin
val tmToken = TicketmasterAuth.getAccessToken()

SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
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

## Post-Init SSO

If the SSO token isn't available at setup time, authenticate after initialization:

```kotlin
// Later, when token becomes available:
val success = SquadSportsSDK.shared?.authenticateWithSSO(
    provider = SSOProvider.TICKETMASTER,
    token = tmToken,
)
```

## SSO + Partner Auth

Combine SSO with partner user data for the richest experience:

```kotlin
SquadSportsSDK.setup(
    context = this,
    partnerId = "acme-sports",
    apiKey = "sqk_live_...",
    ssoToken = tmToken,
    ssoProvider = SSOProvider.TICKETMASTER,
    userData = PartnerUserData(
        email = user.email,
        displayName = user.name,
        avatarUrl = user.avatar,
    ),
)
```

This auto-authenticates via SSO and enriches the Squad profile with your user data.
