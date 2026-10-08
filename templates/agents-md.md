[Overview](../README.md) › [Templates](README.md)

# Minimal AGENTS.md (router)

```markdown
# AGENTS.md  (≤ ~150 lines; human-written; CODEOWNERS-reviewed)
## Commands
- Build: `make build` · Test: `make test` · Single test: `go test ./pkg/x -run TestY`
- Lint/typecheck: `make lint` (run `make fmt`; don't hand-format)
## Conventions an agent would get wrong
- Wrap errors with `errs.Wrap`, never `fmt.Errorf`, in `domain/`.
## Do not touch
- `gen/**` (run `make gen`) · `migrations/**` (human review) · `vendor/**`
## Constraints (canonical: ARCHITECTURE.md §5)
- `domain/` must not import `adapters/`. No new deps in `core/` without an ADR.
## Read before structural changes
- ARCHITECTURE.md §2 Codemap, §4 Boundaries · docs/adr/README.md
```

See also: [Structuring agent context](../ai-agents/structuring-agent-context.md) for the rules behind this shape, and [Non-default conventions](../concepts/non-default-conventions.md) for what qualifies as a convention, with three filled-in AGENTS.md files.
