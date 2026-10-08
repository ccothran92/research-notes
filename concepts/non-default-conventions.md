[Overview](../README.md) › [Concepts explained](README.md)

# Non-default conventions

## Definition

A **non-default convention** is a local rule that a competent engineer new to this codebase would get wrong on the first try, because it differs from the common default. The same goes for an AI coding agent, whose defaults come from public code.

A rule qualifies only if all three of these hold:

1. **It differs from the default.** That means the language, framework or AWS default, or what most public examples do.
2. **No tool catches it.** No compiler, formatter, linter or CI policy flags it. If a tool does catch it, document the tool, not the rule.
3. **Getting it wrong costs more than a review comment.** For example a bug, a data problem, a security gap, or a PR rejected after hours of work.

## Why the code alone can't teach these

Most codebases contain both the old way and the new way while a migration is in progress, for example:

- IRSA and EKS Pod Identity;
- AWS SDK v2 and v3;
- offset pagination and cursor pagination.

A newcomer, or an agent pattern-matching on nearby files, copies whichever version it sees first. A written convention says which one is current. If both versions are live, write both and give the direction of travel: *"New code: Pod Identity. Existing IRSA namespaces: leave them unless you are migrating them (see MIGRATION.md)."*

## How to phrase and place them

- **Phrase each convention as an imperative one-liner** that includes the alternative: "Use X, not Y."
- **Put the one-liner in AGENTS.md** (or the tool-specific equivalent; see the [AGENTS.md template](../templates/agents-md.md) and [Structuring agent context](../ai-agents/structuring-agent-context.md)), which is loaded every agent session.
- **Put any longer rationale elsewhere:** in ARCHITECTURE.md's cross-cutting-concerns section, or in an ADR if it was a real decision (see the [ARCHITECTURE.md template](../templates/architecture-md.md)).

## Examples: Orders (UI, API, backend)

| What people do by default | Our convention | Why |
|---|---|---|
| One DynamoDB table per entity | Cart data uses a single table; keys are prefixed (`CART#<id>`, `ITEM#<sku>`); new access patterns need a table-design review | A whole cart in one query; transactional writes across items |
| Timestamps in whatever format the library emits | ISO-8601, UTC, millisecond precision, `Z` suffix: `2026-10-07T14:03:22.120Z` | DynamoDB sort keys compare as strings; mixed precision breaks range queries |
| Money as a float or decimal | Integer minor units plus ISO 4217 code: `{"amount": 1999, "currency": "USD"}` | No rounding errors; handles currencies with 0 or 3 decimal places |
| SDK client created inside the Lambda handler | Create SDK clients at module scope and reuse them across invocations | Connection reuse cuts latency; module scope is for clients, never business state |
| AWS SDK for JavaScript v2 (still present in old code) | v3 modular clients only, e.g. `@aws-sdk/client-dynamodb` | v2 is no longer supported; smaller bundles |
| Ad-hoc error bodies like `{"error": "..."}` | RFC 9457 problem details (`application/problem+json`) with `type` values listed in `docs/errors.md` | Clients can handle errors generically |
| Offset pagination (`?page=3`) | Opaque cursors (`?cursor=...`) wrapping DynamoDB `LastEvaluatedKey` or an Aurora keyset | Offsets are slow on Aurora and not possible on DynamoDB |
| `console.log` | Powertools Logger; propagate `x-correlation-id`; never log request bodies | Searchable logs; no PII in logs |
| UI: hand-written `fetch` calls | Use the client generated from `openapi/orders-v2.yaml` in `packages/api-client`; never edit generated code | Contract drift shows up at compile time |
| Feature flags in environment variables | Flags and tunables come from AWS AppConfig via Powertools Parameters; environment variables hold resource names only | Change behaviour without a redeploy |
| Integration tests against local emulators | `make test-int` deploys an ephemeral stack to the `orders-sandbox` account | Tests exercise real IAM and EventBridge rules |

## Examples: Cloud platform (Terraform)

