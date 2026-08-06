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
│   ├── exceptions.py
│   ├── logging.py
│   ├── pagination.py
│   └── settings/
│       ├── __init__.py
│       ├── base.py
│       ├── local.py
│       ├── test.py
│       └── production.py
├── common/
│   ├── __init__.py
│   ├── health.py
│   └── testing.py
├── <domain>/
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py
│   ├── services.py
│   ├── selectors.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   ├── permissions.py
│   ├── admin.py
│   ├── migrations/
│   │   └── __init__.py
│   └── tests/
│       ├── __init__.py
│       ├── factories.py
│       ├── test_models.py
│       ├── test_services.py
│       ├── test_serializers.py
│       └── test_api.py
├── static/
├── media/
└── scripts/
    ├── validate_project.py
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
- OpenAPI is generated from the backend and checked against the canonical contract.

Generation order:

1. Resolve supported Python, Django, and DRF versions and record them.
2. Create configuration, environment validation, health, logging, and dependency
   locks.
3. Decide the custom user model before the first migration.
4. Create only domains required by the feature plan.
5. Implement one vertical slice from model through API tests.
6. Add Docker/CI only in the generated target, never in this pack.
