# One-shot backend creation

After requirements and architecture are approved, create `apps/backend/` inside the
target monorepo. Work as vertical slices, never by dumping a prebuilt application.
Use `rules/project-structure.md` as the ownership blueprint and
`rules/project-structure.json` as the machine-readable structural contract. Generate
only files justified by selected requirements, validate all required paths, and
record every output in the task evidence.
Use PostgreSQL only, create schema through reviewed migrations, generate explicit
input/output serializers and thin versioned views/routers, and emit database verification
evidence before the backend gate.
