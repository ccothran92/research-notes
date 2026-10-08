[Overview](../README.md) › [Concepts explained](README.md)

# When the docs live in a central wiki

## Short answer

This page applies the hybrid model from [Where docs live](../topics/where-docs-live.md) to a specific question: if the architecture docs live in a central wiki, should the code link to them?

**Yes, link, in both directions, from the places people and tools already look.** But a link only solves *findability*. It does not solve *drift*. Three rules make links work:

1. **Each fact has one canonical home.** The other location links to it and never restates it.
2. **Anything that must constrain a code change is canonical in the repo.** That covers invariants, absences, boundaries and commands. Most coding agents, many contractors and anyone without wiki access cannot follow a link into an SSO-protected wiki. A constraint they can't read is a constraint they won't respect.
3. **Every link carries context:** what is behind it, who owns it, and when it was last reviewed. A reader should be able to judge staleness before clicking.

## Decide what is canonical where

| Content | Canonical home | Why |
|---|---|---|
| Business context, stakeholders, roadmap, organisational narrative | Wiki | Edited by non-engineers; not tied to code versions |
| Cross-team onboarding hub | Wiki or portal home page | Many audiences |
| Invariants, absences, boundaries, codemap | Repo (ARCHITECTURE.md) | Changes with code, reviewed in PRs, readable by agents |
| ADRs | Repo (`docs/adr/`) | Immutable, numbered, reviewed |
| API and event contracts | Repo spec files | Tooling: code generation, validation, breaking-change checks |
| System-level C4 L1 and L2 | System repo or model, rendered in the portal | Generated and versioned; the wiki embeds or links the render |
| Runbooks | Runbook repo or tool, linked from alarms | On-call starts from an alarm, not a README |
| Design proposals still under discussion | Wherever collaboration works best (often the wiki) | Archived to the repo as an ADR or design doc once accepted |
| Compliance policies and control definitions | GRC tool or compliance-owned wiki space | Owned by compliance; architecture docs reference control IDs |

## Where to put the links: code side

**1. README, "Architecture" section near the top**, not in a link dump at the bottom:

```markdown
## Architecture
Orders is the system of record for customer orders: checkout, payment orchestration and order
status. It publishes order events consumed by Fulfilment, Notifications and Finance.

- **Binding for code changes:** [ARCHITECTURE.md](./ARCHITECTURE.md) — codemap, boundaries, invariants
- **Business context and roadmap:** [Orders — overview (wiki, SSO)](https://acme.atlassian.net/wiki/spaces/ORD/pages/123456789)
  · owner `team-orders` · last reviewed 2026-08-30
- **Decisions:** [docs/adr/](./docs/adr/) · **API contract:** [openapi/orders-v2.yaml](./openapi/orders-v2.yaml)
```

**2. ARCHITECTURE.md header**, stating the split explicitly:

```markdown
> Scope of this document: how the code is shaped and what must stay true.
> Business context, stakeholders and roadmap: [Orders overview (wiki)](https://acme.atlassian.net/wiki/spaces/ORD/pages/123456789).
> If the wiki and this file disagree about a constraint, this file wins; please fix the wiki.
```

**3. The service catalog entry** (Backstage `catalog-info.yaml`), so the links appear on the service's portal page:

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: orders-api
  annotations:
    backstage.io/techdocs-ref: dir:.
  links:
    - url: https://acme.atlassian.net/wiki/spaces/ORD/pages/123456789
      title: Orders overview (wiki)
    - url: https://acme.atlassian.net/wiki/spaces/ORD/pages/223344556
      title: Orders runbooks
spec:
  type: service
  lifecycle: production
  owner: group:team-orders
  system: orders
  providesApis: [orders-api-v2]
