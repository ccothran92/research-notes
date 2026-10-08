[Overview](../README.md) › [Concepts explained](README.md)

# Architecture invariants

## Definition

An **architecture invariant** is a property that must stay true for every valid version of the system, whatever the implementation details. If a change would break it, that change is a *design change*. It needs an ADR and the owners' agreement; a code-review comment is not enough.

[Absences](absences.md) are the negative subset of invariants. Good invariants share five properties:

1. **Implementation-independent.** They survive refactors, language changes and library upgrades.
2. **Checkable.** Someone can tell whether a change violates them, ideally a machine.
3. **Stable.** They stay true for years, not sprints.
4. **Reasoned.** They carry their *why*, so readers can judge edge cases.
5. **Consequential.** Breaking them causes real harm: corrupted data, an outage, a breach or an audit finding.

## What is *not* an invariant

| Statement | What it actually is | Why it fails as an invariant |
|---|---|---|
| "`orders-api` runs 6 tasks" | Configuration | Changes hourly with autoscaling; belongs in IaC |
| "We aim to be fully event-driven" | An aspiration | Not true today, and not checkable |
| "Use 2-space indentation" | Formatting | The formatter enforces it |
| "`orders-api` handles orders" | A description | Nothing could ever violate it |
| "Downstream timeouts are 2 seconds" | Configuration | The number changes; the *relationship* is the invariant (next tip) |

**Tip: state invariants as relationships, not numbers.** "Every downstream call on the request path has a timeout shorter than the load balancer's idle timeout" stays true when either number changes. "The timeout is 2 s" does not.

## Template

```markdown
- **<Statement in the present tense, one sentence.>**
  Why: <the harm it prevents>. Enforced by: <mechanism, or "review only">.
  Changing it: <ADR number, or "requires an ADR">.
```

"Review only" is allowed, but it should be rare. Each one is a candidate for automation.

## Examples: Orders

```markdown
## Architecture invariants

- **Only orders-api writes to the `orders` schema.** Other services get order data from events
  or the orders API. Why: one writer keeps the order state machine consistent.
  Enforced by: only the `orders_api_rw` database role has write grants.

- **Domain events are published only through the transactional outbox.** Request handlers write
  the event to the `outbox` table in the same database transaction as the order; `outbox-relay`
  then publishes it to EventBridge. Why: writing to Aurora and to EventBridge separately can
  commit one and lose the other. Enforced by: architecture test — only `outbox-relay` may import
  the EventBridge client.

- **Every event consumer is idempotent on `eventId`.** Why: EventBridge and SQS standard queues
  can deliver the same message more than once. Enforced by: the Powertools idempotency utility
  (backed by DynamoDB); contract tests deliver every event twice.

- **The request path is synchronous only for data needed to answer the shopper.** Payment capture,
  emails, invoices and fulfilment happen after order acceptance, asynchronously. Why: checkout
  latency and availability must not depend on the payment provider or SES. Enforced by:
  dependency rule forbids the payment-provider client in orders-api.

- **Each SQS queue's visibility timeout is at least six times its consumer Lambda's timeout.**
  Why: AWS recommends this ratio so a batch is not redelivered while still being processed;
  Lambda itself only rejects a visibility timeout *shorter* than the function timeout.
  Enforced by: Conftest policy on the Terraform plan.

- **`src/domain/` performs no I/O.** It imports nothing from `src/adapters/` and no AWS SDK.
  Why: domain rules stay fast to unit-test and independent of infrastructure.
  Enforced by: dependency-cruiser rule in CI.
```

## Examples: Cloud platform

