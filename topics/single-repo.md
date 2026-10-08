[Overview](../README.md)

# Topology A: single repository

**architecture.md contents (150–500 lines; ≤15 minutes to read):**

1. **Purpose & context** (5–10 lines, C4 L1 crop, catalog link).
2. **Codemap**, which is the bulk of the doc and answers "Where is X?" and "What is this?".
3. **Entry points**: binaries, handlers, consumers, jobs.
4. **Boundaries & dependencies**: callers, callees, sync/async, owned vs consumed contracts.
5. **Architecture invariants**: bolded, testable statements, including [absences](../concepts/absences.md) (see [Architecture invariants](../concepts/invariants.md)).
6. **Cross-cutting concerns**: authn/z, errors, observability, config, testing.
7. **Decisions**: link to the ADR index.
8. **Known gaps/debt.**

Two filled-in examples: [ARCHITECTURE.md excerpts](../concepts/architecture-md-excerpts.md).

**Relationship to other files:**

- README covers *what it is and how to run it*.
- architecture.md covers *how it is shaped and where to change things*.
- ADRs cover *why it is this way*.
- API specs cover *the exact contract* and are generated.
- AGENTS.md covers *what an agent must do and avoid*, and points to the rest.

**Real-world reference: rust-analyzer's architecture.md** (matklad's model example). At a pinned commit it is 518 lines (~27 KB): Bird's Eye View, Entry Points, a Code Map of roughly 20 crate subsections, and Cross-Cutting Concerns, with bold "**Architecture Invariant:**" and "**API Boundary**" labels. **Critique:** it has an excellent codemap and invariants. It is also longer than most enterprise services need, has a "Performance Testing: TBA" stub, admits a "code--architecture gap", has no ADRs and little security content, and has since moved to a new path, a reminder that links into docs rot too. **Lesson:** copy its invariant style, not its length.

**Contrast: Backstage's "Architecture overview".** It is a well-diagrammed tour of frontend, backend, plugins and databases ("separate logical database per plugin"), but it mixes concepts with how-to material, uses time-relative wording ("at this time of writing"), and keeps the threat model on a separate page. Linking the threat model is correct. The time-relative wording is a drift smell.

---

## Sources

- [matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)](../sources/architecture-practice.md#matklad-alex-kladov-architecturemd-feb-6-2021) (Architecture documentation practice)
- [rust-analyzer: docs/dev/architecture.md (pinned commit)](../sources/architecture-practice.md#rust-analyzer-docsdevarchitecturemd-pinned-commit) (Architecture documentation practice)
- [rust-analyzer: CLAUDE.md](../sources/architecture-practice.md#rust-analyzer-claudemd) (Architecture documentation practice)
- [Backstage: "Architecture overview"](../sources/architecture-practice.md#backstage-architecture-overview) (Architecture documentation practice)
