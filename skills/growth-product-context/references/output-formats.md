# Output Formats

## PRODUCT.md Format

Write this to `.growth/PRODUCT.md`:

```markdown
# Product Context

## Product Overview

- **Name:** [Confirmed product name]
- **One-liner:** [What the product does in one sentence]
- **Stage:** [Pre-launch / Early / Growth / Mature]
- **Usage Pattern:** [Daily Habitual / Episodic / Transactional / Seasonal / Consumption]
- **Value Prop:** [Core value proposition]

## User Personas

[Discovered from models and routes]

## Core Action

- **Event:** [core_action_event_name]
- **Description:** [What the user does]
- **Central Model:** [The model at the center of the product]

## Primary Success Metric

[Pattern-appropriate metric from Step 9]

## User Journeys

| Domain | File | Events | Status |
|--------|------|--------|--------|
| Acquisition | journeys/acquisition.md | X | Not Tracked |
| Activation | journeys/activation.md | X | Not Tracked |
| Value Delivery | journeys/value-delivery.md | X | Not Tracked |
| Monetization | journeys/monetization.md | X | Not Tracked |
| Engagement | journeys/engagement.md | X | Not Tracked |
| Expansion | journeys/expansion.md | X | Not Tracked |
```

## Journey File Format

Each journey file follows this structure. Write one per domain in `.growth/journeys/`.

Example for `.growth/journeys/acquisition.md`:

```markdown
# Acquisition Journey

Guest visitor → registered user. Ends at signup.

## Flow

Land on Homepage / Blog → View Pricing → Sign Up → Verify Email

## Events

| Step | Event Name | Type | Properties | Status |
|------|-----------|------|------------|--------|
| View landing page | `$pageview` | auto | `$referrer`, `utm_source`, `utm_medium`, `utm_campaign` | Not Tracked |
| Read blog post | `$pageview` | auto | `$referrer`, `utm_source`, `blog_slug` | Not Tracked |
| View pricing page | `$pageview` | auto | `$referrer`, `utm_source` | Not Tracked |
| Sign up | `user_signed_up` | server | `method`, `referral_source`, `utm_source` | Not Tracked |
| Verify email | `email_verified` | server | `time_to_verify` | Not Tracked |

## Conversion Funnel

$pageview (landing) → $pageview (pricing) → user_signed_up → email_verified → [handoff to Activation]

## Metrics

- **Signup rate:** user_signed_up / unique visitors
- **Verification rate:** email_verified / user_signed_up

## Notes

- Acquisition events fire for anonymous visitors until signup — no `distinctId` until `user_signed_up`
- At signup, alias the anonymous ID to the new user ID (PostHog handles this automatically)
- Capture UTM parameters on first `$pageview` and carry them through to `user_signed_up`
- Blog posts are key entry points — track `blog_slug` to identify high-converting content
```