```

**4. AGENTS.md**, so agents know the wiki exists and that it does not override the repo:

```markdown
## Architecture
- Binding constraints: ARCHITECTURE.md (Invariants, Boundaries). Read before structural changes.
- Business context lives in the wiki (SSO; you probably cannot read it). Nothing there overrides ARCHITECTURE.md.
```

**5. The PR template**, so updates are prompted at the moment of change:

```markdown
- [ ] This PR changes a boundary, contract or invariant → updated ARCHITECTURE.md / added an ADR
- [ ] This PR changes something described in the [Orders wiki overview](https://acme.atlassian.net/wiki/spaces/ORD/pages/123456789) → wiki updated or issue filed
```

**6. ADRs**, linking back to the discussion that produced them: `Discussion: [RFC: async payments (wiki)](https://acme.atlassian.net/wiki/spaces/ORD/pages/334455667)`.

**7. Alarm descriptions**, because on-call starts from the alarm, not the README:

```hcl
alarm_description = "Payments DLQ is not empty. Runbook: https://acme.atlassian.net/wiki/spaces/ORD/pages/223344556"
```

## Where to put the links: wiki side

Put a standard header panel at the top of every architecture page:

| Field | Value |
|---|---|
| Code | `github.com/acme/orders-api` · `github.com/acme/orders-system-architecture` |
| Canonical on this page | Business context, stakeholders, roadmap, glossary |
| Canonical in the repo (not copied here) | Invariants, codemap, contracts, ADRs → ARCHITECTURE.md |
| Owner | team-orders |
| Last reviewed · interval | 2026-08-30 · every 90 days |
| Valid as of | orders-api v4.12 |

For diagrams on the wiki, embed the image rendered by CI or the portal and link to its source file. Never paste a hand-made PNG that has no editable source.

When a page has been migrated, replace its body with a "Moved to <link>" banner rather than deleting it, so old links keep working.

## Link hygiene

- **Use stable URLs.** Prefer wiki URLs that contain the page ID, which survive renames. Avoid title-based URLs, search-result links and links to a space's home page.
- **Check access.** A link from a repo that 2,000 engineers can read to a restricted wiki space is a dead end. Either open the page up or move the content.
- **Check links automatically.** Use a link checker such as lychee in CI for repo and public links. Wiki links need an authenticated check through the wiki's API, run on a schedule.
- **Avoid personal spaces.** Pages under personal wiki spaces disappear when people leave.

## If you want to move off the wiki: a migration path

1. **Phase 1 (weeks).** Add links in both directions and the header panels. Nothing moves yet.
2. **Phase 2.** Move *binding* content (invariants, absences, codemap, ADRs) into the repo, after someone has verified it. Replace each wiki section with a one-paragraph summary and a link.
3. **Phase 3.** Publish the repo docs in the portal. The wiki page becomes a front door for business context, with links to the portal. Archive old pages with "Moved" banners.

**Don't bulk-migrate.** Copying every wiki page into Git imports stale content and gives it false credibility, because "it's in the repo" reads as "it's current". Migrate only what someone has checked.

## Anti-patterns

- "See Confluence" with no specific page.
- The same diagram maintained in both places.
- A wiki architecture page with no link back to the code.
- Pasting whole wiki pages into AGENTS.md "so the agent can see them". This bloats every session; carry over only the binding one-liners, each with a pointer.

**Read more**

- Backstage catalog descriptor format (the `links` field): <https://backstage.io/docs/features/software-catalog/descriptor-format/>
- Backstage TechDocs: <https://backstage.io/docs/features/techdocs/>
- lychee link checker: <https://github.com/lycheeverse/lychee>
- Diátaxis (why runbooks and explanations are separate documents): <https://diataxis.fr/>

---

[Concepts explained](README.md): [Absences](absences.md) · [Invariants](invariants.md) · [C4 diagrams](c4-diagrams.md) · [Conventions](non-default-conventions.md) · **Wiki links** · [Anti-content](anti-content.md) · [ARCHITECTURE.md excerpts](architecture-md-excerpts.md)
