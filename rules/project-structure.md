# Generated Django REST Framework structure

The agent generates this structure under target `apps/backend/`. Domain names are
derived from reconciled requirements; `user` below is illustrative, not mandatory.

```text
apps/backend/
├── manage.py
├── pyproject.toml
├── requirements.lock
├── requirements-dev.lock
├── .env.example
├── .gitignore
├── .dockerignore
├── Dockerfile
├── README.md
├── core/
│   ├── __init__.py
│   ├── asgi.py
│   ├── wsgi.py
│   ├── urls.py
│   ├── api.py
│   ├── exceptions.py
│   ├── logging.py
│   ├── pagination.py
│   └── settings/
│       ├── __init__.py
│       ├── base.py
│       ├── local.py
│       ├── test.py
│       ├── production.py
│       └── database.py
├── common/
│   ├── __init__.py
│   ├── health.py
│   ├── models.py
│   ├── pagination.py
│   ├── testing.py
│   └── database/
│       ├── __init__.py
│       └── health.py
├── <domain>/
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py
│   ├── services.py
│   ├── selectors.py
│   ├── serializers/
│   │   ├── __init__.py
│   │   ├── input.py
│   │   └── output.py
│   ├── views.py
│   ├── urls.py
│   ├── filters.py
│   ├── permissions.py
│   ├── admin.py
│   ├── migrations/
│   │   └── __init__.py
│   └── tests/
│       ├── __init__.py
│       ├── factories.py
│       ├── test_models.py
│       ├── test_services.py
│       ├── test_selectors.py
│       ├── test_serializers.py
│       ├── test_api.py
│       └── test_queries.py
├── static/
├── media/
└── scripts/
    ├── validate_project.py
    ├── check_database.py
    ├── check_migration_plan.py
    ├── check_query_plans.py
    └── check_openapi_drift.py
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
- serializers are explicit side-effect-free transport boundaries; input serializers
  call services and output serializers consume selector-shaped querysets.
- domain routers have explicit basenames and mount under the versioned root `/api/v1/`.
- OpenAPI is generated from the backend and checked against the canonical contract.

Generation order:

1. Resolve supported Python, Django, and DRF versions and record them.
2. Create configuration, PostgreSQL environment validation, connection/readiness,
   logging, and dependency locks.
3. Decide the custom user model before the first migration.
4. Create only domains required by the feature plan.
5. Implement one vertical slice from constrained/indexed model through service,
   optimized selector, explicit serializers, thin view, named URL, and API tests.
6. Generate and review migrations; prove an empty PostgreSQL database reaches head.
7. Add query budgets/plans, database evidence, Docker, and CI only in the target.
