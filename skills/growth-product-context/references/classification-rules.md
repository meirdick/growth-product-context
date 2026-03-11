# Classification Rules

## Route-to-Domain Classification Table

| Route Pattern | Journey Domain | AARRR Stage |
|--------------|----------------|-------------|
| `/`, `landing`, `pricing`, `about`, `features`, `blog*`, public marketing pages | Acquisition | Acquisition |
| `register`, `signup`, `verify-email` | Acquisition (conversion) | Acquisition |
| `onboard*`, `welcome`, `getting-started` | Activation (Setup Moment) | Activation |
| First core action (create first {central_model}, complete first workflow) | Activation (Aha Moment) | Activation |
| `login`, `logout`, `password/*`, `two-factor*` | Authentication (supports Retention) | — |
| Core resource CRUD (the central model's routes) | Value Delivery | Retention |
| `dashboard`, `home`, `feed`, `overview` | Value Delivery (entry point) | Retention |
| Secondary resource CRUD | Engagement | Retention |
| `settings`, `profile`, `account`, `preferences`, `notifications` | Engagement | Retention |
| `billing`, `subscribe`, `checkout`, `payment`, `plan*`, `pricing` | Monetization | Revenue |
| `invite*`, `share*`, `team*`, `referral*`, `export*` | Expansion | Referral |
| `admin/*`, `health`, `up`, `status`, `horizon`, `telescope` | Internal (skip) | — |
| `api/*` | Mirror of web domain (classify by resource) | — |

## Acquisition vs Activation — The Boundary Is Signup

- **Acquisition** = anonymous visitor to registered user. Acquisition covers everything from first visit through signup. Track: pageviews, traffic source, UTM parameters, blog engagement, pricing page views, and `user_signed_up` as the conversion event. Signup **ends** Acquisition.
- **Activation** = registered user to first "aha moment." Activation begins **after** signup. It has three sub-moments (per Reforge):
  1. **Setup Moment** — user completes onboarding steps, configures workspace, connects integrations
  2. **Aha Moment** — user experiences core product value for the first time (e.g. creates first project, sees first result, gets first insight). This is the activation metric.
  3. **Habit Moment** — user repeats the core action enough to form a pattern (bridges into Retention)

The aha moment is the key activation metric: it's the earliest event that predicts long-term retention. It is experiential, not intellectual — the user doesn't just *understand* the product, they *feel* its value. Famous examples: Facebook = add 7 friends in 10 days, Slack = 2,000 team messages, Dropbox = first file synced across devices.

**Benchmark:** Median activation rate (signup to aha moment) across SaaS products is ~25%. Average is 34%. A 25% improvement in activation rate can yield a ~34% increase in MRR.

## Usage Pattern Detection

| Signal in Codebase | Usage Pattern | Why |
|---------------------|--------------|-----|
| Notification/reminder models, streak counters, daily metric tables, `last_active_at` on users | **Daily Habitual** | Product expects users back every day |
| Date-range models (`start_date`/`end_date`), trip/project/event with lifecycle states, milestone tracking | **Episodic** | Users have intense windows then go dormant |
| Order/transaction/payment models, cart/checkout, booking system, invoice tables | **Transactional** | Each interaction is an independent value exchange |
| Season/period fields, tax-year, annual-cycle, holiday/calendar models | **Seasonal** | Usage follows predictable calendar patterns |
| Content library, playback/progress tracking, watch/read history, recommendation models | **Consumption** | Users consume content on an ongoing basis |

## Success Metric per Usage Pattern

- **Daily Habitual:** DAU with DAU/MAU stickiness ratio (target 0.3+)
- **Episodic:** Active Window Engagement Rate + Repeat {central_model} Rate
- **Transactional:** Repeat Transaction Rate + Transactions per Active User
- **Seasonal:** Season-over-Season Retention + Seasonal Completion Rate
- **Consumption:** Consumption Hours per Active User + DAU/MAU stickiness
