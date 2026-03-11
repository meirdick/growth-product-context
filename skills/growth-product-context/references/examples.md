# Examples

## Route Discovery Summary

```
Found 42 routes across web and API:

| Domain     | Routes | Examples                          |
|------------|--------|-----------------------------------|
| Auth       | 8      | /login, /register, /password/reset |
| Dashboard  | 3      | /dashboard, /dashboard/stats       |
| Resources  | 18     | /projects/*, /tasks/*              |
| Billing    | 5      | /billing, /subscribe, /checkout    |
| Settings   | 6      | /settings/*, /profile              |
| API        | 12     | /api/v1/projects, /api/v1/tasks    |
```

## Controller Mapping Summary

```
Key actions found:

- ProjectController: create, update, archive, share (uses Project model)
- TaskController: create, complete, assign, reorder (uses Task model)
- BillingController: subscribe, cancel, updatePlan (uses Subscription model)
- OnboardingController: step1, step2, complete (uses User model)
```

## Domain Entity Summary

```
Domain entities:

- User (central: has projects, tasks, subscriptions, teams)
- Project (belongs to user, has many tasks)
- Task (belongs to project, has statuses)
- Subscription (belongs to user, has plan)
- Team (belongs to user, has members)

Proposed central model: Project
  Rationale: Most relationships, core CRUD operations, referenced in most controllers
```

## Journey Domain Classification

```
Proposed journey domains:

ACQUISITION (4 routes) — guest visitor → registered user
  / (landing page), /pricing, /register, /verify-email

ACTIVATION (5 routes) — registered user → aha moment
  Setup: /onboarding/step-1, /onboarding/step-2, /onboarding/step-3,
         /onboarding/complete
  Aha:   /projects/create (first project = core value experienced)

VALUE DELIVERY (12 routes) — ongoing core usage
  /dashboard, /projects, /projects/{id}, /projects/{id}/edit,
  /tasks/create, /tasks/{id}/complete, ...

MONETIZATION (4 routes)
  /billing, /subscribe/{plan}, /checkout, /billing/portal

ENGAGEMENT (6 routes)
  /settings, /profile, /notifications, /preferences, ...

EXPANSION (3 routes)
  /teams/invite, /projects/{id}/share, /referral

INTERNAL (skipped: 4 routes)
  /admin/*, /health, /up

Does this grouping look right? Should any routes move to a different domain?
What is the "aha moment" — the first action where a user experiences real value?
```

## Journey Flow Diagrams

**Acquisition Journey** (guest to registered user):

Homepage ($pageview) → Blog Post ($pageview) → Pricing ($pageview) → Sign Up (user_signed_up) → Verify Email (email_verified)

Properties per step:
- Homepage: `$referrer`, `utm_source`, `utm_medium`, `utm_campaign`
- Blog Post: `$referrer`, `utm_source`, `blog_slug`
- Pricing: `$referrer`, `utm_source`, `utm_medium`
- Sign Up: `method`, `referral_source`, `utm_source`

Conversion event: `user_signed_up`. Metric: signup rate = signups / unique visitors.

**Activation Journey** (registered user to aha moment):

Complete Onboarding (onboarding_completed) → Configure Workspace (workspace_configured) → First Core Action (project_created)

Properties per step:
- Onboarding: `steps_completed`, `time_to_complete`
- First Core Action: `template`, `time_since_signup`

Setup Moment: `onboarding_completed`. Aha Moment: `project_created` (first time user experiences core value). Metric: activation rate = users who hit aha / signups (benchmark: ~25% median).

**Value Delivery Journey** (ongoing core usage):

Open Dashboard (dashboard_viewed) → Create Project (project_created) → Add Tasks (task_created) → Complete Task (task_completed) → View Progress (dashboard_viewed)

Properties per step:
- Project: `template`
- Task Created: `project_id`, `position`
- Task Completed: `project_id`, `time_to_complete`

**Monetization Journey** (free user to paying customer):

View Pricing (pricing_page_viewed) → Select Plan (plan_selected) → Start Checkout (checkout_started) → Complete Payment (payment_completed) → Upgrade Confirmed (subscription_started)

Properties per step:
- Pricing Page: `$referrer`, `current_plan`
- Plan Selected: `plan_name`, `billing_interval`, `price`
- Checkout Started: `plan_name`, `trial_eligible`
- Payment Completed: `plan_name`, `billing_interval`, `amount`, `payment_method`
- Subscription Started: `plan_name`, `trial_days`, `mrr_impact`

Conversion event: `payment_completed`. Metric: free-to-paid rate = paying users / signups.

**Engagement Journey** (active user deepening usage):

Update Profile (profile_updated) → Configure Settings (settings_changed) → Enable Notifications (notifications_configured) → Use Secondary Feature (feature_used)

Properties per step:
- Profile Updated: `fields_changed`
- Settings Changed: `setting_name`, `old_value`, `new_value`
- Notifications Configured: `channel`, `enabled`
- Feature Used: `feature_name`, `context`

Metric: feature breadth = distinct features used per active user.

**Expansion Journey** (user inviting others or sharing):

Invite Team Member (team_invite_sent) → Share Resource (resource_shared) → Referral Link Created (referral_created) → Referred User Signs Up (referral_converted)

Properties per step:
- Team Invite: `invite_method`, `role`
- Resource Shared: `resource_type`, `share_method`, `recipient_is_user`
- Referral Created: `channel`
- Referral Converted: `referrer_id`, `time_to_convert`

Conversion event: `referral_converted`. Metric: viral coefficient = invites sent * conversion rate.

## Core Action Proposal

```
Proposed core action: project_created

Rationale:
- Project is the central model with the most relationships
- Creating a project is the primary action in the value delivery journey
- It represents the moment the user gets value from the product

Is this the right core action, or is there a different action that
better represents the "aha moment" for your users?
```

## Usage Pattern Detection

```
Detected usage pattern: Episodic

Evidence:
- Trip model has `departure_date` and `return_date` (lifecycle dates)
- Checklist items are tied to a trip with a finite completion state
- No daily streak or notification loop features
- Users plan intensely before a trip, then go dormant until next trip

Does this match how your users interact with the product?
```

## Success Metric (Episodic Example)

```
Primary: Active Window Engagement Rate
Formula: Of users with an active {central_model} (end_date > today),
         % who performed {core_action} in the last 7 days

North Star: Repeat {central_model} Rate
Formula: % of users who create a 2nd {central_model} after their
         first one completes

Leading: Activation rate (signup → first {core_action})
         Completion rate at end-of-window
Lagging: Repeat {central_model} rate, time to 2nd {central_model}
```
