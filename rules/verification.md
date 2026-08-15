# Verification rules

Required gates are formatting, Ruff, strict mypy, Django checks, migration drift,
focused unit/API tests, OpenAPI drift, integration tests, dependency audit, secret
scan, and independent review. Container startup is required only when configured and
must be reported as blocked rather than passed when the host runtime is unavailable.
Database evidence must prove PostgreSQL connectivity, empty-database migration, current
migration head, second-run idempotence, expected tables/constraints/indexes, and measured
plans or query-count budgets for affected hot paths.
The structure gate must also report no source-rule violations and no incomplete conditional
capability groups. For affected features require race-safe conflicts, authenticated ownership,
response-field filtering, and durable external-effect failure tests.
