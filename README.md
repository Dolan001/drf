# Django REST Framework behavior pack

This repository teaches the [agents workflow](https://github.com/Dolan001/agents) how to build and
verify a Django REST Framework backend. It contains instructions and structure contracts, not a
runnable Django project. Generated code belongs in the user's `apps/backend/` directory.

## When to use it

Declare `Backend framework: Django REST Framework` in the PRD. The workflow selects this `drf`
submodule automatically; users do not copy its files or invoke its internal agents. `$start-build`
runs the complete build. To stop after backend verification, use `$start-backend` or:

```bash
./.agents/bin/ai start-backend --project . --github-user <your-github-user>
```

The PRD or saved state supplies the framework choice, so `--backend django-drf` is needed only
when that choice has not been recorded. Web and mobile builds require a backend; a local desktop
app or standalone Python package does not unless its product requirements call for an API. No
command pushes by default.

## What it creates and verifies

Generated backends use PostgreSQL, migration-owned schema, service/selector boundaries,
explicit serializers, thin views, versioned URLs, and measured query checks.
When requirements need durable deferred work, the conditional background-task capability adds and
verifies Celery, Redis, transactional outbox delivery, worker health, and idempotency tests.
The executable structure contract supports dependency-lock alternatives, requirement-triggered
domain capabilities, and source-policy checks without storing application templates.

The independent gate checks migrations on an empty database, runtime startup, live HTTP and
database round trips, authorization and negative cases, query budgets, OpenAPI, and security. A
passing unit test alone does not complete the gate. Evidence and checkpoints live under `.ai/` in
the target project; `$workflow-status` identifies a failing check or resume point.

## Pack contents

- `agents/` — stable framework agent capabilities;
- `skills/` — deterministic Django/DRF procedures;
- `commands/` — one-shot phase command contracts;
- `hooks/` — lifecycle checks;
- `rules/` — architecture, generation, security, and verification constraints.

This repository is not copied into generated applications and is never used as an
application scaffold.
