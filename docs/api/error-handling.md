# Error Handling

## SDK Errors

| Error | Cause | Resolution |
|-------|-------|------------|
| `API_KEY_REQUIRED` | Missing apiKey in config | Provide your API key |
| `PARTNER_NOT_FOUND` | Invalid partnerId | Check your partner ID at partners.squadforsports.com |
| `PARTNER_MISMATCH` | API key doesn't match partner | Use the correct API key for your partner |
| `INVALID_TOKEN` | Auth token expired or invalid | SDK auto-handles via silent re-auth |
| `RATE_LIMIT_EXCEEDED` | Too many requests | SDK auto-retries with Retry-After |
| `Network Error` | No internet | SDK queues actions offline, retries on reconnect |

## Error Boundary (React Native)

Every screen is wrapped in `ScreenErrorBoundary`. If a screen crashes:

1. Error UI shows with "Something went wrong" message
2. "Try Again" button resets the screen
3. `onError` callback fires (from SquadExperience props)
4. Error is logged via the structured logger

## Request Retry Behavior

| Scenario | Behavior |
|----------|----------|
| 5xx error | Retry up to 3 times with exponential backoff |
| 429 rate limit | Wait for Retry-After duration, retry once |
| 401/403 (partner flow) | Silent re-auth, retry original request |
| 401/403 (no partner data) | Navigate to login screen |
| Network error | No retry (offline queue handles writes) |
| Timeout (15s) | No retry, throw error |

## Offline Support

Write operations (messages, reactions, freestyle posts) are queued when offline and automatically processed when connectivity returns.
