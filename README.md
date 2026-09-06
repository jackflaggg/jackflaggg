# Rasul Khamzin

Senior Backend Engineer - Node.js / NestJS / PostgreSQL. EdTech, high-load LMS platforms.

I own backend for a family of online-school products: a monolith with ~10 contributors,
an auth service, a notification service, a payroll integration, and a localized fork of the
platform for the LATAM market. Most of my work lives in a self-hosted GitLab, so the
contribution graph here shows only a fraction of it.

## What I've done recently

- **Auth security pipeline, end to end.** Designed a versioned event-contract package
  (`@school/contracts`, 0.2 to 0.11), login-flow logging, IP handling with privacy constraints,
  geo enrichment, a ClickHouse-backed login analytics store with dashboards and alerts,
  and suspicious-login notifications with a per-day email ceiling. Moved password storage
  out of the monolith into the auth service, with cleanup migrations.
- **Reliability patterns in production.** Outbox pattern, DLQ processor with a circuit breaker
  for ClickHouse, distributed locks and guards on Redis, race-condition fixes
  (TOCTOU on payout approval, academic-year creation).
- **Database performance.** Query rewrites and indexes on the hottest endpoints: popular
  lessons cache cut 33.6% of total DB time, action analytics dedupe cut 18.6%,
  diary and grading dashboards, reindex jobs sized for production volumes, P0 fix for
  a series deletion that took up to a minute.
- **Search.** Cross-entity search over programs and materials on OpenSearch, with
  reindexing pipeline and production resync.
- **Services from scratch.** SMS/email notification microservice; payroll backend with
  event-driven sync from the LMS, DLQ handling, CI/CD and Docker; Telegram bot on webhooks;
  OAuth 2.0 for VK without Passport; referral program with document workflow and amoCRM.
- **Platform localization and launch.** Prepared and shipped the LATAM fork to production:
  i18n across notifications, exports and reports, ClickHouse migrations in the deploy
  pipeline, trimester academic year, data backfills.
- **Code review.** Reviewer on 160+ merge requests across the team.

## Stack

NestJS, TypeScript, PostgreSQL / TypeORM, Redis, RabbitMQ, Bull, ClickHouse, OpenSearch,
Socket.io, S3, Docker, GitLab CI, Sentry, Prometheus, OpenTelemetry, Pino, Jest / Supertest
(unit, integration, e2e).

## Open source

- [claude-fleet](https://github.com/jackflaggg/claude-fleet) - local dashboard for
  all running Claude Code / Codex sessions: lifecycle hooks, SSE, focus-on-click,
  liveness detection. Tests included.

## Contact

Telegram [@jackflagg](https://t.me/jackflagg) · [LinkedIn](https://linkedin.com/in/jackflaggg) · rasul.khamzinnn@gmail.com