```markdown
## Architecture invariants

- **Every AWS account is created by the vending pipeline.** Why: every account gets the
  baseline (logging, guardrails, network, ownership tags) from minute one. Enforced by: SCP
  allows `organizations:CreateAccount` only for the pipeline role.

- **All internet egress from workload VPCs passes through the inspection VPC.** Why: central
  logging, domain allow-listing and one place to block data exfiltration. Enforced by: no
  internet gateways (SCP); Transit Gateway route tables send 0.0.0.0/0 to the inspection VPC.

- **Every IAM role created by workload pipelines carries the `workload-boundary` permissions
  boundary.** Why: application teams can create the roles they need without being able to grant
  themselves more than the account guardrails allow. Enforced by: SCP denies `iam:CreateRole`
  unless the `iam:PermissionsBoundary` condition matches.

- **Organisation CloudTrail logs are immutable for the retention period in records policy RP-7.**
  Why: audit evidence and forensics. Enforced by: S3 Object Lock in compliance mode in the
  log-archive account; SCP denies trail changes from member accounts.

- **No IP range is used unless it was allocated from IPAM.** Why: overlapping CIDR ranges make
  Transit Gateway routing impossible later. Enforced by: `modules/workload-vpc` accepts an IPAM
  pool ID, never a literal CIDR; a nightly check compares VPC CIDRs with IPAM allocations.

- **Platform modules are backward compatible within a major version.** Why: hundreds of consumer
  stacks cannot upgrade in lockstep. Enforced by: module tests run consumer fixtures written for
  the previous minor version; breaking changes require a new major version and a migration note.
```

## Examples: Churn prediction

```markdown
## Architecture invariants

- **Training sets are built only with point-in-time joins on `event_time`.** Why: joining on the
  *latest* feature value leaks future information into training and inflates offline metrics.
  Enforced by: training code may only call `build_training_set()`, which queries the Feature
  Store offline store as of each label's timestamp; a unit test plants a leakage case.

- **Online and offline features come from one definition.** The feature pipeline writes both
  stores from the same code. Why: training/serving skew silently degrades the model.
  Enforced by: no other job has permission to write to the feature groups.

- **Only Model Registry versions with status `Approved` reach ml-prod.** Why: human sign-off on
  quality, the bias report and cost. Enforced by: the deploy pipeline is triggered by the
  approval-status change; humans have no endpoint-creation permission in ml-prod.

- **Every registered model is reproducible.** Its model package records the git commit, the
  training image digest, the Iceberg snapshot IDs of its inputs and its hyperparameters. Why:
  audit, rollback and drift debugging. Enforced by: the register step fails if any is missing.

- **Curated and downstream data contain tokenized identifiers only.** Why: limits exposure of
  personal data. Enforced by: Lake Formation tags on PII columns; a Glue Data Quality rule on
  curated tables.
```

## Invariant or convention? A quick test

| Question | Invariant | Convention |
|---|---|---|
| Who is affected when it's broken? | Other containers, other teams, data consumers | Mostly the module and its reviewers |
| How bad is it? | Corruption, outage, breach, audit finding | Inconsistency, rework, review friction |
| How do you change it? | ADR plus owners' agreement | Team agreement plus an [AGENTS.md](../templates/agents-md.md) update |
| Scope | A system or container boundary | Inside one codebase |

**A borderline case.** "Money is integer minor units" is a *convention* inside one service. It is an *invariant of the `OrderPlaced` event contract*, though, because every consumer would misread a float. Write each rule where it binds: in the event schema for consumers, and in AGENTS.md for authors.

**Read more**

- Lambda and SQS (visibility-timeout guidance): <https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-configure.html>
- Powertools for AWS Lambda, idempotency: <https://docs.powertools.aws.dev/lambda/python/latest/utilities/idempotency/>
- Transactional outbox pattern: <https://microservices.io/patterns/data/transactional-outbox.html>
- IAM permissions boundaries: <https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html>
- S3 Object Lock: <https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html>
- SageMaker Feature Store: <https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store.html>
- SageMaker Model Registry approval status: <https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry-approve.html>
- Lake Formation tag-based access control: <https://docs.aws.amazon.com/lake-formation/latest/dg/tag-based-access-control.html>
- Architecture-test tools: dependency-cruiser <https://github.com/sverweij/dependency-cruiser> · ArchUnit <https://www.archunit.org/> · import-linter <https://import-linter.readthedocs.io/> · Conftest <https://www.conftest.dev/>

---

[Concepts explained](README.md): [Absences](absences.md) · **Invariants** · [C4 diagrams](c4-diagrams.md) · [Conventions](non-default-conventions.md) · [Wiki links](wiki-links.md) · [Anti-content](anti-content.md) · [ARCHITECTURE.md excerpts](architecture-md-excerpts.md)
