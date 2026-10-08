[Overview](../README.md)

# Where should docs live? (Q3)

| Option | Discoverability | Versioned with code | Ownership | Drift risk | Best for |
|---|---|---|---|---|---|
| In-repo (`ARCHITECTURE.md`, `/docs`, `/docs/adr`) | Low alone; high via portal | Yes | High (CODEOWNERS) | Lowest | Repo architecture, ADRs, agent context |
| Dedicated architecture repo | Medium | Not with described code | Needs named owner | High for repo facts; OK for system views | System views, system ADRs, shared model |
| Wiki (Confluence/Notion) | High for non-engineers | No | Low (orphan pages) | Highest | Org narrative, onboarding hubs, early drafting |
| Docs-as-code portal (TechDocs, MkDocs, Docusaurus) | High | Yes (source in repos) | Inherits repo owner | Low | Default publishing layer |
| Hybrid: repo source + portal + wiki front door | High | Yes | High | Low if wiki only links | Most enterprises |

**Evidence for docs-as-code.** Spotify's TechDocs has "5000+ documentation sites and around 10000 average daily hits". Spotify was deliberately opinionated, declaring TechDocs "the one way to produce documentation". Google's g3doc puts docs beside source, which "establishes de facto ownership" and lets engineers "update the code and its associated documentation in the same change". [Established]

**The competing position.** Google writes most design docs in Google Docs for collaboration, and Spotify found its Golden Path tutorials "the most challenging to manage within a docs-like-code environment". Wikis win for many-commenter proposals, audiences without repo access, and cross-repo narrative. **Rule [Synthesis]:** draft anywhere. Approved artifacts and living architecture descriptions live in Git, and wiki pages link to them rather than forking them.

**When the docs stay in a wiki.** What is canonical where, where to put links on each side, and a migration path: [When the docs live in a central wiki](../concepts/wiki-links.md).

---

## Sources

- [Malte Ubl: "Design Docs at Google" (2020)](../sources/architecture-practice.md#malte-ubl-design-docs-at-google-2020) (Architecture documentation practice)
- [Software Engineering at Google, Ch. 10 "Documentation" (Winters, Manshreck & Wright, O'Reilly, 2020)](../sources/architecture-practice.md#software-engineering-at-google-ch-10-documentation-winters-manshreck--wright-oreilly-2020) (Architecture documentation practice)
- [Backstage: TechDocs documentation](../sources/docs-as-code.md#backstage-techdocs-documentation) (Docs-as-code and developer portals)
- [Backstage blog: "Announcing TechDocs: Spotify's docs-like-code plugin for Backstage" (Sep 8, 2020)](../sources/docs-as-code.md#backstage-blog-announcing-techdocs-spotifys-docs-like-code-plugin-for-backstage-sep-8-2020) (Docs-as-code and developer portals)
- [Docs Like Code: "Ten tips for maintaining a long-term relationship with docs like code" (n.d.)](../sources/docs-as-code.md#docs-like-code-ten-tips-for-maintaining-a-long-term-relationship-with-docs-like-code-nd) (Docs-as-code and developer portals)
