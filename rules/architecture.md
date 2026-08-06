# Architecture rules

- Generate backend source only in target `apps/backend/`.
- Use `core` as configuration and root-level packages for domain apps.
- Keep writes in services, reusable reads in selectors, validation/representation in
  serializers, authorization in permissions, and HTTP orchestration in views.
- Each domain owns models, services, selectors, serializers, views, URLs,
  permissions, admin, migrations, and tests.
- OpenAPI is the frontend/backend integration boundary.
