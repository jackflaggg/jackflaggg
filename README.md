# Rasul Khamzin

Backend engineer, Node.js / NestJS / PostgreSQL. I build and run the backend of an online
school with 80,000+ active students: the core LMS, which is a distributed monolith with about
ten people committing to it, the services around it (auth, notifications, payroll), and a fork
of the whole platform for the LATAM market that I took to production. Almost all of that code
lives in a private GitLab, not here.

For the last year I have owned the auth service: login and session flows, password audit,
RabbitMQ and Redis integrations, login analytics on ClickHouse. The piece I would point to
first is suspicious login detection. It started as a loose idea and I took it to production,
and most of the work was deciding things, not typing: what counts as suspicious, how not to
lock out legitimate users, what to do when geo or device data is incomplete. Login history
comes out of ClickHouse in 7-14 ms on production (uniqExact instead of FINAL, so the skip
indexes keep working). On the way I found a race where releasing a send slot could delete
someone else's live Redis key; the release now checks ownership. I covered all of it with
tests for the edge cases, because those are the ones that turn into incidents.

The rest of the year was about making the platform hold up under load and stop losing data.
Product events reach ClickHouse through a transactional outbox in PostgreSQL and RabbitMQ
with quorum queues, delayed-exchange retries with an attempt limit, a DLQ that records why,
and idempotent consumers on Redis SET NX. Password storage moved out of the monolith into
the auth service. On the database side, bulk advisory locks on document deletion went from
7.5 s to 37 ms (one query over unnest instead of a loop), and six lesson-scheduler scenarios
now run in a single transaction with FOR NO KEY UPDATE instead of FOR UPDATE, because 13
foreign keys point at the lessons table and plain FOR UPDATE would block their inserts.
Search over programs and materials runs on Elasticsearch and is reindexed without downtime:
a new index with a temporary suffix, an atomic alias switch, the old index dropped later.

Things I wrote from zero: an SMS/email notification service; a payroll backend with
event-driven sync from the LMS, e2e on testcontainers (PostgreSQL, RabbitMQ, Redis) and
contract tests for the sync, where a bug-hunt pass caught 26 defects before release,
including duplicate creation and a lost manual status; the learning modules of StudyCats,
a flashcard product: spaced repetition, gamification, a referral program with cron-based
bonuses; a Telegram bot; VK OAuth without Passport.

Security is a habit more than a project: I closed an IDOR in the student dashboard (the id
comes only from the token, with a shadow log of substitution attempts), and login geolocation
never writes the raw IP.

Two things about how I work. I do not call something an optimization until I have measured
it on production before and after; every number above comes from that. And I take tasks end
to end, from the ticket to watching the release on prod, which is also why I review a lot of
code: 160+ merge requests across three services so far. If you want a second opinion on any
of this, my manager's recommendation is on LinkedIn.

Stack I touch daily:

- **Runtime**: Node.js, TypeScript, NestJS, Express
- **Data**: PostgreSQL (EXPLAIN, indexes, locking, advisory locks), TypeORM, Redis, ClickHouse, MongoDB
- **Events**: RabbitMQ (quorum queues, DLX/DLQ, transactional outbox)
- **Search and infra**: Elasticsearch, Docker, GitLab CI
- **Tests**: Jest, Supertest, testcontainers
- **Observability**: Grafana, Loki, Metabase

Open source: [claude-fleet](https://github.com/jackflaggg/claude-fleet), a local
dashboard for all running Claude Code and Codex sessions, with lifecycle hooks and SSE.

Telegram [@jackflagg](https://t.me/jackflagg) · [LinkedIn](https://linkedin.com/in/jackflaggg) · rasul.khamzinnn@gmail.com
