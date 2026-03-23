# Changelog

## 0.1.0 — 2026-03-23 (Partner Release)

Initial production release for partner integrations.

### All Platforms
- Partner auth flow: pass user data for seamless authentication (no login screen)
- SSO integration: Ticketmaster, OAuth2, custom providers
- API key required for SDK initialization
- 15s request timeout (30s for uploads)
- 429 rate limit retry with Retry-After header
- Silent partner re-auth on 401/403
- Structured logger with pluggable sinks
- Analytics tracker with custom adapter support
- SDK version header on all requests
- App lifecycle handling (flush analytics on background, reconnect on foreground)
- 60-second verification code resend cooldown

### React Native
- `<SquadExperience>` drop-in component
- ScreenErrorBoundary on every navigator screen
- Encrypted token storage via expo-secure-store
- EventProcessor with TTL-based deduplication
- Network monitoring + offline queue

### iOS
- `SquadSportsSDK.setup(partnerId:apiKey:)` one-line init
- `SquadExperienceViewController` for UIKit embedding
- Keychain token storage
- URLSession-based API client with retry logic
- SwiftUI screens with community theming

### Android
- `SquadSportsSDK.setup(context, partnerId, apiKey)` one-line init
- `SquadExperienceActivity` for Activity embedding
- Compose UI screens with Material3 theming
- EncryptedSharedPreferences token storage
- OkHttp interceptors for timeout, retry, auth
- ProcessLifecycleOwner for background/foreground handling

### Backend (6.17.0)
- `GET /health` endpoint with postgres/redis checks
- Redis-backed rate limiting (survives restarts)
- Redis-backed auth cache (distributed invalidation)
- Partner provisioning endpoint (API key required)
- Cross-partner data isolation on SSO and partner-sync
- Security headers on all responses
- OTP rate limiting (3/min, 10/hour per identifier)
- Sanitized error responses (no PII leakage)
- Cryptographically secure login code generation
