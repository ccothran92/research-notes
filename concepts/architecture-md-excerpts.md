[Overview](../README.md) › [Concepts explained](README.md)

# Putting it together: two ARCHITECTURE.md excerpts

These show the concepts ([absences](absences.md), [invariants](invariants.md), [non-default conventions](non-default-conventions.md), [anti-content](anti-content.md)) in their natural setting, filled in from the [ARCHITECTURE.md template](../templates/architecture-md.md): short, stable, and pointing elsewhere for anything volatile.

## Infrastructure: `platform-network/ARCHITECTURE.md`

```markdown
---
owner: group:platform-network
system: cloud-platform
last_reviewed: 2026-09-02
review_interval: 90d
status: current
---
# platform-network — Architecture

## Purpose
Private connectivity, DNS and inspected internet egress for every workload account.
Consumers attach through `modules/workload-vpc` (in terraform-modules); they never edit this repo.

## Codemap
- `modules/transit-gateway/` — Transit Gateway and route tables per environment class (prod, nonprod, shared).
- `modules/inspection-vpc/` — internet egress and east-west inspection: NAT gateways, Network Firewall, domain allow-lists.
- `modules/dns/` — Route 53 Resolver endpoints and rules, shared to workload accounts through RAM.
- `modules/ipam/` — IPAM pools per region and environment class. The only place CIDRs are defined.
- `live/<account>/<region>/<layer>/` — instantiations; one Terraform state per account, region and layer.
- `policies/` — Conftest rules run against every plan.

## Boundaries
| Direction | Counterparty | Interface | Notes |
|---|---|---|---|
| In | Workload accounts | TGW attachment created by `modules/workload-vpc` | Associated to a route table by the `env` tag |
| In | Security team | Firewall allow-lists in `allowlists/*.yaml` | Security CODEOWNERS approve |
| Out | On-premises | Direct Connect gateway, BGP | Shared with network engineering |
| Out | Internet | Inspection VPC only | |

## Architecture invariants
- **Prod and non-prod workloads cannot route to each other.** Separate route tables; no
  propagation between them. Enforced by: Conftest rule on propagations.
- **All internet egress from workload VPCs leaves through the inspection VPC.** Enforced by:
  SCP (no internet gateways in Workloads OU) and default routes on the TGW.
- **No CIDR literals outside `modules/ipam/`.** Enforced by: Conftest rule.
- **Sandbox accounts get no TGW attachment.** Why: sandboxes are untrusted. Enforced by: the
  vending pipeline skips attachment for the Sandbox OU.

## Deployment (shape only)
Hub in the network-hub account, two regions; an inspection VPC per region across three AZs.
Sizes, throughput settings and costs live in `live/` and on the network dashboard.

## Changing this system
- Route-table or firewall-policy changes: plan attached to the PR, two approvals including Security.
- Decisions: `docs/adr/` — 0002 centralised egress · 0005 IPAM pools per environment class ·
  0009 Network Firewall over third-party appliances.

## Known gaps
- Target (not yet true): IPv6 dual-stack for new VPCs (ADR-0011, NET-412).

## Not in this document
CIDR allocations (IPAM console / `modules/ipam/`), firewall rule lists (`allowlists/`),
runbooks (linked from alarms), costs (network cost dashboard).
```

## Data and ML: `churn-model/ARCHITECTURE.md`

```markdown
---
owner: group:data-science-retention
system: churn-prediction
last_reviewed: 2026-08-18
review_interval: 90d
status: current
---
# churn-model — Architecture

## Purpose
Defines churn features, then trains, evaluates and registers the churn model. Ingestion and the
lake zones are owned by data-platform; this repo reads curated tables and owns the feature
groups `customer_activity_v3` and `customer_billing_v2`.

## Codemap
- `src/features/` — feature definitions, one module per feature group; writes online and offline stores.
- `src/training/` — `build_training_set()` (point-in-time), model code, evaluation.
- `pipelines/train/` — SageMaker Pipeline: build set → validate → train → evaluate → bias check → gate → register.
- `pipelines/score/` — nightly Batch Transform; writes scores for the CRM sync.
- `inference/` — real-time endpoint handler; reads online features by `customer_id`.
- `notebooks/` — exploration only. Nothing here runs in production or is imported by `src/`.

## Boundaries
| Direction | Counterparty | Contract |
|---|---|---|
| In | `curated.orders_v2`, `curated.sessions_v1`, `curated.subscriptions_v4` | Glue Catalog schemas, owned by data-platform |
| Out | orders-api | Endpoint `churn-realtime`; request/response schema in `inference/schema.json`; latency SLO in the catalog |
| Out | CRM | Nightly scores; schema in `pipelines/score/output_schema.json` |

## Architecture invariants
- **Training sets are built only by `build_training_set()` (point-in-time on `event_time`).**
- **Feature groups are written only by jobs in `src/features/`** (one definition for online and offline).
- **Only `Approved` model versions are deployed to ml-prod.**
- **Model scores are never used as features or labels** (ADR-0007, feedback loops).
- **Nothing in this repo reads `raw.*` tables** (PII).

## Cross-cutting concepts
- Reproducibility: each model package records commit, image digest, input Iceberg snapshot IDs and hyperparameters.
- Promotion policy: a candidate must beat the current Approved version on AUC and calibration and
  pass the bias check; thresholds in `pipelines/train/quality_gate.yaml`.
- Monitoring: Model Monitor data-quality and model-quality schedules on the endpoint; alarms link to runbooks.

## Not in this document
Current model metrics and hyperparameters (Model Registry), instance types (pipeline config),
feature lists with types (generated: `docs/generated/features.md`), costs (cost dashboard).
```

---

[Concepts explained](README.md): [Absences](absences.md) · [Invariants](invariants.md) · [C4 diagrams](c4-diagrams.md) · [Conventions](non-default-conventions.md) · [Wiki links](wiki-links.md) · [Anti-content](anti-content.md) · **ARCHITECTURE.md excerpts**
