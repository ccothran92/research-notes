[Overview](../README.md)

# Concepts explained, with AWS examples

Absences, invariants, C4 levels, non-default conventions, wiki links and anti-content, made concrete with three running AWS example systems.

This section is a companion to the [research notes](../README.md) and their [annotated sources](../sources/README.md). It can be read on its own: each concept is defined and illustrated with embedded examples, and the links are for going deeper. Compiled October 7, 2026. The original single-file version is kept in [archive/concepts-explained.md](../archive/concepts-explained.md).

Statements about AWS behaviour link to AWS documentation. Statements marked *(recommendation)* are this section's own synthesis, not an established standard.

## Pages

- [Absences](absences.md): the deliberate "nevers" that code cannot show
- [Architecture invariants](invariants.md): properties that must hold for every valid version, and how to enforce them
- [C4 diagrams](c4-diagrams.md): L1, L2 and L3 for all three systems, plus deployment views and one-model consistency
- [Non-default conventions](non-default-conventions.md): local rules a newcomer or agent would get wrong, and how they look in AGENTS.md
- [When the docs live in a central wiki](wiki-links.md): what is canonical where, and where to put the links
- [Anti-content, with the why](anti-content.md): fifteen kinds of content that don't belong, and the mechanism behind each
- [Two ARCHITECTURE.md excerpts](architecture-md-excerpts.md): the concepts together, for an infrastructure repo and an ML repo

Related pages elsewhere in these notes: [Level of detail](../topics/level-of-detail.md), [Where docs live](../topics/where-docs-live.md), [Structuring agent context](../ai-agents/structuring-agent-context.md) and the [templates](../templates/README.md).

## Three running example systems

All three systems are fictional but built from common AWS patterns. They belong to one company and depend on one another on purpose, because the hardest documentation problems sit at the seams between systems.

### Orders (UI, API, backend)

- **UI.** Shoppers use a React single-page app (SPA), served from S3 through CloudFront.
- **Core service.** The SPA calls `orders-api`, a TypeScript service on ECS Fargate that owns checkout and the order lifecycle.
- **Supporting services.** `pricing-svc` is written in Go and runs on ECS. `inventory-svc` is written in Kotlin, runs on the platform team's shared EKS cluster and keeps its data in DynamoDB.
- **Data.** Orders live in Aurora PostgreSQL.
- **After acceptance.** Once an order is accepted, the rest happens asynchronously. An outbox relay publishes domain events to an EventBridge bus, which fans out to Lambda functions for payments (through an SQS queue), notifications (via SES) and invoice PDFs (stored in S3).
- **Sign-in.** Uses a Cognito user pool owned by the separate Identity team.

### Cloud platform (infrastructure)

This is the landing zone that everything else runs in. It is all Terraform, spread across several repos.

- **Accounts and OUs.** AWS Organizations with Control Tower. Organizational units (OUs) for security, infrastructure, workloads and sandboxes.
- **Workforce sign-in.** IAM Identity Center, federated to the corporate identity provider (IdP).
- **Guardrails as code.** Service control policies (SCPs), AWS Config rules and IAM permissions boundaries.
- **Networking.** A network hub account with Transit Gateway (TGW), a central egress ("inspection") VPC with NAT gateways and AWS Network Firewall, Route 53 Resolver for hybrid DNS, and VPC IPAM for address allocation.
- **Logging.** An organisation-wide CloudTrail trail that writes to a log-archive account.
- **Shared compute.** Shared EKS clusters, deployed through Argo CD.
- **Account vending.** A Terraform-based pipeline that creates new accounts.

### Churn prediction (data analytics and ML)

This system predicts which subscribers are likely to cancel.

- **Data lake.** Data from Orders, product clickstream and billing lands in an S3 data lake with raw and curated zones, the Glue Data Catalog and Lake Formation permissions.
- **Features.** Computed on SageMaker Processing and stored in SageMaker Feature Store: the offline store for training, the online store for real-time lookups.
- **Training.** A SageMaker Pipeline trains, evaluates and registers each model in the Model Registry.
- **Deployment.** Approved models run in two places: a nightly Batch Transform job, whose scores go to the CRM, and a real-time endpoint that `orders-api` calls at checkout.
- **Monitoring.** Model Monitor watches for drift.

AWS's documentation now calls the service "Amazon SageMaker AI". This document uses "SageMaker".

---

## Vocabulary at a glance

| Term | One-line definition | Orders example | Usually lives in | Enforced by |
|---|---|---|---|---|
| Boundary | Where the system ends, and who it talks to in which direction | `orders-api` calls `pricing-svc` synchronously; payments are triggered by events | ARCHITECTURE.md; [C4](c4-diagrams.md) L1 and L2 | Network, IAM, contracts |
| [Absence](absences.md) | A deliberate "never": something the system does not do, contain or depend on | The SPA never calls AWS services directly | ARCHITECTURE.md invariants | IAM, lint rules, architecture tests |
| [Invariant](invariants.md) | A property that must hold for every valid version of the system | Only `orders-api` writes to the orders database | ARCHITECTURE.md | Database grants, IAM, tests, policy |
| [Non-default convention](non-default-conventions.md) | A local rule that a competent newcomer or AI agent would get wrong by default | Money is integer minor units plus a currency code | AGENTS.md (the rule); ARCHITECTURE.md (the why) | Review, sometimes lint |
| Configuration | A value that tunes behaviour and is expected to change | `orders-api` runs between 4 and 40 tasks | Infrastructure as code (IaC), parameter store; not docs | Pipeline |
| Requirement | What the system must do for its users | Shoppers can cancel an unpaid order | Product docs, tickets, tests | Tests |
| Decision | A choice among alternatives, with its reasons | EventBridge rather than SNS for fan-out | Architecture decision record ([ADR](../sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011)), e.g. ADR-0003 | Review |

**Rule of thumb.** Invariants and absences say what must stay true. Conventions say how routine things are done here. Configuration says how much. ADRs say why a choice was made.

---

## Link notes

**Checked while preparing these pages:**

- the C4 model pages (via the site's own index);
- the AWS Lambda page on SQS event-source configuration, including the six-times visibility-timeout recommendation and the fact that Lambda itself only enforces "function timeout ≤ visibility timeout";
- the SageMaker Model Registry approval-status page;
- matklad's essay;
- the Backstage pages;
- the two arXiv studies.

**Not individually re-checked:** the remaining AWS documentation links and the tool homepages (dependency-cruiser, ArchUnit, import-linter, Conftest, terraform-docs, lychee, Mermaid, Structurizr). These are canonical documentation pages; AWS occasionally reorganises its docs, so search the page title if a link has moved.

**Placeholders.** All company names, URLs under `acme.atlassian.net` and `github.com/acme`, account names, ADR numbers and ticket IDs are fictional.
