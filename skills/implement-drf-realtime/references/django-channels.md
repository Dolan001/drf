# Django Channels realtime

- Use ASGI, Django Channels, and `channels-redis`; production must fail readiness when Redis is
  unavailable and must never fall back to `InMemoryChannelLayer`.
- Authenticate with secure cookies or a short-lived single-use HTTPS-issued ticket. Reject invalid
  Origin, anonymous users, expired tickets, nonparticipants, and forbidden commands with stable
  close/error codes. Never put reusable JWTs in query strings.
- Use `AsyncJsonWebsocketConsumer`. Wrap bounded ORM work with `database_sync_to_async`; do not pass
  lazy querysets or model instances through the channel layer.
- Consumer flow: validate frame -> call service -> atomically persist state/outbox -> return ack ->
  publish a scalar JSON event. Do not write models or mark global read state inside broadcast handlers.
- Model participant receipts/cursors and direct-room uniqueness. Use indexed cursor pagination;
  never serialize a complete conversation or execute per-row last-message/unread queries.
- Use opaque, length-bounded group keys. Configure capacity, expiry, timeouts, Redis TLS/auth, health,
  structured metrics, graceful drain, and payload/connection/rate limits.
- Validate attachment ownership and malware-scan status through stored upload records.
- Prove multi-process fan-out with real Redis plus auth, ordering, dedupe, resume, slow-consumer,
  Redis outage/recovery, and graceful-deploy tests. In-memory communicators may cover focused tests
  but cannot satisfy production verification.
