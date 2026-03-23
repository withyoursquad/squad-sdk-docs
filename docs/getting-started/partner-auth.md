# Partner Auth Deep Dive

Partner auth lets you pass user data from your host app directly to the Squad SDK. The SDK creates or updates a Squad user and obtains a session token — the user never sees a login screen.

## How It Works

```
Your App                       Squad SDK                    Squad API
   │                              │                            │
   │  userData={email, name}      │                            │
   ├─────────────────────────────>│                            │
   │                              │  POST /v2/auth/partner-sync│
   │                              ├───────────────────────────>│
   │                              │                            │  find/create user
   │                              │                            │  scoped to community
   │                              │  { accessToken, userId }   │
   │                              │<───────────────────────────│
   │                              │                            │
   │  SDK ready, user logged in   │  store token (encrypted)   │
   │<─────────────────────────────│                            │
```

## Community Scoping

When partner auth is used, the user is scoped to your community. This means:

- User lookups (by email/phone) only match users in your community
- A user with the same email in a different partner's community is a different user
- If you change the community ID in config, the SDK clears the old token and re-authenticates

## Silent Re-Authentication

If a token expires mid-session, the SDK automatically:

1. Intercepts the 401/403 response
2. Calls partner-sync again with the stored userData
3. Gets a fresh token
4. Retries the original request

The user never sees an error. This only works when `userData` is provided.

## Onboarding Skip

When partner auth succeeds:

- Display name and community are auto-set from your data
- The onboarding flow (team select, name entry) is skipped entirely
- User goes directly to the home feed

## Security

- API key is required in the `X-Squad-API-Key` header
- The API key must belong to the partner making the request
- User data is validated and sanitized server-side
- Tokens are stored in encrypted device storage
- Rate limited: 600 requests/minute per partner (configurable)
