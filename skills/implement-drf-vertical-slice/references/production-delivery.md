# DRF production delivery reference

Load this reference only while creating, implementing, or independently verifying a
Django REST Framework backend.

## Foundations

- Pin compatible Python, Django, DRF, database-driver, schema, test, and lint versions;
  commit the resolved lock. Never invent versions from memory when the target already
  constrains them.
- Split settings by environment, validate required environment variables at startup,
  and keep secrets out of defaults, examples, logs, evidence, and Git.
- Decide the custom user model before the first migration. Use stable public IDs where
  exposing sequential database IDs would leak information.
- Provide liveness and dependency-aware readiness endpoints, structured logs, request
  correlation, trusted proxy configuration, and a uniform error envelope.

## Domain and data boundaries

- Models enforce durable invariants with database constraints where possible.
- Services own writes and transaction boundaries. Selectors own reusable reads and
  make query shape explicit. Views coordinate HTTP only.
- Use `transaction.atomic` around a complete business write, not around unrelated I/O.
  Use row locking or idempotency keys when retries or concurrent writes can duplicate
  effects.
- Migrations are additive by default. Separate schema expansion, data backfill, code
  cutover, and later cleanup for zero-downtime changes. Check migration drift and the
  forward plan in CI.
- Prevent N+1 queries deliberately and add a query-count or representative performance
  check for list endpoints with nested relationships.

## API and security boundaries

- Serializers validate the transport boundary; services still enforce business
  invariants. Never rely on client-hidden fields for authorization.
- Permissions are deny-by-default and tested for anonymous, wrong-role, wrong-tenant,
  and object-ownership cases. Filter querysets by tenant before object lookup.
- Define pagination, filtering, ordering, time-zone, decimal, file-size, and upload-type
  behavior. Return stable machine-readable error codes without internal traces.
- Treat authentication, CSRF/CORS, cookies, token rotation/revocation, throttling,
  password reset, and account enumeration as explicit architecture decisions.
- Generate OpenAPI from the running backend, validate it, and detect drift against the
  contract consumed by the frontend.

## Verification

- Focused lane: formatter, lint, types, Django system checks, migration drift, changed
  unit/service/API tests.
- Full lane: complete backend suite, database-backed integration, OpenAPI contract,
  authorization negatives, startup/health, dependency/security scan, and representative
  query/performance checks.
- Test success, validation failure, conflict, not-found, unauthorized, forbidden,
  throttled, dependency failure, and retry/idempotency behavior as relevant.
- Evidence records exact argv, cwd, exit code, tool version, affected requirement IDs,
  and artifact paths. A skipped or unavailable required check is not a pass.
