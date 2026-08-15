# Architecture rules

- Generate backend source only in target `apps/backend/`.
- Use `core` as configuration and root-level packages for domain apps.
- Keep writes in services, reusable reads in selectors, validation/representation in
  serializers, authorization in permissions, and HTTP orchestration in views.
- Each domain owns models, services, selectors, serializers, views, URLs,
  permissions, admin, migrations, and tests.
- PostgreSQL is the only generated runtime database. Models and migrations own schema;
  selectors own query shape; services own transactions; serializers never mutate on read.
- Mount domain routers below `/api/v1/` with explicit basenames and stable operation IDs.
- OpenAPI is the client/backend integration boundary.
- Use Celery with Redis for requirement-backed deferred work. Commit a PostgreSQL outbox/job in the
  same transaction as business state; enqueue only after commit. Tasks accept scalar identifiers,
  reopen their own database context, and are idempotent under duplicate delivery.
