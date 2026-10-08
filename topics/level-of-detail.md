[Overview](../README.md)

# What level of detail (Q4)

| Framework | Adopt | Skip / adapt | Why |
|---|---|---|---|
| **C4** | L1 Context, L2 Container; deployment and dynamic views for runtime-heavy systems | L3 only for complex containers, ideally generated; L4 never by hand | C4 on component diagrams: "Recommended? No, only create… if you feel they add value, and consider automating their creation" |
| **arc42** | As a checklist: Context (3), Solution Strategy (4), shallow Building Blocks (5), Deployment (7), Crosscutting Concepts (8), Decisions (9 → ADRs), Risks (11), Glossary (12) | Full 12 sections per repo; Quality (10) except tier-1; Runtime (6) except non-obvious flows | arc42 FAQ: prefer crosscutting concepts "over detailed building blocks (section 5) or runtime scenarios (section 6)" |
| **ADRs (Nygard/MADR)** | Title/Status/Context/Decision/Consequences; MADR options for contested decisions | Editing accepted ADRs; ADRs for reversible choices | Nygard: keep superseded ADRs; numbers "will not be reused" |
| **Google design docs** | Context, goals *and non-goals*, alternatives, cross-cutting concerns | Docs without ambiguity; pasted API/schema definitions | Those "quickly get out of date" |
| **Diátaxis** | Use it to *exclude*: architecture.md is explanation plus a reference-style codemap | Tutorials/how-tos/runbooks inside architecture docs | Mixing modes causes bloat |

Worked C4 L1–L3 diagrams for three AWS systems, with what each level leaves out: [C4 diagrams](../concepts/c4-diagrams.md).

## Anti-content: what does NOT belong in architecture.md

- Copies of API specs, protobufs, GraphQL schemas or DDL. Link to them instead.
- Runbook steps, on-call procedures, rollback commands.
- Volatile numbers: instance counts, task sizes, autoscaling thresholds, account IDs, IPs, dependency versions.
- File-by-file inventories: "a map of a country, not an atlas of maps of its states" (matklad).
- Hyperlinks to specific files. Name the module and let readers search; matklad: "Do *not* directly link them".
- Lint and style rules enforced by tooling.
- Target state without a "Target (not yet true)" label.

Each item, with the mechanism behind it and what to write instead: [Anti-content, with the why](../concepts/anti-content.md).

---

## Sources

- [matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)](../sources/architecture-practice.md#matklad-alex-kladov-architecturemd-feb-6-2021) (Architecture documentation practice)
- [Malte Ubl: "Design Docs at Google" (2020)](../sources/architecture-practice.md#malte-ubl-design-docs-at-google-2020) (Architecture documentation practice)
- [Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)](../sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011) (Architecture documentation practice)
- [C4 model: FAQ](../sources/frameworks-and-modelling.md#c4-model-faq) (Frameworks and architecture modelling)
- [C4 model: Component diagram](../sources/frameworks-and-modelling.md#c4-model-component-diagram) (Frameworks and architecture modelling)
- [arc42 FAQ (Gernot Starke et al., Leanpub)](../sources/frameworks-and-modelling.md#arc42-faq-gernot-starke-et-al-leanpub) (Frameworks and architecture modelling)
- [Diátaxis (Daniele Procida)](../sources/frameworks-and-modelling.md#diátaxis-daniele-procida) (Frameworks and architecture modelling)
- [Canonical: "Diátaxis, a new foundation for Canonical documentation" (n.d.)](../sources/frameworks-and-modelling.md#canonical-diátaxis-a-new-foundation-for-canonical-documentation-nd) (Frameworks and architecture modelling)
