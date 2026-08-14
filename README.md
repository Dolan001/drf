# Django REST Framework agent pack

Code-free instructions used by `ai_workflow` agents to create or adopt a Django REST
Framework backend inside a target monorepo.
Generated backends use PostgreSQL, migration-owned schema, service/selector boundaries,
explicit serializers, thin views, versioned URLs, and measured query checks.

Contents:

- `agents/` — stable framework agent capabilities;
- `skills/` — deterministic Django/DRF procedures;
- `commands/` — one-shot phase command contracts;
- `hooks/` — lifecycle checks;
- `rules/` — architecture, generation, security, and verification constraints.

This repository is not copied into generated applications and is never used as an
application scaffold.
