[Overview](../README.md)

# Topology B: multi-repository / multi-application

## What goes where

| Content | System-level doc | Repo-level architecture.md |
|---|---|---|
| C4 L1 context/landscape | **Owns** | Links; may show generated crop |
| C4 L2 containers | **Owns** | Own container + direct neighbors, generated |
| C4 L3 components | Never | **Owns** (if needed) |
| Contract index | **Owns** the index | Owns specs it produces; lists consumed contracts by ID |
| Cross-repo flows | **Owns** 2–5 critical flows | References flow IDs |
| Trust boundaries, data classification | **Owns** system view | States local enforcement points |
| Ownership | Rendered from catalog | `catalog-info.yaml` in repo |
| Shared libraries | Lists with owner and support policy | Declares usage and constraints |
| ADRs | System-wide (protocols, tenancy, identity) | Local |

## Preventing duplication and contradiction

- **One model, referenced by ID.** Structurizr recommends one workspace per software system plus a landscape-scoped workspace. Its enterprise pattern defines people and systems once in a shared **system catalog**, has each team's workspace `extends` it, and runs `generate system-landscape` to aggregate a central view. [Established tooling] Worked example: [one model, many views](../concepts/c4-diagrams.md#45-keeping-the-levels-consistent-one-model-many-views).
- **Alternative: the service catalog as model.** Backstage entities (Component, API, System, Domain, Resource) with `providesApis`/`consumesApis` relations require less modeling effort but give weaker diagrams.
- **Detect drift from runtime.** The C4 FAQ suggests building context views from "existing system/service catalogs" and container views by "parsing log files or using OpenTelemetry data". Use traces to *detect* undeclared edges, not to replace declared intent.
- **Rule [Synthesis]:** neither level describes another repo's internals. Each refers to the other by catalog ID.

## Shared libraries, runtimes, contracts

- **Shared libraries** are catalog components with an owner, a semver and support policy (N and N-1), a deprecation window, and a *scope limit* ("cross-cutting plumbing only; no domain logic"). The classic failure is a "commons" library that becomes a distributed monolith.
- **Multiple runtimes:** a deployment view per environment *class*, with runtime constraints written as [invariants](../concepts/invariants.md) (cold starts, timeouts, statelessness, egress).
- **Contracts:** specs live in the producer repo and are published to a registry. The index records owner, consumers, sync/async, delivery semantics and compatibility policy.

## Source of truth

| Fact | Source of truth | Doc behavior |
|---|---|---|
| Repo structure | Code | Stable shape only |
| Contracts | Producer spec files | Link; CI breaking-change check |
| Ownership | Catalog | Render |
| Topology | IaC | Generate/review against IaC |
| Cross-system relations | Shared model | Generate; compare declared vs observed |
| Rationale | ADRs | Supersede, don't edit |

---

## Sources

- [C4 model: FAQ](../sources/frameworks-and-modelling.md#c4-model-faq) (Frameworks and architecture modelling)
- [Structurizr: Usage recommendations](../sources/frameworks-and-modelling.md#structurizr-usage-recommendations) (Frameworks and architecture modelling)
- [Structurizr: Enterprise usage / Scope / enterprise workspace example](../sources/frameworks-and-modelling.md#structurizr-enterprise-usage--scope--enterprise-workspace-example) (Frameworks and architecture modelling)
- [edirand: structurizr-enterprise-example (GitHub)](../sources/frameworks-and-modelling.md#edirand-structurizr-enterprise-example-github) (Frameworks and architecture modelling)
- [Glen Thomas: "Mapping Complex Distributed Systems with C4 Diagrams and Structurizr DSL" (Aug 27, 2025)](../sources/frameworks-and-modelling.md#glen-thomas-mapping-complex-distributed-systems-with-c4-diagrams-and-structurizr-dsl-aug-27-2025) (Frameworks and architecture modelling)
