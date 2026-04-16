# Error Handling

## SDK Errors

| Error | Cause | Resolution |
|-------|-------|------------|
| `API_KEY_REQUIRED` | Missing apiKey in config | Provide your API key |
| `PARTNER_NOT_FOUND` | Invalid partnerId | Verify your partner ID with your Squad partner manager |
| `PARTNER_MISMATCH` | API key doesn't match partner | Use the correct API key for your partner |
| `INVALID_TOKEN` | Auth token expired or invalid | SDK auto-handles via silent re-auth |
| `RATE_LIMIT_EXCEEDED` | Too many requests | SDK auto-retries with Retry-After |
| `Network Error` | No internet | SDK queues actions offline, retries on reconnect |

## Initialization Errors (React Native)

If SDK initialization fails (bad API key, network error, invalid partnerId), `SquadExperience` renders an error card with:

1. The error message (so integrators can diagnose the issue)
2. A "Try Again" button that re-attempts initialization
3. The `onError` callback fires with the error details

## Screen Error Boundary (React Native)

Every screen is wrapped in `ScreenErrorBoundary`. If a screen crashes after init:

1. Error UI shows with "Something went wrong" message
2. "Try Again" button resets the screen
3. `onError` callback fires (from SquadExperience props)
4. Error is logged via the structured logger

## Request Retry Behavior

| Scenario | Behavior |
|----------|----------|
| 5xx error | Retry up to 3 times with exponential backoff |
| 429 rate limit | Wait for Retry-After duration, retry once |
| 401/403 (partner or SSO flow) | Silent re-auth (partner sync first, then SSO token re-exchange), retry original request |
| 401/403 (OTP session, no partner/SSO) | Navigate to login screen |
| Network error | No retry (offline queue handles writes) |
| Timeout (15s) | No retry, throw error |

## Offline Support

Write operations (messages, reactions, freestyle posts) are queued when offline and automatically processed when connectivity returns.
