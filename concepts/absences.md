[Overview](../README.md) › [Concepts explained](README.md)

# Absences

## Definition

An **absence** is an architectural property stated as something the system deliberately does *not* do, contain or depend on. Examples: the UI never touches the database directly; workload accounts have no internet gateway; training never reads production transactional databases.

Absences are a subset of [invariants](invariants.md); you can think of them as negative invariants. They deserve their own name because code cannot show them. You can search a repo for every call to DynamoDB, but you cannot search for the call that was left out on purpose.

The idea comes from [matklad's ARCHITECTURE.md essay](../sources/architecture-practice.md#matklad-alex-kladov-architecturemd-feb-6-2021). It points out that many important invariants are expressed as the absence of something, and that an absence is very hard to discover by reading code.

## Why absences matter

**Code shows what is there, not what was excluded.** Suppose a newcomer reads `inventory-svc` and sees no calls to the orders database. They cannot tell whether that is a policy or a coincidence.

**Architecture erodes mostly by addition.** Systems tend to degrade one reasonable-looking addition at a time, for example:

- a "quick" read of another service's table;
- a cron job placed in the wrong service;
- an IAM wildcard added to unblock a release.

Each of these passes review because nothing written down forbids it.

**Newcomers and AI agents fill gaps from public examples.** Public tutorials routinely show browsers talking to DynamoDB through Amplify, Lambda functions with open internet access, and notebooks writing straight to production buckets. Without a stated absence, the public default wins. *(recommendation)*

## How to write one

Each absence answers four questions:

- What never happens?
- Why not?
- What should be done instead?
- What enforces it?

A good example:

```markdown
- **The web app never calls AWS services directly.** No AWS SDK in the browser bundle and no
  Cognito identity-pool credentials. *Why:* every authorization decision lives in orders-api;
  browser-held AWS credentials would bypass it. *Instead:* add an endpoint to orders-api.
  *Enforced by:* a dependency-cruiser rule forbidding `@aws-sdk/*` under `apps/web/`;
  no identity pool exists in the account.
```

A bad version of the same idea: *"Security is important; avoid insecure patterns in the frontend."* It names no prohibited act, offers no alternative and has no enforcement, so nobody can tell whether a given change violates it.

## Examples: Orders (UI, API, backend)

| Absence | Why it's deliberate | What someone would "helpfully" add | Do this instead | Enforced by |
|---|---|---|---|---|
| The SPA never calls AWS services directly | Authorization lives in `orders-api` | Amplify or DynamoDB calls from React "to save a round-trip" | Add an API endpoint | Import rule on `@aws-sdk/*`; no Cognito identity pool |
| No service reads another service's datastore | Each team can change its schema without breaking others | `inventory-svc` querying the orders Aurora database for a report | Consume events or call the owning API | Per-service database users and IAM roles |
| `payments-worker` never calls `orders-api` synchronously | Avoids a call cycle; payments keep working if `orders-api` is degraded | An HTTP call to "fetch the latest order" | Put the needed fields in the `OrderPlaced` event | No network path; review |
| No in-memory session or cart state in `orders-api` tasks | Deployments and autoscaling replace tasks at any moment | A local cart cache "for speed" | DynamoDB-backed carts; any cache must be safe to lose | Review; a resilience test kills tasks mid-checkout |
| No scheduled jobs inside `orders-api` | A scheduler inside a horizontally scaled service runs once per task | `node-cron` for a nightly cleanup | EventBridge Scheduler triggering a Lambda | Import rule on scheduler libraries |
| No business state kept between Lambda invocations | Execution environments are reused unpredictably, then discarded | A module-level "last processed ID" variable | Persist it in DynamoDB | Review |

A note on the last row: creating an SDK *client* at module scope is encouraged, because it reuses connections. That is caching a client, not keeping business state (see [Non-default conventions](non-default-conventions.md#examples-orders-ui-api-backend)).

## Examples: Cloud platform

| Absence | Why it's deliberate | What someone would "helpfully" add | Do this instead | Enforced by |
|---|---|---|---|---|
| No internet gateways in workload accounts | All egress is inspected and logged centrally | An internet gateway "just for one vendor API" | Request a domain allow-list entry in Network Firewall | SCP denies `ec2:CreateInternetGateway` and `ec2:AttachInternetGateway` in the Workloads OU |
| No IAM users or long-lived access keys | Leaked static keys are a leading breach path | An IAM user for a CI tool | OIDC federation for CI; Identity Center for humans | SCP denies `iam:CreateUser` and `iam:CreateAccessKey` outside break-glass roles |
| No standing admin access to production | Limits the blast radius of a compromised identity | A permanent AdministratorAccess assignment for on-call | Time-bound elevation through an approval workflow | Identity Center assignments are managed only by the pipeline |
| No VPC peering | Peering meshes bypass the inspection path and break IP planning | A peering link between two app VPCs | Transit Gateway route tables | SCP denies `ec2:CreateVpcPeeringConnection` |
| No public S3 buckets | Data exposure | A public bucket for a static site | CloudFront with origin access control | Account-level S3 Block Public Access; SCP stops anyone from turning it off |
| No console-made changes in production | Creates drift from IaC that nobody can see | Hot-fixing a security group in the console | Change the Terraform and run the pipeline | Read-only console roles in prod; drift detection |

## Examples: Churn prediction

| Absence | Why it's deliberate | What someone would "helpfully" add | Do this instead | Enforced by |
|---|---|---|---|---|
| No raw personal data (PII) outside the raw zone | Minimises who can see personal data | Joining email addresses into a feature table | Use the tokenized `customer_id` | Lake Formation tags on PII columns; Glue Data Quality rule |
| Training never reads production OLTP databases | Protects production load; makes training data reproducible | Querying an Aurora read replica from a training job | Use curated snapshots | No network path from ML accounts to the Orders VPC; IAM |
| No production artifact comes from a notebook | Notebooks are not reviewed, versioned or reproducible | Registering a model straight from SageMaker Studio | Run the SageMaker Pipeline from `main` | IAM denies `sagemaker:CreateModelPackage` to Studio execution roles |
| No deployment of unapproved model versions | Requires human sign-off on quality and fairness | Deploying the "obviously better" model directly | Approve it in the Model Registry; the deploy pipeline does the rest | Deploy pipeline reads only Approved packages; humans can't create endpoints in prod |
| Model scores are never fed back as features or labels | Retention offers change the very behaviour the model predicts, which creates a feedback loop | Adding `last_churn_score` as a feature | Raise an ADR; use experiment holdouts | Feature-group review |

## How to find your absences

Ask the people who review the most code:

- What would you reject in review even if every test passed?
- What did we remove on purpose, and what change would bring it back?
- What do new joiners try to add in their first month?
- Which past incident would recur if someone undid the fix?
- What do our SCPs, IAM deny statements and network rules forbid, and do we know why?

That last question is often the richest. Deny statements are absences someone has already written down in policy language. Document the *why* next to them.

**Read more**

- matklad, "ARCHITECTURE.md": <https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html>
- AWS service control policies: <https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html>
- AWS whitepaper, *Organizing Your AWS Environment Using Multiple Accounts*: <https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html>
- S3 Block Public Access: <https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html>

---

[Concepts explained](README.md): **Absences** · [Invariants](invariants.md) · [C4 diagrams](c4-diagrams.md) · [Conventions](non-default-conventions.md) · [Wiki links](wiki-links.md) · [Anti-content](anti-content.md) · [ARCHITECTURE.md excerpts](architecture-md-excerpts.md)
