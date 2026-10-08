[Overview](../README.md) › [Worked examples](README.md)

# Example 4 — Public library/SDK (stress test: the API *is* the architecture)

**GOOD:** a short contributor-facing architecture.md with layering and compatibility invariants ("public API changes require a two-minor-version deprecation cycle"; "core has zero runtime dependencies"; "no HTTP client imported outside `transport/`"). Separately, Diátaxis-structured user docs with a generated API reference.

**BAD:** an architecture.md that is really a user guide, mixing install steps, API tables and internal design so that it serves neither users nor contributors.

**Where it breaks down:** C4 context and container views add almost nothing here, so skip them. The architecture doc shrinks to module layering and compatibility rules.

---

## Sources

- [Diátaxis (Daniele Procida)](../sources/frameworks-and-modelling.md#diátaxis-daniele-procida) (Frameworks and architecture modelling)
