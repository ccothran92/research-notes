[Overview](../README.md) › [Templates](README.md)

# Minimal repo-level ARCHITECTURE.md (annotated)

```markdown
---
owner: team:<catalog-group>          # catalog group, not a person
system: <catalog-system-id>
last_reviewed: YYYY-MM-DD
review_interval: 180d                # 90d for tier-1
status: current                      # current | target | deprecated
---
# <Service> — Architecture

## 1. Purpose and context            <!-- 5–10 lines + C4 L1 crop (generated if possible) -->
## 2. Codemap                        <!-- 40–60% of doc. Name modules/types; don't hyperlink files -->
- `cmd/server` — HTTP entry point; wires adapters.
- `domain/` — pricing rules; **no I/O**.
## 3. Entry points                   <!-- 3–10 bullets -->
## 4. Boundaries and dependencies    <!-- table: direction, counterparty, contract ID, sync/async, semantics -->
## 5. Architecture invariants        <!-- 3–10 bold, testable statements + how enforced. Highest value. -->
- **`domain/` never imports from `adapters/`.** (enforced: import-lint rule)
## 6. Cross-cutting concerns         <!-- 10–30 lines: authn/z, errors, observability, config, testing -->
## 7. Deployment (shape only)        <!-- 5–15 lines; numbers live in IaC -->
## 8. Decisions                      <!-- links to ADR index + 3–5 key ADRs -->
## 9. Known gaps and debt            <!-- ≤10 bullets; label "Target (not yet true)" -->
```

**Length budgets:** T3 ≈ 80–150 lines; T2 ≈ 150–300; T1 ≈ 300–500. Beyond 500 lines, split into linked explanation pages.

See also: [Single-repo topology](../topics/single-repo.md) for what each section should contain and how it relates to README, ADRs and AGENTS.md; [Architecture invariants](../concepts/invariants.md) and [Absences](../concepts/absences.md) for how to write section 5; and [two filled-in excerpts](../concepts/architecture-md-excerpts.md).
