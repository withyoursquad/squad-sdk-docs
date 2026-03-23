# SDK Config Reference

## SquadPartnerConfig (Recommended)

The simple config — provide your partner ID and API key, everything else auto-resolves.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `partnerId` | string | yes | Your partner ID from onboarding |
| `apiKey` | string | yes | Your API key from partners.squadforsports.com |
| `environment` | string | no | `"production"` (default), `"staging"`, `"development"` |
| `ssoToken` | string | no | External SSO access token |
| `ssoProvider` | string | no | `"ticketmaster"`, `"oauth2"`, `"custom"` |
| `userData` | PartnerUserData | no | User data for seamless auth |
| `storage` | StorageAdapter | no | Custom storage (default: encrypted) |

## PartnerUserData

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `email` | string | one of * | User's email address |
| `phone` | string | one of * | E.164 format (+15551234567) |
| `displayName` | string | no | Display name in Squad |
| `avatarUrl` | string | no | Profile picture URL |
| `externalUserId` | string | one of * | Your internal user ID |

*At least one of `email`, `phone`, or `externalUserId` is required.

## SquadSDKConfig (Full Control)

For integrators who want manual control over every setting.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `apiKey` | string | yes | API key |
| `environment` | string | yes | Target environment |
| `community` | CommunityConfig | yes | Community branding |
| `features` | FeatureFlags | no | Feature overrides |
| `sso` | SSOConfig | no | SSO configuration |
| `partnerAuth` | object | no | Partner auth context |
| `storage` | StorageAdapter | no | Custom storage |
| `apiBaseUrl` | string | no | Override API base URL |

## CommunityConfig

| Field | Type | Required |
|-------|------|----------|
| `id` | string | yes |
| `name` | string | yes |
| `primaryColor` | string | yes |
| `secondaryColor` | string | no |

## FeatureFlags

All default to `true`. Set `false` to disable.

| Flag | Description |
|------|-------------|
| `squadLine` | Voice calls |
| `freestyle` | Audio posts |
| `messaging` | 1:1 messaging |
| `polls` | Interactive polls |
| `events` | Event attendance |
| `wallet` | Rewards & coupons |
