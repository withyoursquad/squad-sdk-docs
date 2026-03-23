# Authentication

The SDK supports three authentication methods. Choose the one that fits your app.

## 1. OTP (Default)

Users enter their email or phone number, receive a 6-digit code, and verify. This is the default if no SSO or partner auth is configured.

No extra setup needed — just initialize the SDK and the auth screens are built in.

## 2. SSO (Ticketmaster, OAuth2, Custom)

Exchange an external identity token for a Squad session. Users already logged into your app are auto-authenticated.

=== "React Native"

    ```tsx
    <SquadExperience
      partnerId="your-id"
      apiKey="your-key"
      ssoToken={externalAccessToken}
      ssoProvider="ticketmaster"  // or "oauth2" or "custom"
    />
    ```

=== "iOS"

    ```swift
    try await SquadSportsSDK.setup(
        partnerId: "your-id",
        apiKey: "your-key",
        ssoToken: externalAccessToken,
        ssoProvider: .ticketmaster
    )
    ```

=== "Android"

    ```kotlin
    SquadSportsSDK.setup(
        context = this,
        partnerId = "your-id",
        apiKey = "your-key",
        ssoToken = externalAccessToken,
        ssoProvider = SSOProvider.TICKETMASTER,
    )
    ```

The SDK exchanges the token via `POST /v2/auth/sso/:provider` and obtains a Squad access token.

## 3. Partner Auth (Seamless, No Login Screen)

Pass user data directly from your app. The SDK creates or syncs a Squad user via `POST /v2/auth/partner-sync`. Users never see a login screen.

=== "React Native"

    ```tsx
    <SquadExperience
      partnerId="your-id"
      apiKey="your-key"
      userData={{
        email: "fan@dcunited.com",
        displayName: "Alex Fan",
        externalUserId: "your-internal-user-id",
      }}
    />
    ```

=== "iOS"

    ```swift
    try await SquadSportsSDK.setup(
        partnerId: "your-id",
        apiKey: "your-key",
        userData: PartnerUserData(
            email: "fan@dcunited.com",
            displayName: "Alex Fan",
            externalUserId: "your-internal-user-id"
        )
    )
    ```

=== "Android"

    ```kotlin
    SquadSportsSDK.setup(
        context = this,
        partnerId = "your-id",
        apiKey = "your-key",
        userData = PartnerUserData(
            email = "fan@dcunited.com",
            displayName = "Alex Fan",
            externalUserId = "your-internal-user-id",
        ),
    )
    ```

### PartnerUserData Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `email` | string | one of email/phone/externalUserId | User's email |
| `phone` | string | one of email/phone/externalUserId | E.164 format (+15551234567) |
| `displayName` | string | optional | Shown in the UI |
| `avatarUrl` | string | optional | Profile picture URL |
| `externalUserId` | string | one of email/phone/externalUserId | Your internal user ID |

At least one identifier (`email`, `phone`, or `externalUserId`) is required.

## Token Lifecycle

- Tokens are stored in encrypted storage (Keychain on iOS, EncryptedSharedPreferences on Android, expo-secure-store on RN)
- Sessions persist across app launches — returning users are auto-authenticated
- On 401/403, the SDK attempts silent re-authentication (for partner auth flows) before showing a login screen
- Community scoping: if you change the community ID, previous tokens are cleared and the user re-authenticates
