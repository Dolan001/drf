# DRF PostgreSQL and API architecture

Load this reference for any Django model, migration, selector, serializer, view,
filter, permission, or URL change.

## PostgreSQL connection

- Generate PostgreSQL configuration only; never silently fall back to SQLite. Read
  credentials from validated environment variables or one secret-backed `DATABASE_URL`.
- Configure the supported psycopg driver, connection reuse appropriate to the serving
  mode, connection health checks, connect/statement/lock timeouts, SSL requirements,
  application name, and UTC deliberately. Keep credentials out of logs and evidence.
- Make liveness process-only and readiness execute a bounded `SELECT 1` plus migration-
  head check. Fail startup or readiness when required database configuration is absent.
- The root Compose contract supplies PostgreSQL with a health check and persistent volume;
  application startup waits for health and applies migrations as a distinct controlled job.

## Models and schema

- Use a UUID or another requirements-backed public identifier; keep internal primary-key
  choices private. Use timezone-aware timestamps and `DecimalField` for money or points.
- Express requiredness with non-null columns and domain invariants with named `CheckConstraint`,
  `UniqueConstraint`, foreign keys, and appropriate `on_delete` behavior. Validation in
  Python never replaces a database constraint for durable invariants.
- Give reverse relations meaningful plural names. Avoid generic relations and JSON fields
  when a relational model provides enforceable structure; document justified PostgreSQL
  arrays, JSONB, ranges, full-text, exclusion constraints, or generated columns.
- Design indexes from observed filters, joins, ordering, uniqueness, and query plans.
  Consider composite left-prefix order, partial indexes, expression indexes, GIN/GiST/BRIN,
  and covering columns only when the workload supports them. Account for write overhead.
- Record table, constraint, and index names. Keep identifiers stable and within PostgreSQL
  limits so migrations and diagnostics remain predictable.
- Normalize case-insensitive usernames, emails, slugs, and natural keys deliberately. Pair
  canonical application input with a PostgreSQL functional unique constraint or another explicit
  representation. Preflight checks improve errors but never replace catching `IntegrityError`,
  mapping the named constraint to a stable conflict code, and leaving the transaction usable.
- Prefer database-generated creation timestamps and deliberate update/version columns when
  multiple writers exist. Keep Django and database defaults consistent.

## Migrations and table creation

- Create and alter tables only with committed Django migrations. Never create production
  tables from ad hoc SQL at startup and never use `--run-syncdb` as a migration substitute.
- Decide `AUTH_USER_MODEL` before the initial migration. Review generated operations and
  generated SQL before applying them; never treat autogeneration as sufficient review.
- Prefer expand/backfill/cutover/contract. Add nullable columns before backfill, batch large
  data changes, make `RunPython` use historical models, and separate irreversible cleanup.
- Use non-atomic or concurrent PostgreSQL operations only when required, explicitly reviewed,
  and compatible with Django migration state. Define a forward recovery for irreversible work.
- Verify a clean PostgreSQL database can migrate from zero to head, `makemigrations --check`
  is clean, `showmigrations --plan` is expected, the second `migrate` is a no-op, and affected
  rollback/forward recovery works where safe.

## Query ownership and optimization

- Put reusable read composition in selectors or custom QuerySet methods. Views choose a
  selector and serializers consume already-shaped objects; serializers must not initiate an
  unbounded query per row.
- Use `select_related` for required single-valued joins and `Prefetch`/`prefetch_related` for
  collections with an explicit filtered queryset. Use `only`, `defer`, annotations, subqueries,
  aggregates, bulk operations, and iterator/chunking only after measuring their tradeoffs.
- Filter tenant/owner scope before lookup. Whitelist filter and ordering fields, cap page size,
  and use deterministic ordering with a unique tie-breaker. Prefer cursor/keyset pagination for
  large or frequently changing collections.
- Keep writes in services with explicit `transaction.atomic` scope. Use `select_for_update`,
  unique constraints, optimistic versioning, or idempotency keys for concurrency as required.
  Do not hold a transaction open across network calls or background work.
- For each hot list/detail/write path, capture query count and a sanitized PostgreSQL
  `EXPLAIN` plan. Add a regression budget when N+1 behavior or plan degradation is plausible.

## Serializers, views, and URLs

- Separate write/input serializers from read/output serializers when field policy differs.
  Declare fields explicitly for public APIs; avoid `fields = "__all__"`. Validate shape and
  cross-field input and return validated data. The view/ViewSet calls a service for business writes.
- Serialization is side-effect free. Never update rows in `SerializerMethodField`,
  `to_representation`, property access, list, or retrieve operations. Precompute or annotate
  derived state, or update it through a scheduled/service command.
- Use a ViewSet only for a cohesive resource. Restrict `http_method_names`, declare action-
  specific serializer/permission policy, keep `get_queryset` tenant-safe and optimized, and
  delegate writes to services. Use APIView/generics for workflows that are not CRUD resources.
- Each domain exposes one `SimpleRouter` or explicit URL module with explicit `basename`,
  `app_name`, stable resource nouns, typed path converters, and named non-ViewSet routes. The
  root mounts domains under `/api/v1/<resource>/`; router prefixes have no duplicate slash.
- Give custom actions stable `url_path`, `url_name`, methods, request/response schemas,
  permissions, status codes, and OpenAPI operation IDs. Keep schema endpoints available under
  explicit environment policy rather than coupling the schema itself to debug mode.

## Required verification

- Test model constraints against PostgreSQL, services and concurrency, selector query counts,
  serializer input/output and secret exclusion, permissions, pagination/filter/order limits,
  URL reverse/resolve names, OpenAPI operation IDs, and success plus negative API behavior.
- Write `.ai/evidence/database-verification.json` without secrets. It must truthfully record
  connection, migration, schema, and query verification required by the workflow schema.
- Keep a clean-database migration lane separate from faster transaction-isolated API tests.
  Tests using disabled migrations, SQLite, or schema shortcuts never count as migration evidence.
