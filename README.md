# Rasul Khamzin

Senior backend engineer, Node.js / NestJS / PostgreSQL. I build and run the backend
of an online school: the core LMS, which is a distributed monolith with about ten people
committing to it, the services around it (auth, notifications, payroll), and a fork of
the whole platform for the LATAM market that I took to production. Almost all of that
code lives in a private GitLab, not here.

For the last two years I have owned the auth service: login and session flows, password
audit, RabbitMQ and Redis integrations, login analytics on ClickHouse. The piece I would
point to first is suspicious login detection. It started as a loose idea and I took it to
production, and most of the work was deciding things, not typing: what counts as
suspicious, how not to lock out legitimate users, what to do when geo or device data is
incomplete. I covered it with tests for the edge cases, because those are the ones that
turn into incidents.

The rest of the year was about making the platform hold up under load and stop losing
data. I moved password storage out of the monolith into the auth service and built a
versioned event-contract package for the events between them. On the database side
I rewrote the hottest queries (the popular-lessons cache alone was a third of total DB
time), added an outbox, a DLQ with a circuit breaker for ClickHouse, Redis locks, and
fixed a TOCTOU race on payout approval. Search over programs and materials runs on
Elasticsearch with a reindex pipeline sized for production volumes.

Things I wrote from zero: an SMS/email notification service, a payroll backend with
event-driven sync from the LMS, a Telegram bot, VK OAuth without Passport, a referral
program with document workflow and amoCRM.

I also review a lot of code. 160+ merge requests across the team so far. If you want
a second opinion on any of this, my manager's recommendation is on LinkedIn.

Stack I touch daily: NestJS, TypeORM, PostgreSQL, Redis, RabbitMQ, Bull, ClickHouse,
Elasticsearch, Socket.io, Docker, GitLab CI, Sentry, Prometheus, OpenTelemetry, Jest.

Open source: [claude-fleet](https://github.com/jackflaggg/claude-fleet), a local
dashboard for all running Claude Code and Codex sessions, with lifecycle hooks and SSE.

Telegram [@jackflagg](https://t.me/jackflagg) · [LinkedIn](https://linkedin.com/in/jackflaggg) · rasul.khamzinnn@gmail.com
