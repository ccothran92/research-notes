[Overview](../README.md)

# Decision framework: situation → what, where, depth

| Situation / trigger | What to produce | Where | Depth | Owner |
|---|---|---|---|---|
| Small repo (<~2k LOC), single obvious purpose | README "How it fits" section; no architecture.md | README.md | 5–15 lines | Repo team |
| Repo passes ~10k LOC, >1 team contributes, or onboarding >1 week | architecture.md | `/ARCHITECTURE.md` or `/docs/architecture.md` | 150–500 lines; C4 L1+L2 (L3 only if non-obvious) | Repo tech lead (CODEOWNERS) |
| Ambiguous design, real alternatives, cross-team impact | Design doc (context, goals/non-goals, design, alternatives, cross-cutting) | PR-reviewed Markdown, or collaborative doc archived to repo on approval | 1–3 pages ("mini") to 10–20 pages | Author(s) |
| Decision costly to reverse (datastore, protocol, runtime, trust boundary, tenancy) | ADR | `/docs/adr/NNNN-title.md` in the constrained repo; system-wide in system repo | 1–2 pages | Decider |
| New externally consumed contract | Spec (OpenAPI/AsyncAPI/protobuf) + ≤10 lines in architecture.md (owner, versioning, consumers) | Producer repo; registered in catalog | Spec as code | Producer team |
| Multi-repo product/platform (≥3 deployables) | System-level doc: landscape, containers, contract index, flows, trust boundaries, system ADRs | System repo rendered in portal; Structurizr DSL or catalog model | 5–15 pages, generated diagrams | Named system architect |
| Compliance/audit need | Control-mapping: trust boundaries, data classification, enforcement points, evidence links | Security section of system doc | Tables; link evidence | Security architect + owner |
| Incident revealed an architectural misunderstanding | Update the violated invariant/boundary; ADR if decision changes | architecture.md / ADR | Smallest preventive diff | Incident owner |
| AI coding agents used in repo | AGENTS.md router (+ tool shims) | Root + nested per package | ~100–200 lines max | Repo team |
| Prototype/spike changing weekly | **Nothing beyond README + decision-log bullets** | README | Minimal | — |
| Facts derivable from code/infra | **Don't write; generate or link** | `docs/generated/` | Automated | CI |

## Gaps and challenges to the premise

- **"Architecture docs" are three different artifacts, and standards that conflate them fail.** *Descriptive maps* (architecture.md, C4 views) are living and short. *Decision records* (ADRs) are immutable history. *Proposals* (design docs/RFCs) are point-in-time and mostly stale after implementation. Set separate rules for each.
- **At hundreds of teams, the bottleneck is the catalog, not the template.** "Which system is this, who owns it, what does it depend on" is best answered by machine-readable ownership (e.g., Backstage `catalog-info.yaml`). Prose should hang off catalog entities.
- **Assuming AI agents need *more* documentation is, on current evidence, probably wrong.** The strongest 2026 controlled study found repository overviews in context files "are not helpful" and costs up over 20%. Agents need less but sharper context.
- **Runbooks, threat models and data-classification docs are separate artifacts.** Architecture docs should link to them, not absorb them. Absorbing them is the main cause of bloat.
- **Uniform depth is a mistake.** Tier documentation burden by risk and fan-in, not by org chart.
- **"Source of truth" is a per-fact decision**: code for structure, specs for contracts, catalog for ownership, ADRs for rationale, IaC for topology. The doc points to these and states what no machine artifact can: intent, invariants and trade-offs.

---

## Sources

- [Gloaguen, Mündler, Müller, Raychev & Vechev: "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv Feb 2026; ICLR 2026)](../sources/agent-context-research.md#gloaguen-mündler-müller-raychev--vechev-evaluating-agentsmd-are-repository-level-context-files-helpful-for-coding-agents-arxiv-feb-2026-iclr-2026) (AI agent-context files: empirical research)
