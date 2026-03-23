# Webhooks

Receive real-time events from Squad when actions occur in your community. Configure a webhook URL in your partner settings to get notified.

## Setup

Provide your webhook URL during onboarding or update it via the partner dashboard at [partners.squadforsports.com](https://partners.squadforsports.com).

## Event Format

All webhook events are sent as `POST` requests with a JSON body:

```json
{
  "event": "partner.user.synced",
  "partnerId": "your-partner-id",
  "timestamp": "2026-03-23T12:00:00.000Z",
  "data": {
    "userId": "12345",
    "email": "fan@example.com"
  }
}
```

### Verification

Every webhook request includes an HMAC signature in the `X-Squad-Webhook-Signature` header:

```
X-Squad-Webhook-Signature: sha256=<HMAC-SHA256 of request body>
```

Verify the signature using your **webhook signing secret** as the HMAC key. Your signing secret is available in the partner dashboard under Settings > Webhooks. This is a separate value from your API key — do not use your API key for webhook verification.

## Events

| Event | Trigger | Data |
|-------|---------|------|
| `partner.user.created` | New user created via partner-sync | `userId`, `email`, `communityId`, `isNewUser` |
| `partner.user.synced` | Existing user synced via partner-sync | `userId`, `email`, `communityId` |
| `partner.sso.exchanged` | SSO token exchanged for Squad session | `userId`, `provider`, `externalId` |

## Best Practices

- Respond with `200` within 5 seconds — events are fire-and-forget
- Use the signature header to verify authenticity
- Store events idempotently (use `timestamp` + `event` as dedup key)
- If your endpoint is down, events are not retried (use the analytics API for historical data)

## Rate Limits

Webhook events are dispatched at most once per action. There is no batching — each event is a separate request.
