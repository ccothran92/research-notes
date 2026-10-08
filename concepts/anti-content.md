[Overview](../README.md) › [Concepts explained](README.md)

# Anti-content, with the why

## Six ways content goes wrong

This page expands the short anti-content list in [Level of detail](../topics/level-of-detail.md#anti-content-what-does-not-belong-in-architecturemd), with the mechanism behind each item. Every item below causes harm through one or more of these mechanisms. They are the *why*.

| Code | Mechanism | What happens |
|---|---|---|
| **M1** | Duplication leads to contradiction | Two copies diverge. Readers can't tell which is right, and some trust the wrong one. |
| **M2** | Volatility shortens shelf life | Content that changes faster than the doc is reviewed is wrong for most of its life. |
| **M3** | Wrong mode or audience | Procedures buried in an explanation aren't found under pressure, and aren't updated by the people who own them. |
| **M4** | False authority | Docs are read as truth, so stale or aspirational content drives confident wrong decisions in incidents, audits and agent edits. |
| **M5** | Exposure | Operational identifiers and policy detail gain reconnaissance value and spread wider than the systems they describe. |
| **M6** | Attention and token cost | Every line competes for a human reader's attention and an AI agent's context window. Repository overviews have been measured as not helping agents while raising cost by over 20% (Gloaguen et al., 2026; see [AI agents: evidence](../ai-agents/evidence.md)). |

"Derivable" doesn't mean "useless". Many of the items below are valuable when *generated* by a tool into `docs/generated/` and linked. They are anti-content only when written and maintained by hand.

---

### 7.1 Copies of API specs, event schemas and database DDL

**Looks like:** a Markdown table of every `/v2/orders` endpoint and field; the full `OrderPlaced` JSON pasted in; `CREATE TABLE orders (...)`.

**Why it hurts (M1, M4).** The spec file is what code generators, request validators and breaking-change checks use. The copy is checked by nothing. The first time someone adds a field, the two disagree, and a consumer team building from the doc integrates against the wrong shape.

**Instead:** "orders-api v2 — OpenAPI in `openapi/orders-v2.yaml`; additive changes only; breaking changes require a new major path (ADR-0021)." Render the spec in the portal.

### 7.2 Runbook steps and rollback commands

**Looks like:** "If the payments DLQ fills: 1. run `aws sqs ...` 2. redrive ..."

**Why it hurts (M3, M2, M4).**

- On-call engineers start from an alarm at 3 a.m. and need a short procedure. They find it faster in a runbook linked from that alarm.
- Commands change with tooling, and the people who change them (SREs) don't review ARCHITECTURE.md.
- A stale command run under pressure can make an incident worse.

**Instead:** the architecture doc explains the design and the failure mode: *"failed payments land in the DLQ; redriving is safe because consumers are idempotent on `eventId`."* It then links the runbook. Why it is safe is architecture; how to do it is a runbook.

### 7.3 Volatile numbers

**Looks like:** "orders-api runs 6 tasks of 1 vCPU / 2 GB; Aurora is db.r6g.2xlarge; Lambda has 512 MB; autoscaling triggers at 70% CPU."

**Why it hurts (M2, M4).** Autoscaling changes the task count many times a day, while the doc is reviewed perhaps twice a year, so the number is wrong almost all the time. Nobody updates prose for a one-line Terraform change, so it stays wrong. Readers plan capacity or debug incidents from it.

**Instead:** state the *shape* and point to where the values live: "orders-api scales horizontally on CPU and request count (values in `infra/ecs.tf`; live values on the service dashboard). Tasks are stateless, so any task can be replaced."

### 7.4 Account IDs, ARNs, IP addresses and hostnames

**Looks like:** "Prod is 123456789012; the payments role is `arn:aws:iam::123456789012:role/payments-worker`; Aurora is at 10.20.3.14."

**Why it hurts (M2, M5).**

- These values are volatile: accounts get split, roles get renamed, and an Aurora instance's IP address changes on failover, which is why you connect through endpoints.
- AWS does not classify account IDs as secret. But a document that maps IDs to environments, roles and trust relationships is a ready-made reconnaissance map, and docs travel further than IaC does.

**Instead:** name things by role ("the orders-prod account", "the payments-worker role"). Resolve real values from the account catalog or IaC outputs.

### 7.5 Pasted IAM policies, Terraform and Helm values

**Looks like:** 40 lines of IAM JSON in the security section.

**Why it hurts (M1, M4, M5).** Security reviewers approve the pasted copy while the deployed policy drifts. The real policy also has tooling, such as IAM Access Analyzer and policy tests, that a pasted copy never gets.

**Instead:** state the *intent* in one sentence, which a reviewer can actually judge: "payments-worker can read secrets under `/payments/*` and publish to the orders bus; nothing else (see `infra/iam.tf`)." Then test the real policy against that intent.

### 7.6 File-by-file inventories

**Looks like:** a tree of 200 files, each with a one-line description.

**Why it hurts (M2, M6).**

- It breaks with every refactor.
- It adds nothing a file tree doesn't already show.
- It buries the ten lines that matter.
- Agents can list files themselves, and overview content has been measured as unhelpful to them.

**Instead:** a module-level codemap that says what each part is for, where its boundaries are, and where common changes go: "To add a payment method, implement `PaymentProvider` in `src/providers/`."

### 7.7 Hyperlinks to specific files and lines

**Looks like:** `[handler](https://github.com/acme/orders-api/blob/main/src/http/orders.ts#L42)`

**Why it hurts (M2).**

- Line anchors break on the next edit.
- File links break on rename or move.
- Links to `main` show a different version from the reader's checkout.

Symbol names, by contrast, are searchable in every IDE and code-search tool, and survive moves.

**Instead:** name the symbol: "`CheckoutService.placeOrder` in `src/domain/checkout`."

### 7.8 Rules a linter or formatter already enforces

**Looks like:** "Use 2-space indentation; no unused variables; sort imports."

**Why it hurts (M1, M6).** The tool is the authority, so a prose copy can only ever disagree with it. In agent context files this "lint leakage" was the most common problem found, in 62% of files in one 2026 study ([AI agents: evidence](../ai-agents/evidence.md)). It costs tokens every session and adds nothing, because the linter fails the build anyway.

**Instead:** "Run `make lint` before committing." If a lint rule encodes an architectural reason, such as an import boundary, document the *reason* once.

### 7.9 Target state presented as current

**Looks like:** "All services communicate asynchronously via EventBridge." In fact two of them still call each other synchronously.

**Why it hurts (M4).** This is the most dangerous item on the list:

- Incident responders follow the wrong mental model.
- Auditors receive evidence of controls that don't exist yet, which is a finding waiting to happen.
- Agents implement against a design that isn't there.

**Instead:** separate the two explicitly: "Today: orders-api → pricing-svc is synchronous REST. *Target (not yet true):* pricing moves to async by Q2 2027 (ADR-0031, ORD-1123)."

### 7.10 People as owners

**Looks like:** "Owner: Priya Shah."

**Why it hurts (M2, M4).** People change teams and leave, and the doc is orphaned. Review requests go to someone who no longer works on the system, and CODEOWNERS entries fail quietly.

**Instead:** a team handle or catalog group (`group:team-orders`), plus the on-call rotation for urgent questions.

### 7.11 Console screenshots

**Looks like:** screenshots of the ECS service page or the SageMaker Studio console.

**Why it hurts (M2, M5).**

- They can't be searched or diffed in review.
- They go stale when the console is redesigned.
- They often show account IDs, ARNs and resource names.

**Instead:** describe the configuration by pointing to code. If a picture genuinely helps, use a diagram-as-code.

### 7.12 Cost figures

**Looks like:** "This system costs $14k per month."

**Why it hurts (M2, M4).** The figure changes monthly but gets quoted in budget decisions as if it were current.

**Instead:** describe the cost *drivers* ("cost scales mainly with Aurora instance size and endpoint instance-hours; batch scoring is a small share") and link a cost dashboard filtered by the `cost-center` tag.

### 7.13 Current ML model metrics and hyperparameters

**Looks like:** "Current model: XGBoost, max_depth=6, AUC 0.81."

**Why it hurts (M1, M2).** These change with every retrain. The Model Registry already stores metrics and lineage for each version, so the doc is wrong after the next approval and readers compare new models against a stale baseline.

**Instead:** state the *policy* and point to the record: "Production uses the latest Approved version in model package group `churn-xgb`; each version carries its metrics, bias report and lineage. A candidate must beat the current version on AUC and calibration and pass the bias check (thresholds in `pipelines/train/quality_gate.yaml`)."

### 7.14 Hand-maintained copies of generated tables

**Looks like:** a hand-typed table of Terraform module inputs and outputs; a list of dependency versions.

**Why it hurts (M1, M2).** Generators already exist for these (terraform-docs, SBOM tools), and a hand copy diverges at the first change.

**Instead:** generate the table into `docs/generated/` in CI and link to it.

### 7.15 History narrative instead of current design

**Looks like:** "In 2022 we moved from SNS to EventBridge after an outage, then in 2023 we added..."

**Why it hurts (M3, M6).** Readers have to reconstruct the current state from a story, and history mixed with the present drifts. ADRs exist to keep each decision dated, immutable and separate ([Nygard](../sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011)).

**Instead:** "Fan-out uses EventBridge (ADR-0003, which supersedes ADR-0001)."

---

A useful device is an explicit **"Not in this document"** section at the end of each ARCHITECTURE.md, listing what readers might expect and where it actually lives. It stops people adding it back, and tells readers where to go. See [the ARCHITECTURE.md excerpts](architecture-md-excerpts.md).

**Read more**

- matklad on codemaps and not linking files: <https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html>
- AWS account identifiers (and their sensitivity): <https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-identifiers.html>
- terraform-docs: <https://terraform-docs.io/>
- Configuration-smells study (lint leakage and other problems in agent files): <https://arxiv.org/abs/2606.15828>
- Michael Nygard, "Documenting Architecture Decisions": <https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions>

---

[Concepts explained](README.md): [Absences](absences.md) · [Invariants](invariants.md) · [C4 diagrams](c4-diagrams.md) · [Conventions](non-default-conventions.md) · [Wiki links](wiki-links.md) · **Anti-content** · [ARCHITECTURE.md excerpts](architecture-md-excerpts.md)
