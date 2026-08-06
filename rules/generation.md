# Generation rules

- Inspect before generating; adopt useful existing conventions.
- Generate the smallest requirement-backed vertical slice.
- Never copy runnable code from this behavior pack.
- Never overwrite existing user code without reconciliation evidence.
- Require additive migrations by default and explicit approval for destructive schema
  operations or major dependency upgrades.
- Keep secrets out of source and require environment validation to fail closed.
