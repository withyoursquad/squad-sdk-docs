# Features Overview

The Squad Sports SDK includes a full suite of fan engagement features. Each feature is built-in and themed to your team's colors — no additional UI work needed.

## Community Feed

The home screen. Fans post audio **Freestyles** (short voice recordings) and react to each other's content. Freestyles can be prompted (daily questions from your community) or free-form.

- Audio recording with waveform visualization
- Emoji reactions with community-wide reaction counts
- Community-prompted freestyles on a daily schedule
- Feed pagination with pull-to-refresh

## Messaging

1:1 messaging between connected fans (squad members).

- Text and audio messages
- Message reactions
- Real-time delivery via SSE
- Offline queue (messages sent when connectivity returns)
- Conversation list with unread indicators

## Squad Line

Patented interactive voice calling. See [Squad Line](squad-line.md) for details.

- Custom call titles
- Real-time floating emoji reactions during calls
- Incoming call overlay with caller info
- Full-duplex voice via Twilio infrastructure

## Polls

Interactive polls with live results and social nudging.

- Multiple choice questions
- Real-time vote counts
- Nudge friends to vote
- Poll response reactions
- Scheduled polls with active/expired states

## Events

Game-day event attendance and check-ins.

- Event listing with date/time
- RSVP / attendance tracking
- Attendee list with avatars
- Community-scoped events

## Wallet

Rewards, coupons, and loyalty features.

- Coupon redemption with QR codes
- Brand partnerships
- Shareable coupons
- Wallet balance tracking

## Squaddie of the Day

Daily featured community member spotlight.

- Auto-selected from active community members
- Claimable badge / recognition
- Drives daily engagement

## Invitations

Grow the community through invites.

- Unique invite codes per user
- QR code generation and scanning
- Deep link invite URLs (`squad://invite/:code`)
- Invite tracking and rewards

## User Profiles

Fan identity within the community.

- Display name, avatar, birthday
- Squad connections (friends)
- Activity stats (freestyles, messages, calls)
- Block / report functionality
- Account deletion (GDPR compliant)

## Feature Availability

All features are included and enabled by default. The full Squad experience is designed to work as a complete package — each feature reinforces the others to drive fan engagement.

Feature flags exist for cases where a specific partner agreement excludes a feature. They are not intended for ad-hoc toggling:

```tsx
features: {
  squadLine: true,   // Voice calls (patented)
  freestyle: true,   // Audio posts
  messaging: true,   // 1:1 messaging
  polls: true,       // Interactive polls
  events: true,      // Event attendance
  wallet: true,      // Rewards & coupons
}
```

When a feature is disabled, it is hidden from the UI entirely — no dead links or empty screens.
