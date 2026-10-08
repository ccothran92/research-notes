[Overview](../README.md)

# How to maintain and review (Q5)

The core mechanisms are CODEOWNERS on architecture files, PR-triggered update prompts, freshness metadata checked in CI, triggered plus semiannual review, diagram-as-code, and usage metrics. The concrete checklist is in the [playbook](#maintenance-and-review-playbook) below. Two positions frame it:

- **Google:** freshness blocks with owner and reviewed date, which trigger reminders (e.g., after three months untouched). "Last reviewed by…" bylines "led to increased adoption".
- **matklad:** "Don't try to keep it synchronized with code. Instead, revisit it a couple of times a year."

These are compatible. Keep the map stable and review it on triggers and on a calendar. Put fast-changing facts in generated artifacts.

**Diagram tooling.** Use Mermaid for repo-level diagrams, since it renders natively in GitHub/GitLab and in portals. Use Structurizr DSL when you need one model with many views across repos, and PlantUML where it is already entrenched. Visual tools (draw.io, IcePanel, Miro) are fine for workshops, but the canonical diagram must be regenerated from committed source.

## Maintenance and review playbook

### Rollout checklist

- [ ] **Tiering:**
  - T1 (revenue/regulated/high fan-in): architecture.md + ADRs + system-doc participation + quarterly review.
  - T2: architecture.md + ADRs.
  - T3: README "How it fits" + catalog entry.
- [ ] **Catalog entry** per deployable: owner group, system, lifecycle, provided/consumed APIs, TechDocs annotation.
- [ ] **CODEOWNERS** on `ARCHITECTURE.md`, `docs/adr/**`, `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.github/instructions/**`.
- [ ] **Front matter** on every architecture page: `owner`, `last_reviewed`, `review_interval`, `status` (current/target/deprecated).
- [ ] **ADR tooling:** template, monotonic numbering, auto-generated index, lint that every superseded ADR links to its successor.
- [ ] **Diagram-as-code** rendered in CI.
- [ ] **Portal** publishing with a "report issue" button on every page (TechDocs ships a ReportIssue add-on).

### CI checks (in order of value)

1. **Link and diagram-render checks** (Mermaid/Structurizr/PlantUML compile).
2. **Freshness:** warn when `last_reviewed + review_interval` has passed; block only if the owner is missing.
3. **Boundary-change prompt:** PRs touching contracts, IaC networking/IAM, new top-level directories, new topics/queues or new external dependencies get a checklist comment ("architecture.md? ADR? contract index?"). T1 requires an explicit "no doc change needed" checkbox.
4. **Contract checks:** breaking-change detection (e.g., `buf breaking`). Contract-index entries must resolve to real specs.
5. **Model consistency:** every model container maps to a catalog entity and every `consumesApis` relation appears in the model. Optionally compare with the OpenTelemetry service graph and report undeclared edges.
6. **Docs lint** (Vale/markdownlint): required headings, plus forbidden volatile patterns in architecture.md (instance types, raw IPs, account IDs, "currently N replicas").
7. **Agent-context lint:** line budget, no duplicated linter rules, no unexplained URLs, no conflicting directives ([AI agents](../ai-agents/README.md)).

### PR and review process

- Doc changes ride in the same PR as the code (the g3doc lesson).
- architecture.md changes are reviewed by the repo CODEOWNER. Contract or system-invariant changes also need a system-doc owner.
- ADRs: opened as a PR with status `Proposed`, merged as `Accepted`. A reversal is a new ADR plus a status change on the old one.
- Design reviews are async by default. Synchronous review is only for T1 cross-team changes; Google warns reviews "are also a dangerous trap of overhead".
- **Semiannual audit** per T1/T2 repo: the owner verifies five random claims, prunes, and updates `last_reviewed`. AI pre-passes are allowed, but a human signs off.

### Usage metrics (quarterly)

| Metric | Signal | Direction |
|---|---|---|
| Portal views / unique readers per entity | Is anyone reading? | Investigate zeros for T1/T2 |
| "Doc incorrect" reports and time-to-fix | Correctness, responsiveness | Fix time down |
| Stale ratio | Maintenance health | <10% for T1 |
| Onboarding time-to-first-merged-PR | Real outcome | Down |
| Zero-result searches | Findability | Down |
| Agent rework on structural mistakes | Agent-facing value | Down |

---

## Sources

- [Malte Ubl: "Design Docs at Google" (2020)](../sources/architecture-practice.md#malte-ubl-design-docs-at-google-2020) (Architecture documentation practice)
- [Software Engineering at Google, Ch. 10 "Documentation" (Winters, Manshreck & Wright, O'Reilly, 2020)](../sources/architecture-practice.md#software-engineering-at-google-ch-10-documentation-winters-manshreck--wright-oreilly-2020) (Architecture documentation practice)
- [Goodreads: quote page for the same chapter (freshness dates)](../sources/architecture-practice.md#goodreads-quote-page-for-the-same-chapter-freshness-dates) (Architecture documentation practice)
- [Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)](../sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011) (Architecture documentation practice)
- [Backstage: TechDocs documentation](../sources/docs-as-code.md#backstage-techdocs-documentation) (Docs-as-code and developer portals)
- [bowenlabs/louise-toolkit, Issue #430: "Docs: freshness dates and owners on every published page"](../sources/docs-as-code.md#bowenlabslouise-toolkit-issue-430-docs-freshness-dates-and-owners-on-every-published-page) (Docs-as-code and developer portals)
- [Octopus Deploy: "Understanding the 4 DORA Metrics and Top Findings from 2024/25 DORA Report" (n.d.)](../sources/dora.md#octopus-deploy-understanding-the-4-dora-metrics-and-top-findings-from-202425-dora-report-nd) (Documentation and organisational performance (DORA))
