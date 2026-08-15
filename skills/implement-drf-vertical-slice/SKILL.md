---
name: implement-drf-vertical-slice
description: Implement one requirement-linked Django REST Framework slice from persistence through API tests within leased target paths.
---

# Implement DRF slice

Follow the task contract and existing local patterns. Put invariants in models, writes
in services, reusable reads in selectors, boundary validation only in serializers,
authorization in permissions, and HTTP orchestration plus service invocation in views. Create additive
migrations and negative API tests. Run focused format, lint, types, Django checks,
migration checks, and tests; stop for independent verification.

Read `references/production-delivery.md` for the boundary, security, transaction,
API-error, migration, and test decisions that apply to the slice.
Read `references/database-api-architecture.md` whenever the slice creates or changes a
model, migration, selector, serializer, view, filter, permission, or URL.
