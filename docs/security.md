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
- 15-second request timeout (30s for uploads)
- 429 rate limit responses include `Retry-After` header
- CORS restricted to `*.withyoursquad.com` (browser requests only)

## Security Headers

All API responses include:

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
X-XSS-Protection: 1; mode=block
```

## Reporting Vulnerabilities

Email security@withyoursquad.com with details. We respond within 24 hours.
