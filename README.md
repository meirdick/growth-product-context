# Growth Product Context

A free [Laravel Boost](https://boost.laravel.com) skill that discovers your application's user journeys by scanning routes, controllers, and models — then proposes a product context with event names following the AARRR (Pirate Metrics) framework.

## What It Does

- Scans your Laravel routes, controllers, and Eloquent models
- Classifies routes into journey domains: Acquisition, Activation, Value Delivery, Monetization, Engagement, Expansion
- Proposes event names and properties for each journey
- Identifies your core action and usage pattern
- Writes a `.growth/PRODUCT.md` and per-domain journey files

## Install

```bash
php artisan boost:add-skill meirdick/growth-product-context
php artisan boost:install --skills --no-interaction
```

The first command downloads the skill. The second syncs it to your agent (Claude Code, Cursor, etc.). Then activate:

```
/growth-product-context
```

## Requirements

- Laravel application
- [Laravel Boost](https://boost.laravel.com) v2+

## Want More?

This skill discovers **what** to track. For **implementing** the tracking automatically, **auditing** coverage, and **PostHog MCP tools**, install the full Growth Engineering package:

**[growengine.dev](https://growengine.dev)**

The full package includes:
- **Auto-instrumentation** — implements PostHog events across your codebase
- **Instrumentation audit** — compares planned vs. actual tracking coverage
- **PostHog MCP server** — query events, funnels, and health checks from your editor
- **Pennant bridge** — auto-syncs feature flags to PostHog

## License

MIT — see [LICENSE](LICENSE).
