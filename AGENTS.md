# Django REST Framework behavior pack

This repository contains AI behavior only. It must never contain a runnable Django
project, generated application source, dependencies, migrations, Dockerfiles, or test
fixtures. Agents consume this pack while writing into a separate target monorepo.

Generated backend code belongs under `apps/backend/` in the target project. Preserve
the recognizable `core` configuration package, root-level domain apps, root
`manage.py`, project-level static/media directories, and domain-owned models,
services, selectors, serializers, views, URLs, permissions, admin, migrations, and
tests. Follow the task contract, path leases, and independent-verification rules.
