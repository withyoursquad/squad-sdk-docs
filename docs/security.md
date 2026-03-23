# Security

## API Key Security

- Your API key authenticates all SDK requests
- Keys are hashed (SHA-256) before storage — the raw key is never in our database
- Each key is scoped to one partner — no cross-partner access
- Keys can be revoked instantly via the partner dashboard
- Rate limited: 600 requests/minute per key (configurable per tier)

**Never commit your API key to version control.** Use environment variables or a secrets manager.

## Token Storage

| Platform | Storage | Encryption |
|----------|---------|------------|
| React Native | expo-secure-store | Hardware-backed keystore |
| iOS | Keychain | AES-256-GCM (Secure Enclave) |
| Android | EncryptedSharedPreferences | Android Keystore (AES-256) |

Fallback: AsyncStorage (RN) or SharedPreferences (Android) if encrypted storage is unavailable. Only non-sensitive data uses the fallback.

The React Native SDK uses a `SecureStorageAdapter` that routes keys based on sensitivity:

**Encrypted (expo-secure-store):** access tokens, user IDs, email, phone, community ID, partner ID

**Plain (AsyncStorage):** navigation state, UI preferences, non-auth data

Partners can provide a custom `StorageAdapter` via the `storage` config option if their app has its own encrypted storage solution.

## Transport Security

- All API communication over HTTPS (TLS 1.2+)
- HSTS header enforced (`max-age=31536000`)
- SDK rejects non-HTTPS base URLs

## Partner Isolation

- User lookups are scoped to your community — partner A cannot see partner B's users
- Analytics events are scoped — you can only write to your own partner analytics
- Provision endpoint verifies API key matches the requested partner

## OTP Security

- Codes generated with `crypto.randomBytes()` (cryptographically secure)
- Rate limited: 3 attempts per minute, 10 per hour per phone/email
- Codes expire after 10 minutes
- 60-second cooldown between resend attempts (client-enforced)

## Request Security

- SDK version header sent on every request (`X-Squad-SDK-Version`)
- Unique `X-Request-ID` on every response (for tracing and support)
- 15-second request timeout (30s for uploads)
- 429 rate limit responses include `Retry-After` header
- CORS restricted to authorized origins (browser requests only)

## Security Headers

All API responses include:

```
X-Request-ID: <unique-uuid>
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
X-XSS-Protection: 1; mode=block
```

Include the `X-Request-ID` value when contacting support — it allows us to trace the exact request in our logs.

## User Consent & GDPR

The SDK tracks analytics events by default. If your app requires GDPR/CCPA consent before collecting data, disable analytics until consent is granted:

=== "React Native"

    ```tsx
    import { AnalyticsTracker } from '@squad-sports/core';

    // Disable tracking until user consents
    AnalyticsTracker.shared.setEnabled(false);

    // After user grants consent:
    AnalyticsTracker.shared.setEnabled(true);
    ```

=== "iOS"

    ```swift
    // Disable until consent
    SquadAnalytics.shared.setEnabled(false)

    // After consent
    SquadAnalytics.shared.setEnabled(true)
    ```

=== "Android"

    ```kotlin
    // Disable until consent
    SquadAnalytics.setEnabled(false)

    // After consent
    SquadAnalytics.setEnabled(true)
    ```

When disabled, no analytics events are collected or sent. SDK functionality is unaffected.

### User Data Rights

Users can exercise their data rights through:

- **Access**: User data is visible in the Profile and Settings screens within the SDK
- **Deletion**: Users can request account deletion from Settings. Partners can also call `DELETE /v2/partners/:partnerId/users/:userId`
- **Portability**: Contact support@squadforsports.com for data export requests

## API Key Rotation

If your API key is compromised:

1. **Revoke immediately** via the partner dashboard (Settings > API Keys > Revoke)
2. **Generate a new key** — a new key is active instantly
3. **Update your app** — replace the old key in your SDK config and deploy
4. Active SDK sessions using the revoked key will receive 401 errors and redirect to login. There is no grace period — revocation is immediate.

## Reporting Vulnerabilities

Email security@squadforsports.com with details. We respond within 24 hours.
