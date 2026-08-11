# Django REST Framework behavior pack

This repository contains AI behavior only. It must never contain a runnable Django
project, generated application source, dependencies, migrations, Dockerfiles, or test
fixtures. Agents consume this pack while writing into a separate target monorepo.

Generated backend code belongs under `apps/backend/` in the target project. Preserve
the recognizable `core` configuration package, root-level domain apps, root
`manage.py`, project-level static/media directories, and domain-owned models,
services, selectors, serializers, views, URLs, permissions, admin, migrations, and
tests. Follow the task contract, path leases, and independent-verification rules.

Load agent and skill Markdown only after Django DRF is selected. JSON catalogs route
the workflow but do not replace behavioral instructions. Prefer cached discovery,
bounded context, and focused checks.
