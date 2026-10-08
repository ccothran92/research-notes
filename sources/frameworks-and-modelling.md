[Overview](../README.md) › [Sources](README.md)

# Frameworks and architecture modelling (Part 2)

## C4 model: FAQ
<https://c4model.com/diagrams/faq>

**Type:** Framework documentation (Simon Brown; living) · **Label:** Established · **Used in:** [When to write](../topics/when-to-write.md), [Level of detail](../topics/level-of-detail.md), [Multi-repo topology](../topics/multi-repo.md)

This page supports three points in the report:

- Component-level and code-level diagrams change often or go stale quickly.
- Context views can be built from existing service catalogs.
- Container views can be derived from logs or OpenTelemetry data.

That last point is the basis of the report's recommendation to *detect* drift from traces rather than replace declared intent with them.

## C4 model: Component diagram
<https://c4model.com/diagrams/component>

**Type:** Framework documentation · **Label:** Established · **Used in:** [Level of detail](../topics/level-of-detail.md)

C4 itself does not recommend component diagrams by default. It says to create them only when they add value and to consider generating them. The report's position of "C4 L3 only where it pays, L4 never by hand" rests on this.

## arc42 FAQ (Gernot Starke et al., Leanpub)
<https://leanpub.com/arc42-faq/read>

**Type:** Framework FAQ (living) · **Label:** Established · **Used in:** [Level of detail](../topics/level-of-detail.md)

This source advises investing in crosscutting concepts (arc42 §8) rather than detailed building blocks (§5) or runtime scenarios (§6). That advice is the basis for the report's "use arc42 as a checklist, not a 12-section mandate" recommendation.

## Diátaxis (Daniele Procida)
<https://diataxis.fr/>

**Type:** Documentation framework (living) · **Label:** Established · **Used in:** [Level of detail](../topics/level-of-detail.md), [Example 4: library/SDK](../examples/library-sdk.md)

Diátaxis sorts documentation into four modes: tutorials, how-to guides, reference and explanation. The report uses it mainly to decide what to *exclude*. An architecture.md is explanation plus a reference-style codemap, so tutorials, how-tos and runbooks belong elsewhere.

## Canonical: "Diátaxis, a new foundation for Canonical documentation" (n.d.)
<https://ubuntu.com/blog/diataxis-a-new-foundation-for-canonical-documentation>

**Type:** Engineering blog (adoption case) · **Label:** Established · **Used in:** [Level of detail](../topics/level-of-detail.md)

This is evidence that Diátaxis has been adopted at organisational scale (Canonical/Ubuntu), not just proposed.

## Structurizr: Usage recommendations
<https://structurizr.com/help/usage-recommendations>

**Type:** Tool documentation (living) · **Label:** Established (tooling) · **Used in:** [Multi-repo topology](../topics/multi-repo.md)

This page recommends one workspace per software system plus a landscape-scoped workspace. It is the basis of the report's "one model, many views" pattern for multi-repo estates.

## Structurizr: Enterprise usage / Scope / enterprise workspace example
- Enterprise usage: <https://docs.structurizr.com/workspaces/enterprise>
- Scope: <https://docs.structurizr.com/workspaces/scope>
- Enterprise workspace example: <https://github.com/structurizr/structurizr.github.io/blob/main/workspaces/04-enterprise.md>

**Type:** Tool documentation · **Label:** Established (tooling) · **Used in:** [Executive summary](../executive-summary.md), [Multi-repo topology](../topics/multi-repo.md)

These pages describe the enterprise pattern. People and systems are defined once in a shared system catalog. Each team's workspace extends that catalog, and a central landscape view is generated from the team workspaces. The pattern is well documented. Evidence on what it costs and returns at hundreds of teams is anecdotal (see report [Open questions](../topics/open-questions.md)).

## edirand: `structurizr-enterprise-example` (GitHub)
<https://github.com/edirand/structurizr-enterprise-example>

**Type:** Community example repo · **Label:** Emerging · **Used in:** [Multi-repo topology](../topics/multi-repo.md)

This is a worked implementation of the Structurizr enterprise pattern and is useful as a starter. It is a single community example, not a reference architecture.

## Glen Thomas: "Mapping Complex Distributed Systems with C4 Diagrams and Structurizr DSL" (Aug 27, 2025)
<https://blog.glen-thomas.com/architecture/2025/08/27/mapping-complex-distributed-systems-with-c4-diagrams-and-structurizr-dsl.html>

**Type:** Practitioner blog · **Label:** Emerging · **Used in:** [Executive summary](../executive-summary.md), [Multi-repo topology](../topics/multi-repo.md)

This is a recent practitioner account of modelling a distributed system in Structurizr DSL. It supports the claim that the pattern is practical. It is one author's experience.
