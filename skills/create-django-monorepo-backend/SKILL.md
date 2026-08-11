---
name: create-django-monorepo-backend
description: Create the Django DRF backend structure in a new target monorepo after architecture and task contracts are approved.
---

# Create Django backend

Generate only paths required by `rules/project-structure.json`. Resolve supported
versions, lock dependencies, split settings, validate environment variables, decide
the user model, configure health/logging/errors/OpenAPI, and create only required
domain apps. Add Docker and CI inside the target only. Run structure and Django checks
before feature implementation.
