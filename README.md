# helpdesk-core

Multi-tenant live support/helpdesk backend, inspired by internal architecture of Zendesk and Intercom.

Built to solve the core hard problem in real-time support systems: a chat session
or agent connection surviving a drop/reconnect without losing state, backed by
real-time notifications for queue and assignment events.

**Status:** In development - schema and connection-layer design completed, implementation in progress.

## Stack
NestJS · TypeScript · PostgreSQL · Prisma · Redis · BullMQ · Docker

## Core design decisions
- Durable state (Postgres) vs. live/ephemeral state (Redis) split for session lifecycle vs. connection presence
- Two-key Redis connection registry with reverse socket lookup for reconnect handling
- Grace-period-via-delayed-job disconnect handling (BullMQ) instead of plain TTL, so disconnect events produce real notifications
- Monotonic per-session sequence numbers for message ordering; client-generated idempotency keys to dedupe retried sends