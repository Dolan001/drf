---
name: implement-drf-background-work
description: Implement requirement-backed durable Django background jobs, scheduled work, notifications, or external effects with Celery, Redis, and a PostgreSQL outbox. Use when a DRF slice activates background tasks.
---

# Implement DRF background work

Complete the background-task capability group. Read
`../implement-drf-vertical-slice/references/production-delivery.md` section “External effects and
files”. Keep task entrypoints thin, scalar-ID based, idempotent, bounded, observable, and independently
verified. Do not add Beat or result storage unless requirements consume them.
