---
name: create-django-monorepo-backend
description: Create the Django DRF backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create Django backend

Generate only paths required by `rules/project-structure.json`. Resolve supported
versions, lock dependencies, split settings, validate environment variables, configure
PostgreSQL and connection health, decide the user model, configure logging/errors/OpenAPI,
and create only required domain apps. Create tables only through reviewed committed
migrations. Add Docker and CI inside the target only. Run structure, connection, migration,
and Django checks before feature implementation.

Read `../../rules/project-structure.md` for the generated layout. Before writing
configuration, authentication, persistence, or deployment boundaries, read
`../implement-drf-vertical-slice/references/production-delivery.md`. For PostgreSQL,
models, migrations, selectors, serializers, views, and URLs, also read
`../implement-drf-vertical-slice/references/database-api-architecture.md`.
