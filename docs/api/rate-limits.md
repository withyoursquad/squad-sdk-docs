# Rate Limits

All SDK requests are rate-limited per API key. Limits are configurable per partner tier.

## Limits by Tier

| Tier | Requests/Minute | Requests/Day | Features |
|------|-----------------|--------------|----------|
| **Standard** | 600 | 100,000 | All core features |
| **Premium** | 1,200 | 500,000 | All features + priority support |
| **Enterprise** | Custom | Custom | All features + SLA + dedicated support |

## Rate Limit Headers

Every API response includes rate limit information:

```
X-RateLimit-Limit: 600
X-RateLimit-Remaining: 542
Retry-After: 12          (only on 429 responses)
```

## 429 Handling

When the rate limit is exceeded, the API returns `429 Too Many Requests`:

```json
{
  "error": "RATE_LIMIT_EXCEEDED",
  "message": "Rate limit exceeded. Max 600 requests per minute."
}
```

The SDK automatically handles this:

1. Reads the `Retry-After` header
2. Waits the specified duration (capped at 30 seconds)
3. Retries the request once

Your app code does not need to handle 429 responses.

## OTP Rate Limits

Verification code requests have additional per-user limits to prevent abuse:

| Window | Limit | Scope |
|--------|-------|-------|
| Per minute | 3 attempts | Per phone number or email |
| Per hour | 10 attempts | Per phone number or email |
| Client-side | 60-second cooldown | Between resend attempts |

These limits are independent of your API key rate limit.

## Tips

- The SDK batches analytics events (up to 20 per flush) to minimize request count
- Cache responses client-side where possible (the SDK handles this automatically)
- If you need higher limits, contact your partner manager to upgrade your tier
