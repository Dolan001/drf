---
name: verify-django-delivery
description: Independently verify changed Django DRF slices or backend delivery with focused and affected full checks.
---

# Verify Django delivery

Reconstruct requirements, inspect the diff, validate generated structure, and run
format, lint, types, `manage.py check`, migration drift/plan, affected units and APIs,
OpenAPI drift, authorization negatives, and security checks. Capture commands and
results. Do not edit source or mark verified with missing evidence.

Use the independent checklist in
`../implement-drf-vertical-slice/references/production-delivery.md`; verify observable
behavior rather than trusting implementation evidence.
