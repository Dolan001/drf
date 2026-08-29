# Generation rules

- Inspect before generating; adopt useful existing conventions.
- Generate the smallest requirement-backed vertical slice.
- Never copy runnable code from this behavior pack.
- Never overwrite existing user code without reconciliation evidence.
- Require additive migrations by default and explicit approval for destructive schema
  operations or major dependency upgrades.
- Keep secrets out of source and require environment validation to fail closed.
- Never fall back to SQLite. Require PostgreSQL connection settings, bounded connection
  lifetime/health checks, timeouts, and a dependency-aware readiness check.
- Derive constraints and indexes from invariants and measured query shapes; do not add
  speculative indexes or perform database writes during serialization.
- Pass the executable source rules and complete every activated domain capability group.
- Resolve domain names from PRD product nouns first. If the PRD describes a capability without
  naming it, use familiar capability vocabulary; never substitute vague architecture labels.
- Use resource/use-case serializer and view packages, never `input.py`, `output.py`, or one growing
  `views.py`. Keep cohesive source modules at or below the contract's 300-line split threshold.