| What people do by default | Our convention | Why |
|---|---|---|
| Raw `aws_iam_role` resources | Use `modules/iam-role`, which adds the permissions boundary and tags. CI blocks raw roles, but knowing this saves a failed pipeline run. | Boundary invariant ([invariants](invariants.md#examples-cloud-platform)) |
| Inline JSON policy heredocs | `aws_iam_policy_document` data sources | Validated, composable, readable diffs |
| `terraform apply` from a laptop | Plan locally if you like; apply only through the pipeline | Audit trail; prod state is writable only by the pipeline role |
| Literal CIDR ranges | Allocate from IPAM through `modules/workload-vpc` | No overlaps |
| Modules referenced at `?ref=main` | Pin to a release tag, e.g. `?ref=v3.4.1` | Reproducible plans |
| Defaulting to `us-east-1` | Primary `eu-west-1`, secondary `eu-central-1`. Use `us-east-1` only where a global service requires it, e.g. ACM certificates for CloudFront. | Data residency; CloudFront certificates must be in us-east-1 |
| IRSA for pod permissions (most public examples) | EKS Pod Identity for new workloads; IRSA only in legacy namespaces being migrated | Simpler trust setup; no per-cluster OIDC provider wiring |
| `kubectl apply` to a shared cluster | Deploy through Argo CD from `deploy/` only | No drift; full audit trail |
| Free-form tags | Required on every resource: `owner`, `cost-center`, `data-classification`, `env` | Cost allocation and data handling. Tag policies enforce this only for some resource types, so set the tags everywhere anyway. |

## Examples: Churn prediction

| What people do by default | Our convention | Why |
|---|---|---|
| Hard-coded `s3://bucket/path` strings | Reference tables by Glue Catalog name (`curated.orders_v2`) and resolve the location at runtime | Lake Formation permissions apply to catalog tables; buckets move |
| Random row-level train/test split | Split by customer (`hash(customer_id)`) *and* by time (train before T, test on T) | Prevents leakage: the same customer in both sets inflates metrics |
| Free-form feature names | `<entity>__<feature>__<window>`, e.g. `customer__orders_count__30d` | Discoverable; no collisions |
| Joining on the latest feature value | Always use `build_training_set()` (point-in-time) | Upholds the leakage invariant ([invariants](invariants.md#examples-churn-prediction)) |
| `:latest` container image tags | Pin training and inference images by digest | Reproducibility |
| Logic living in notebooks | Notebooks are for exploration; anything scheduled lives in `src/` and runs in the pipeline | Reviewable and reproducible |
| Local or mixed time zones | UTC everywhere; partitions are `dt=YYYY-MM-DD` in UTC | Avoids off-by-one-day joins |
| Printing metrics to logs | Log parameters and metrics to SageMaker's managed MLflow, tagged with `git_sha` | Runs are comparable over time |

## What these look like in AGENTS.md

**Orders (`orders-api`):**

```markdown
# AGENTS.md — orders-api

## Commands
- Install: `pnpm install` · Unit tests: `pnpm test` · One file: `pnpm test -- src/domain/checkout.test.ts`
- Lint and types: `pnpm lint && pnpm typecheck`
- Integration: `make test-int` (needs `AWS_PROFILE=orders-sandbox`; deploys an ephemeral stack)

## Conventions you would otherwise get wrong
- Money: integer minor units plus currency code. Never floats.
- Timestamps: ISO-8601 UTC with milliseconds and `Z`.
- AWS SDK: v3 modular clients only, created at module scope.
- Errors: RFC 9457 problem details; allowed types are in docs/errors.md.
- Pagination: opaque cursors, never offsets.
- Logging: Powertools Logger; never log request bodies.

## Do not touch
- `packages/api-client/` — generated; run `pnpm gen:client`
- `migrations/` — human review required

## Before structural changes, read
- ARCHITECTURE.md: Invariants, Boundaries · docs/adr/README.md
```

**Platform (`platform-network`):**

```markdown
# AGENTS.md — platform-network

## Commands
- Format and validate: `make fmt validate` · Policy tests: `make conftest`
- Plan one stack: `make plan STACK=live/network-hub/eu-west-1/tgw` (read-only role)
- Never run `terraform apply`; the pipeline applies after merge.

## Conventions you would otherwise get wrong
- CIDRs come from IPAM pools in modules/ipam; never write a literal CIDR.
- IAM: use modules/iam-role and aws_iam_policy_document; no raw roles, no JSON heredocs.
- Pin module sources to release tags; never ?ref=main.
- us-east-1 only for CloudFront certificates and other global-service needs.

## Do not touch
- `live/*/security-*` — Security team CODEOWNERS
- Route-table propagation in modules/transit-gateway — read ARCHITECTURE.md first

## Before structural changes, read
- ARCHITECTURE.md: Invariants · docs/adr/0002-centralised-egress.md
```

**Churn (`churn-model`):**

```markdown
# AGENTS.md — churn-model

## Commands
- Env: `uv sync` · Tests: `uv run pytest` · Leakage tests only: `uv run pytest -m leakage`
- Validate the pipeline definition: `make pipeline-validate` (no AWS calls)

## Conventions you would otherwise get wrong
- Reference datasets by Glue table name, never s3:// paths.
- Build training data only with build_training_set(); never join on latest values.
- Split by customer hash and by time, never by random rows.
- Feature names: <entity>__<feature>__<window>.
- Pin images by digest. UTC everywhere.

## Do not touch
- `notebooks/` output is never imported by `src/`.
- Do not add model scores as features (ADR-0007).
```

## What does *not* count as a non-default convention

The following don't belong in AGENTS.md as conventions, because they are either universal defaults or already enforced by a tool:

- "Write unit tests."
- "Use meaningful names."
- "Keep functions small."
- "Follow PEP 8" or "run `terraform fmt`." A formatter enforces these, so say "run `make fmt`".
- "Use TypeScript strict mode." `tsconfig.json` enforces it.

**Read more**

- RFC 9457, Problem Details for HTTP APIs: <https://www.rfc-editor.org/rfc/rfc9457>
- DynamoDB best practices: <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html>
- EKS Pod Identity: <https://docs.aws.amazon.com/eks/latest/userguide/pod-identities.html>
- VPC IPAM: <https://docs.aws.amazon.com/vpc/latest/ipam/what-it-is-ipam.html>
- AGENTS.md format: <https://agents.md/>
- Evidence that agent files should hold only non-obvious, minimal requirements (Gloaguen et al., 2026; compared in [AI agents: evidence](../ai-agents/evidence.md)): <https://arxiv.org/abs/2602.11988>

---

[Concepts explained](README.md): [Absences](absences.md) · [Invariants](invariants.md) · [C4 diagrams](c4-diagrams.md) · **Conventions** · [Wiki links](wiki-links.md) · [Anti-content](anti-content.md) · [ARCHITECTURE.md excerpts](architecture-md-excerpts.md)
