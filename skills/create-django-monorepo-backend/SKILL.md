---
name: create-django-monorepo-backend
description: Create the Django DRF backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create Django backend

Generate core paths, one declared dependency-lock alternative, and only requirement-triggered
domain capability groups from `rules/project-structure.json`. Resolve supported
versions, lock dependencies, split settings, validate environment variables, configure
PostgreSQL and connection health, decide the user model, configure logging/errors/OpenAPI,
and create only required domain apps. Create tables only through reviewed committed
migrations. Add Docker and CI inside the target only. Run structure, connection, migration,
source-policy, and Django checks before feature implementation.
Docker Compose is the only runtime provider for PostgreSQL and any requirement-backed Redis,
Celery worker, or Celery Beat service. Do not inspect, install, or use host daemons. Pull missing
pinned database/broker images through Compose; build workers and Beat from the locked backend image.
Generate a normalized unique Compose project name and distinct purpose-specific database names.
First emit the PRD-to-domain map. Use explicit PRD nouns or justified familiar capability names,
then scaffold resource/use-case serializer and view packages—never generic direction files.

Use a currently supported minimal Python base image, pin the verified runtime image by
digest, run as a non-root user, and keep build and runtime stages separate. Re-resolve
the base digest instead of copying a permanent example digest. Build without stale
cache for release verification and block acceptance on fixable critical image findings.

Read `../../rules/project-structure.md` for the generated layout. Before writing
configuration, authentication, persistence, or deployment boundaries, read
`../implement-drf-vertical-slice/references/production-delivery.md`. For PostgreSQL,
models, migrations, selectors, serializers, views, and URLs, also read
`../implement-drf-vertical-slice/references/database-api-architecture.md`.
