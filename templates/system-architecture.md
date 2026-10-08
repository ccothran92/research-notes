[Overview](../README.md) › [Templates](README.md)

# System-level doc template (multi-repo)

```markdown
---
owner: group:<system-architects>
system: <catalog-system-id>
model: ./workspace.dsl               # diagrams generated from here
last_reviewed: YYYY-MM-DD
review_interval: 90d
---
# <System> — System Architecture
## 1. Scope, goals, non-goals         <!-- ½ page; non-goals mandatory -->
## 2. Landscape / context (C4 L1)     <!-- generated -->
## 3. Containers (C4 L2)              <!-- generated; every box = catalog entity -->
## 4. Communication rules             <!-- sync/async policy, delivery semantics, idempotency, timeouts -->
## 5. Contract index                  <!-- ID, type, producer, consumers, compatibility, spec link -->
## 6. Key flows (2–5)                 <!-- business-critical + failure/compensation -->
## 7. Trust boundaries & data classification <!-- enforcement-point table; link evidence -->
## 8. Deployment topology (shape)     <!-- per environment class; runtime-constraint table -->
## 9. Shared libraries & platforms    <!-- owner, scope limit, support policy -->
## 10. Cross-cutting concepts         <!-- identity, observability, tenancy, resilience -->
## 11. System ADRs                    <!-- index -->
## 12. Risks & technical debt         <!-- ranked, owned -->
## 13. Glossary                       <!-- resolves naming conflicts across repos -->
<!-- NOT here: repo internals, runbooks, specs, IaC numbers -->

See also: [Multi-repo topology](../topics/multi-repo.md) and [Example 2](../examples/web-app-services.md) for a filled-in version, and [C4 diagrams](../concepts/c4-diagrams.md) for worked L1, L2 and deployment views.
