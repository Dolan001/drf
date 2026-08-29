---
name: verify-django-delivery
description: Independently verify changed Django DRF slices or backend delivery with focused and affected full checks.
---

# Verify Django delivery

Reconstruct requirements, inspect the diff, validate generated structure, and run
format, lint, types, `manage.py check`, migration drift/plan, affected units and APIs,
OpenAPI drift, authorization negatives, and security checks. Capture commands and
results. Do not edit source or mark verified with missing evidence.
Independently validate domain names against the PRD/fallback policy, serializer and view package
filenames, cohesive ownership, and the 300-line split limit.

Reuse a passing shared full-matrix report only when its revision and workspace inputs
still match. For an individual feature, run only focused changed-slice checks; never
rerun the complete backend, contract, integration, and browser matrix per feature.
Use the commands approved in `.ai/test-commands.json`, not ad hoc shell substitutes.
Verify the built runtime image by immutable digest and fail on fixable critical OS or
application vulnerabilities.

Use the independent checklist in
`../implement-drf-vertical-slice/references/production-delivery.md`; verify observable
behavior rather than trusting implementation evidence.
For database or API work, also apply
`../implement-drf-vertical-slice/references/database-api-architecture.md` and require
truthful `.ai/evidence/database-verification.json` evidence from a disposable PostgreSQL
database.
