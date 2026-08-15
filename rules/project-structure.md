# Generated Django REST Framework structure

The agent generates this structure under target `apps/backend/`. Domain names are
derived from reconciled requirements; `user` below is illustrative, not mandatory.

```text
apps/backend/
├── manage.py
├── pyproject.toml
├── <resolved dependency lock>
├── .env.example
├── .gitignore
├── .dockerignore
├── Dockerfile
├── compose.yaml                    # conditional: Redis + Celery worker
├── README.md
├── core/
│   ├── __init__.py
│   ├── asgi.py
│   ├── wsgi.py
│   ├── urls.py
│   ├── api.py
│   ├── exceptions.py
│   ├── logging.py
│   ├── celery.py                   # conditional: durable background work
│   └── settings/
│       ├── __init__.py
│       ├── base.py
│       ├── local.py
│       ├── test.py
│       ├── production.py
│       ├── database.py
│       └── tasks.py                # conditional: broker/queue policy
├── common/
│   ├── __init__.py
│   ├── models.py
│   ├── pagination.py
│   └── database/
│       ├── __init__.py
│       └── health.py
├── <domain>/
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py
│   ├── services.py
│   ├── tasks.py                    # conditional: scalar-ID task entrypoints
│   ├── outbox.py                   # conditional: transactional delivery records
│   ├── migrations/
│   │   └── __init__.py
│   └── tests/
│       ├── __init__.py
│       ├── test_models.py
│       ├── test_services.py
│       ├── test_tasks.py
│       └── test_outbox.py
└── scripts/
    ├── validate_project.py
    ├── check_database.py
    ├── check_migration_plan.py
    ├── check_query_plans.py
    ├── check_openapi_drift.py
    └── check_workers.py            # conditional
```

Ownership:

- `core`: configuration, root routing, standardized errors, logging, and cross-domain
  infrastructure.
- `common`: deliberately shared primitives only.
- `<domain>`: complete business capability boundary.
- models own persistence invariants; services own writes; selectors own reusable
  reads; serializers own boundary validation; permissions authorize; views
  orchestrate HTTP.
- migrations are committed and additive by default.
- PostgreSQL is mandatory; `core/settings/database.py` owns validated connection and
  timeout policy while `common/database/health.py` owns bounded readiness checks.
- serializers are explicit side-effect-free transport boundaries. Input serializers return
  validated data; views/ViewSets pass that data and authenticated actor context to services.
  Output serializers consume selector-shaped objects and never initiate writes.
- domain routers have explicit basenames and mount under the versioned root `/api/v1/`.
- OpenAPI is generated from the backend and checked against the canonical contract.

Required and conditional paths:

- Resolve exactly one lock strategy: `uv.lock`, `poetry.lock`, `pdm.lock`, or the pair
  `requirements.lock` plus `requirements-dev.lock`.
- Every domain requires only its app configuration, models, services, migrations, and focused
  model/service tests.
- Adding `selectors.py` activates the optimized-read group and requires selector/query tests.
- Adding serializers, views, or URLs activates the REST API group and requires explicit input/
  output serializers, views, URLs, permissions, and serializer/API tests.
- Adding any domain `tasks.py` activates the background-task group. It requires Celery with the
  Redis extra, a Redis broker URL, Django task autodiscovery, a worker health check, Redis and
  worker services in `compose.yaml`, and task tests. Add Celery Beat only for requirement-backed
  schedules; add a result backend only when application behavior consumes task results.
- Adding a consumer/router activates realtime. Require Django Channels, `channels-redis`, Redis-only
  production channel layers, ASGI routing, versioned events, domain auth/fan-out tests, a realtime
  health check, and live Redis evidence. Never fall back to an in-memory layer in production.
- Add `filters.py`, `admin.py`, `common/testing.py`, factories, static assets, or media handling
  only when requirements need them. Production user uploads belong in a requirement-backed
  storage adapter, not a repository media directory.

Generation order:

1. Resolve supported Python, Django, and DRF versions and record them.
2. Create configuration, PostgreSQL environment validation, connection/readiness,
   logging, and dependency locks.
3. Decide the custom user model before the first migration.
4. Create only domains and conditional capability groups required by the feature plan.
5. Implement one vertical slice from constrained/indexed model through service,
   optimized selector, explicit serializers, thin view, named URL, and API tests.
6. For durable deferred effects, persist an outbox/job in the business transaction, then add
   idempotent Celery delivery, bounded retries, failure records, and worker evidence.
7. Generate and review migrations; prove an empty PostgreSQL database reaches head.
8. Add query budgets/plans, database evidence, Docker, and CI only in the target.
