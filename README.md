# Architecture Documentation Standards: Research Notes

Research notes on **architecture documentation standards for a multi-team enterprise**: when to write architecture docs, why, where they should live, how detailed they should be, how to keep them current, and what changes when AI coding agents read them as well as humans. The notes cover single-repo and multi-repo estates, include worked GOOD/BAD examples and copy-ready templates, and back each claim with annotated sources labelled by strength of evidence (Established, Emerging, Vendor, Evidence, Weak, Synthesis).

Researched and compiled October 2026. Start with the executive summary, then go to the topic you need.

## Directory

- [Executive summary](executive-summary.md): the one-paragraph answer, TL;DR and ten recommendations

### Core topics

- [Decision framework](topics/decision-framework.md): situation → what to produce, where, how deep, who owns it; gaps in the premise
- [When to write](topics/when-to-write.md): triggers for writing, and when not to
- [Why it matters](topics/why-it-matters.md): what each reader needs; evidence of value (DORA); the theater test
- [Where docs live](topics/where-docs-live.md): in-repo vs architecture repo vs wiki vs portal
- [Level of detail](topics/level-of-detail.md): C4, arc42, ADRs, design docs, Diátaxis; what does NOT belong
- [Maintenance and review](topics/maintenance-and-review.md): review cadence, diagram tooling, rollout checklist, CI checks, metrics
- [Single-repo topology](topics/single-repo.md): what goes in architecture.md; rust-analyzer and Backstage critiques
- [Multi-repo topology](topics/multi-repo.md): system vs repo docs, one shared model, shared libraries, source of truth
- [Open questions](topics/open-questions.md): where the evidence is weak

### AI agents

- [AI agents](ai-agents/README.md): agent-context files: AGENTS.md, CLAUDE.md, Copilot
- [AI agents: evidence](ai-agents/evidence.md): the 2025–2026 studies, compared
- [Structuring agent context](ai-agents/structuring-agent-context.md): AGENTS.md vs architecture.md, progressive disclosure
- [AI-generated docs and drift detection](ai-agents/generated-docs-and-drift.md): risks of generated docs; detecting drift with AI

### Worked examples

- [Example 1: CI/CD platform](examples/cicd-platform.md): orchestration, Artifactory, supply-chain security
- [Example 2: web app + services](examples/web-app-services.md): UI + backend + 5 services on ECS and Lambda
- [Example 3: data/ML pipeline](examples/data-ml-pipeline.md): when the code is not the architecture
- [Example 4: library/SDK](examples/library-sdk.md): when the API is the architecture
- [Example 5: polyglot monorepo](examples/polyglot-monorepo.md): one repo, many systems

### Concepts explained

Each concept made concrete with three running AWS example systems (Orders, a cloud platform, churn prediction).

- [Concepts explained](concepts/README.md): the three example systems and a vocabulary table
- [Absences](concepts/absences.md): the deliberate "nevers" code cannot show
- [Architecture invariants](concepts/invariants.md): what must stay true, and what enforces it
- [C4 diagrams](concepts/c4-diagrams.md): L1, L2 and L3 for all three systems; one model, many views
- [Non-default conventions](concepts/non-default-conventions.md): local rules a newcomer or agent would get wrong
- [When the docs live in a central wiki](concepts/wiki-links.md): what is canonical where, and where the links go
- [Anti-content, with the why](concepts/anti-content.md): fifteen things that don't belong, and why
- [Two ARCHITECTURE.md excerpts](concepts/architecture-md-excerpts.md): the concepts together

### Templates

- [Template: ARCHITECTURE.md](templates/architecture-md.md): repo-level
- [Template: system architecture doc](templates/system-architecture.md): multi-repo system level
- [Template: AGENTS.md](templates/agents-md.md): agent router

### Sources

- [Sources](sources/README.md): how to read entries, corrections and caveats, plus nine source lists by subject

## Section map

The report and its sources refer to sections by number (§). Those references are now links, but if you meet a § number elsewhere, it maps here:

| § | Page |
|---|---|
| §1 | [Executive summary](executive-summary.md) |
| §2 | [Decision framework](topics/decision-framework.md) |
| §3 | [Decision framework](topics/decision-framework.md) |
| §4 | [When to write](topics/when-to-write.md) |
| §5 | [Why it matters](topics/why-it-matters.md) |
| §6 | [Where docs live](topics/where-docs-live.md) |
| §7 | [Level of detail](topics/level-of-detail.md) |
| §8 | [Maintenance and review](topics/maintenance-and-review.md) |
| §9 | [Single-repo topology](topics/single-repo.md) |
| §10 | [Multi-repo topology](topics/multi-repo.md) |
| §11 | [Worked examples](examples/README.md) |
| §12 | [Maintenance and review](topics/maintenance-and-review.md) |
| §13 | [AI agents](ai-agents/README.md) |
| §13.1 | [AI agents](ai-agents/README.md) |
| §13.2 | [AI agents: evidence](ai-agents/evidence.md) |
| §13.3 | [Structuring agent context](ai-agents/structuring-agent-context.md) |
| §13.4 | [Structuring agent context](ai-agents/structuring-agent-context.md) |
| §13.5 | [Structuring agent context](ai-agents/structuring-agent-context.md) |
| §13.6 | [AI-generated docs and drift detection](ai-agents/generated-docs-and-drift.md) |
| §13.7 | [AI-generated docs and drift detection](ai-agents/generated-docs-and-drift.md) |
| §14 | [Templates](templates/README.md) |
| §14.1 | [Template: ARCHITECTURE.md](templates/architecture-md.md) |
| §14.2 | [Template: system architecture doc](templates/system-architecture.md) |
| §14.3 | [Template: AGENTS.md](templates/agents-md.md) |
| §15 | [Open questions](topics/open-questions.md) |
| §16 | [Sources](sources/README.md) |

The original single-file report, annotated source list and concepts companion are kept in [archive/](archive/).
