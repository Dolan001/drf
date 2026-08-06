# Verification rules

Required gates are formatting, Ruff, strict mypy, Django checks, migration drift,
focused unit/API tests, OpenAPI drift, integration tests, dependency audit, secret
scan, and independent review. Container startup is required only when configured and
must be reported as blocked rather than passed when the host runtime is unavailable.
