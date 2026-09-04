# Django REST Framework agent pack

Code-free instructions used by `agents` to create or adopt a Django REST
Framework backend inside a target monorepo.
Generated backends use PostgreSQL, migration-owned schema, service/selector boundaries,
explicit serializers, thin views, versioned URLs, and measured query checks.
When requirements need durable deferred work, the conditional background-task capability adds and
verifies Celery, Redis, transactional outbox delivery, worker health, and idempotency tests.
The executable structure contract supports dependency-lock alternatives, requirement-triggered
domain capabilities, and source-policy checks without storing application templates.

Contents:

- `agents/` — stable framework agent capabilities;
- `skills/` — deterministic Django/DRF procedures;
- `commands/` — one-shot phase command contracts;
- `hooks/` — lifecycle checks;
- `rules/` — architecture, generation, security, and verification constraints.

This repository is not copied into generated applications and is never used as an
application scaffold.
