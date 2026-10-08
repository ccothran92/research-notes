[Overview](../README.md)

# Sources

Annotated sources for this report, compiled October 7, 2026.

**How to read the entries.** Each entry gives the following:

- the link;
- the source type;
- an evidence label that matches the report: **Established**, **Emerging**, **Vendor**, **Evidence** (empirical study) or **Weak**;
- the report sections (§) that rely on it;
- a short annotation covering what the source says, why it matters and any caveats.

**Link provenance.** Links in Parts 1–8 were retrieved during the original research run or confirmed by search while compiling this list. Part 9 lists tool and standard homepages that are named in the report but not used as evidence; those come from general knowledge and were not re-checked. "n.d." means the source shows no publication date. Living documentation pages are marked "living".

## Source lists

- Part 1: [Architecture documentation practice](architecture-practice.md)
- Part 2: [Frameworks and architecture modelling](frameworks-and-modelling.md)
- Part 3: [Docs-as-code and developer portals](docs-as-code.md)
- Part 4: [Documentation and organisational performance (DORA)](dora.md)
- Part 5: [Software supply chain](supply-chain.md)
- Part 6: [AI agent-context files: standards and vendor guidance](agent-context-guidance.md)
- Part 7: [AI agent-context files: empirical research](agent-context-research.md)
- Part 8: [AI-assisted doc/code drift detection](drift-detection.md)
- Part 9: [Tools and standards (homepages)](tools.md)

## Note on sources (from the report)

Sources are cited inline by author/organization, title and date:

- matklad, "ARCHITECTURE.md" (Feb 2021); rust-analyzer architecture.md; Backstage architecture overview.
- Google: Malte Ubl, "Design Docs at Google" (2020); *Software Engineering at Google*, ch. 10.
- Michael Nygard, "Documenting Architecture Decisions" (2011).
- Frameworks and tooling: the C4 model site and FAQ; arc42 and its FAQ; Diátaxis; Structurizr docs; Backstage TechDocs.
- DORA research.
- Supply chain: SLSA v1.0 levels; Sigstore docs; NTIA (2021) and CISA (2026) SBOM minimum elements.
- Agent-context sources: agents.md; the Agentic AI Foundation announcement (Dec 2025); Anthropic Claude Code docs; GitHub Copilot docs; OpenAI "Harness engineering" (Feb 2026); the arXiv studies dated in [AI agents](../ai-agents/README.md).

## Named in the research brief but not used as evidence

The brief asked for these books to be prioritised. The report did not draw on them directly; its framework claims rest on the free primary sources above. If you need book-level backing for the topology and trade-off sections, these are the gaps to fill:

- **Clements, Bachmann, Bass, Garlan, Ivers, Little, Merson, Nord & Stafford, *Documenting Software Architectures: Views and Beyond*, 2nd ed. (Addison-Wesley, 2010).** This is the SEI's views-based approach and the academic grounding for "document the views your stakeholders need."
- **Ford, Richards, Sadalage & Dehghani, *Software Architecture: The Hard Parts* (O'Reilly, 2021).** It covers trade-off analysis for distributed systems: service granularity, shared code and data ownership. It is directly relevant to Example 2 (shared libraries, no shared DB).
- **Bhatti, Corleissen, Lambourne, Nunez & Waterhouse, *Docs for Developers: An Engineer's Field Guide to Technical Writing* (Apress, 2021).** It covers developer-documentation craft, maintenance and measurement, and is relevant to [Maintenance and review](../topics/maintenance-and-review.md) and [Maintenance and review](../topics/maintenance-and-review.md).

*(Book links omitted. Use your library or the publisher's catalogue.)*

## Corrections and caveats found while compiling the sources

1. **Claude Code does not read AGENTS.md natively.** Its memory docs say it reads CLAUDE.md. In report [AI agents](../ai-agents/README.md), change "keep tool-specific files as thin shims or symlinks" to make the CLAUDE.md shim (import or symlink) **mandatory** wherever Claude Code is used.
2. **OpenAI's Harness case study started from a Codex-written AGENTS.md.** Report [AI agents](../ai-agents/README.md) presents that case as support for human-authored, short routers. The "short router" lesson holds; the "human-authored" lesson is not demonstrated by that source. The human-authorship recommendation rests on Gloaguen et al.
3. **Gloaguen et al. versions differ.** The ICLR abstract says "no improvement"; an earlier arXiv version reported about +4% for developer-written files. Cite a specific version.
4. **CISA 2026 SBOM:** coverage reports the field count going from 7 to 17 and a new digital-signature field. Use Appendix A of the guidance PDF as the authority for exact element names in policy-as-code.
5. **Two sources are weak and replaceable:** the Goodreads quote page (replace with abseil.io) and the louise-toolkit GitHub issue (anecdotal).
6. **Secondary explainers** (Medium ADR post, Octopus, Skillwright, daily.dev) can be dropped without losing any claim. Each is covered by a primary source above.
