# Webhooks

Receive real-time events from Squad when actions occur in your community.

## Setup

Webhook URLs are configured by your Squad partner manager. Provide your endpoint URL during onboarding. To add or update a webhook destination after go-live, email [support@squadforsports.com](mailto:support@squadforsports.com) or contact your partner manager directly.

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

Verify the signature using your **webhook signing secret** as the HMAC key. Your signing secret is issued by your Squad partner manager alongside your API key during onboarding. This is a separate value from your API key — do not use your API key for webhook verification.

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
