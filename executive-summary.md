[Overview](README.md)

# Executive summary

Require one short, stable, in-repo architecture.md for each deployable system. Also require ADRs for significant decisions and one system-level doc set, built on a shared model, for each product or platform. Make humans and AI agents read the same canonical docs, with thin agent-context files acting only as routers. Do not mandate long templates for every team. The evidence says long or generated docs cost more and help less.

## TL;DR

- **Write less, closer to the code, and make it stable.** A good architecture.md is a map, not an atlas. It covers the problem, a codemap, boundaries, invariants (including "what must never happen"), and cross-cutting concerns. It lives in the repo and is revisited on triggers and roughly twice a year, not synced line by line with code. Decisions go into immutable ADRs. Volatile detail (instance counts, schemas, API specs, runbook steps) is generated or linked, never hand-copied.
- **In multi-repo estates, split ownership by C4 level.** The system-level doc owns context and container views and the cross-repo contracts. Each repo owns its own component-level map and its invariants. Neither restates the other: they reference shared IDs from one model, such as a Structurizr system catalog or a Backstage catalog, so drift becomes a detectable CI failure rather than a silent contradiction.
- **Treat AI agents as a new reader, not a new doc type.** Keep AGENTS.md/CLAUDE.md to about 100–200 lines covering commands, non-obvious conventions, do-not-touch zones and pointers into the docs. Do not paste architecture overviews into them. Controlled 2026 studies find that context files often don't improve task success and can raise cost by more than 20%, and that LLM-generated ones do worse than human-written ones. Measure before you mandate.

## Ten opinionated recommendations

1. **One architecture.md per deployable system, about 150–500 lines.** matklad: "keep it short: every recurring contributor will have to read it… the shorter it is, the less likely it will be invalidated." [Established]
2. **Document the absences.** Make "Architecture Invariants" and "Boundaries" mandatory sections. Invariants are often "expressed as an absence of something, and it's pretty hard to divine that from reading the code" (matklad). [Established; agent value is Synthesis] Worked examples: [Absences](concepts/absences.md), [Architecture invariants](concepts/invariants.md).
3. **ADRs are mandatory for architecturally significant decisions. Design docs are optional and tied to ambiguity.** ADRs are short, numbered and superseded rather than edited (Nygard, 2011). Google's guidance: if a design doc is just an "implementation manual" with no trade-offs, skip it. [Established]
4. **Docs live in the repo of the thing they describe and are rendered centrally** through a portal (Backstage TechDocs, MkDocs). Wikis are for non-versioned, cross-org content. [Established for engineering docs; contested, see [Where docs live](topics/where-docs-live.md)]
5. **Use C4 levels 1–2 for system docs, level 3 only where it pays, and level 4 never by hand.** [Established] Worked L1–L3 diagrams: [C4 diagrams](concepts/c4-diagrams.md).
6. **One model, many views, for multi-repo systems.** Element identities come from a single catalog. Repo docs extend it and never redefine it. [Established tooling; enterprise rollout evidence is anecdotal]
7. **Freshness metadata and an owner on every architecture page, enforced in CI**, plus doc-change prompts on PRs that touch boundary-defining files. [Established]
8. **Never hand-copy volatile or derivable facts.** Generate or link schemas, specs, dependency lists, deployment counts and IAM policies. [Established] Why each one hurts: [Anti-content](concepts/anti-content.md).
9. **Agent-context files are routers, not encyclopedias. They are human-written and tested**, and linted for the known "configuration smells". [Emerging; mixed 2025–2026 evidence]
10. **Measure usage and correctness, not existence.** "File exists" mandates produce theater. DORA links documentation *quality*, not quantity, to performance. [Synthesis]

---

## Sources

- [matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)](sources/architecture-practice.md#matklad-alex-kladov-architecturemd-feb-6-2021) (Architecture documentation practice)
- [Malte Ubl: "Design Docs at Google" (2020)](sources/architecture-practice.md#malte-ubl-design-docs-at-google-2020) (Architecture documentation practice)
- [Software Engineering at Google, Ch. 10 "Documentation" (Winters, Manshreck & Wright, O'Reilly, 2020)](sources/architecture-practice.md#software-engineering-at-google-ch-10-documentation-winters-manshreck--wright-oreilly-2020) (Architecture documentation practice)
- [Goodreads: quote page for the same chapter (freshness dates)](sources/architecture-practice.md#goodreads-quote-page-for-the-same-chapter-freshness-dates) (Architecture documentation practice)
- [Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)](sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011) (Architecture documentation practice)
- [ADR explainers (secondary)](sources/architecture-practice.md#adr-explainers-secondary) (Architecture documentation practice)
- [Structurizr: Enterprise usage / Scope / enterprise workspace example](sources/frameworks-and-modelling.md#structurizr-enterprise-usage--scope--enterprise-workspace-example) (Frameworks and architecture modelling)
- [Glen Thomas: "Mapping Complex Distributed Systems with C4 Diagrams and Structurizr DSL" (Aug 27, 2025)](sources/frameworks-and-modelling.md#glen-thomas-mapping-complex-distributed-systems-with-c4-diagrams-and-structurizr-dsl-aug-27-2025) (Frameworks and architecture modelling)
- [DORA: "Documentation quality" capability](sources/dora.md#dora-documentation-quality-capability) (Documentation and organisational performance (DORA))
- [Gloaguen, Mündler, Müller, Raychev & Vechev: "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv Feb 2026; ICLR 2026)](sources/agent-context-research.md#gloaguen-mündler-müller-raychev--vechev-evaluating-agentsmd-are-repository-level-context-files-helpful-for-coding-agents-arxiv-feb-2026-iclr-2026) (AI agent-context files: empirical research)
- ["Configuration Smells in AGENTS.md Files: Common Mistakes in Configuring Coding Agents" (arXiv Jun 2026)](sources/agent-context-research.md#configuration-smells-in-agentsmd-files-common-mistakes-in-configuring-coding-agents-arxiv-jun-2026) (AI agent-context files: empirical research)
