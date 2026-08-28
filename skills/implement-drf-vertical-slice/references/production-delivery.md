# DRF production delivery reference

Load this reference only while creating, implementing, or independently verifying a
Django REST Framework backend.

## Foundations

- Pin compatible Python, Django, DRF, PostgreSQL driver, schema, test, and lint versions;
  commit the resolved lock. Never invent versions from memory when the target already
  constrains them.
- Split settings by environment, validate required environment variables at startup,
  and keep secrets out of defaults, examples, logs, evidence, and Git.
- Decide the custom user model before the first migration. Use stable public IDs where
  exposing sequential database IDs would leak information.
- Provide liveness and dependency-aware readiness endpoints, structured logs, request
  correlation, trusted proxy configuration, and a uniform error envelope.
- Use PostgreSQL in every generated environment that validates persistence behavior;
  do not use SQLite as a transparent substitute for production database tests.
- Resolve a currently supported minimal Python base, pin the verified release image by
  digest, use separate build/runtime stages and a non-root runtime user, and keep build
  tools out of the final image. Rebuild without stale cache and scan the resulting
  digest; a fixable critical OS or application vulnerability blocks acceptance.

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
- Map race-safe database constraint failures to stable domain conflicts. A uniqueness lookup
  alone is never sufficient under concurrency.

## API and security boundaries

- Serializers validate the transport boundary and remain side-effect free; services enforce business
  invariants. Never rely on client-hidden fields for authorization.
- Permissions are deny-by-default and tested for anonymous, wrong-role, wrong-tenant,
  and object-ownership cases. Filter querysets by tenant before object lookup.
- Define pagination, filtering, ordering, time-zone, decimal, file-size, and upload-type
  behavior. Return stable machine-readable error codes without internal traces.
- Treat authentication, CSRF/CORS, cookies, token rotation/revocation, throttling,
  password reset, and account enumeration as explicit architecture decisions.
- Generate OpenAPI from the running backend, validate it, and detect drift against the
  contract consumed by the frontend.
- Version URLs beneath `/api/v1/`, use explicit router basenames and stable operation IDs,
  and contract-test reverse/resolve behavior.
- Derive actor, owner, tenant, and role from authenticated request context. Never trust a client
  supplied owner or tenant field for authorization or ownership assignment.

## External effects and files

- Place email, object storage, webhooks, queues, and image processing behind service adapters.
  Stream bounded uploads, inspect decoded content, create server-side object keys, and enforce
  content and dimension policy rather than trusting names or MIME headers.
- Use a transactional outbox/job row for effects that must survive restart. Deliver after commit
  with retries, idempotency, observability, and dead-letter handling; reserve in-process callbacks
  for disposable work.
- Use Celery with Redis as the default durable worker. Do not add the obsolete `django-celery`
  integration package: configure Celery directly for Django and autodiscover domain tasks. Use
  `django-celery-beat` only when schedules must be database-managed. Store task results only when
  product behavior reads them.
- Task payloads contain versioned scalar IDs, never ORM instances, request objects, credentials, or
  large files. A task opens fresh database state, checks an idempotency key, applies bounded
  exponential retry with jitter, records terminal failures, and has explicit soft/hard time limits
  and queue routing. Worker readiness must prove broker connection and enqueue-to-consume behavior.
- Define compensation for database/object-store split operations. Do not commit a reference to a
  missing object or delete the old object before its replacement is durable.

## Verification

- Focused lane: formatter, lint, types, Django system checks, migration drift, changed
  unit/service/API tests.
- Full lane: complete backend suite, database-backed integration, OpenAPI contract,
  authorization negatives, startup/health, dependency/security scan, and representative
  query/performance checks.
- Test success, validation failure, conflict, not-found, unauthorized, forbidden,
  throttled, dependency failure, and retry/idempotency behavior as relevant.
- Exercise ownership, concurrent uniqueness, upload validation, outbox retry/idempotency, and
  failure between database commit and every required external effect when those capabilities exist.
- When background work is active, also prove worker startup, Redis connectivity, duplicate delivery,
  retry exhaustion, terminal failure visibility, graceful shutdown, and scheduled dispatch when used.
- Evidence records exact argv, cwd, exit code, tool version, affected requirement IDs,
  and artifact paths. A skipped or unavailable required check is not a pass.
