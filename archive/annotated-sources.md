# Annotated Sources: Architecture Documentation Standards Report

Companion to *Architecture Documentation Standards for a Multi-Team Enterprise*. Compiled October 7, 2026.

**How to read the entries.** Each entry gives the following:

- the link;
- the source type;
- an evidence label that matches the report: **Established**, **Emerging**, **Vendor**, **Evidence** (empirical study) or **Weak**;
- the report sections (§) that rely on it;
- a short annotation covering what the source says, why it matters and any caveats.

**Link provenance.** Links in Parts 1–8 were retrieved during the original research run or confirmed by search while compiling this list. Part 9 lists tool and standard homepages that are named in the report but not used as evidence; those come from general knowledge and were not re-checked. "n.d." means the source shows no publication date. Living documentation pages are marked "living".

---

## Part 1: Architecture documentation practice

### matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)
<https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html>

**Type:** Practitioner essay · **Label:** Established · **Used in:** §1, §4, §7, §9, §14

This is the origin of the ARCHITECTURE.md convention and the report's main model for the repo-level doc. Its core claims are:

- Keep the doc short so it rarely goes out of date.
- Give a codemap, a map of the country rather than an atlas.
- Name modules but don't hyperlink files, because links rot.
- Make invariants explicit, especially those expressed as the absence of something.
- Revisit the doc a couple of times a year instead of syncing it with every change.

The essay targets projects of roughly 10k–200k LOC. It is opinion from one experienced maintainer, not research. It has been very widely adopted, though, and it fits with Google's and OpenAI's guidance.

### rust-analyzer: `docs/dev/architecture.md` (pinned commit)
<https://github.com/rust-lang/rust-analyzer/blob/067b4a32dd267d2599f908fc556491be45af7176/docs/dev/architecture.md>
Repo: <https://github.com/rust-lang/rust-analyzer>

**Type:** Real-world exemplar · **Label:** Established (example) · **Used in:** §9

This is matklad's own reference implementation, and the report critiques it as a GOOD example with caveats. It has a strong codemap and labelled "Architecture Invariant" and "API Boundary" callouts. At this commit it runs to about 518 lines, it contains a TBA stub, it has no ADRs and it has little security content. The pinned URL is deliberate: the file has since moved within the repo, which shows that links into docs rot too.

### rust-analyzer: `CLAUDE.md`
<https://github.com/rust-lang/rust-analyzer/blob/master/CLAUDE.md>

**Type:** Real-world exemplar · **Label:** Emerging · **Used in:** §9

This is cited next to the architecture.md as a live example of the agent-context file of a project that already has a strong architecture doc. Its contents were not analysed in depth for the report. Read the current version before you use it as a model.

### Backstage: "Architecture overview"
<https://backstage.io/docs/overview/architecture-overview/>

**Type:** Real-world exemplar (OSS project docs) · **Label:** Established (example) · **Used in:** §9

This is the contrast example to rust-analyzer. It is a well-diagrammed tour of the frontend, backend, plugins and databases, and it keeps the threat model on a separate, linked page, which the report endorses. Its weaknesses are that it mixes concept material with how-to material and uses time-relative wording ("at this time of writing"), which the report flags as a drift smell.

### Malte Ubl: "Design Docs at Google" (2020)
<https://www.industrialempathy.com/posts/design-docs-at-google/>

**Type:** Practitioner essay (ex-Google engineer) · **Label:** Established · **Used in:** §1, §4, §6, §7, §12

This is the main source for when to write a design doc and when not to. Skip the doc when it would be only an "implementation manual" with no trade-offs, and recognise that design-doc overhead may not suit rapid prototyping. It also supplies the standard section set (context, goals and non-goals, design, alternatives, cross-cutting concerns), the warning against pasting API or schema definitions that go stale, the warning that design reviews can turn into overhead, and the observation that Google writes most design docs in Google Docs. It describes one company's culture, so calibrate it to your own.

### Software Engineering at Google, Ch. 10 "Documentation" (Winters, Manshreck & Wright, O'Reilly, 2020)
<https://abseil.io/resources/swe-book/html/ch10.html>
Community mirror: <https://github.com/xgqfrms/Software-Engineering-At-Google/blob/main/docs/ch10-documentation.md>

**Type:** Book chapter (free HTML from Google/Abseil) · **Label:** Established · **Used in:** §1, §6, §8, §12

This is the source for g3doc: docs live beside source, which establishes de facto ownership and lets one change update both code and docs. It also covers freshness metadata (owner and last-reviewed date) that triggers reminders. Cite the abseil.io version; the GitHub mirror is unofficial.

### Goodreads: quote page for the same chapter (freshness dates)
<https://www.goodreads.com/quotes/11341657-at-google-we-often-attach-freshness-dates-to-documentation-such>

**Type:** Quote aggregator · **Label:** Weak (secondary) · **Used in:** §1, §8

This was picked up during research as a secondary copy of the Google freshness-dates passage. **Replace it with the abseil.io chapter above** in any formal version of the report.

### Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)
<https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions>

**Type:** Practitioner essay; origin of the ADR format · **Label:** Established · **Used in:** §1, §4, §7, §12

This is the canonical ADR source. It defines the Title / Status / Context / Decision / Consequences structure. ADRs are numbered, short and superseded rather than edited, and numbers are never reused. Lost rationale leads later decisions to undo earlier ones. Every ADR template in common use, including MADR, descends from this post.

### ADR explainers (secondary)

- **Nolo Mokgosi, "Basics of Architecture Decision Records (ADR)"** (Medium, n.d.): <https://medium.com/@nolomokgosi/basics-of-architecture-decision-records-adr-e09e00c636c6>
- **"Lightweight Architecture Documentation with ADRs"** (production-ready.de, Dec 28, 2023): <https://www.production-ready.de/2023/12/28/lightweight-architecture-documentation-adr-en.html>
- **University of Queensland, CSSE6400 "Architectural Decision Records" lecture handout** (n.d.): <https://csse6400.uqcloud.net/handouts/adr.pdf>

**Type:** Tutorials / course notes · **Label:** Established (secondary) · **Used in:** §1, §4

These were used only to support general ADR claims: lightweight format, numbering and supersession. They add no evidence beyond Nygard and can be dropped if you want a primary-only source list.

---

## Part 2: Frameworks and architecture modelling

### C4 model: FAQ
<https://c4model.com/diagrams/faq>

**Type:** Framework documentation (Simon Brown; living) · **Label:** Established · **Used in:** §4, §7, §10

This page supports three points in the report:

- Component-level and code-level diagrams change often or go stale quickly.
- Context views can be built from existing service catalogs.
- Container views can be derived from logs or OpenTelemetry data.

That last point is the basis of the report's recommendation to *detect* drift from traces rather than replace declared intent with them.

### C4 model: Component diagram
<https://c4model.com/diagrams/component>

**Type:** Framework documentation · **Label:** Established · **Used in:** §7

C4 itself does not recommend component diagrams by default. It says to create them only when they add value and to consider generating them. The report's position of "C4 L3 only where it pays, L4 never by hand" rests on this.

### arc42 FAQ (Gernot Starke et al., Leanpub)
<https://leanpub.com/arc42-faq/read>

**Type:** Framework FAQ (living) · **Label:** Established · **Used in:** §7

This source advises investing in crosscutting concepts (arc42 §8) rather than detailed building blocks (§5) or runtime scenarios (§6). That advice is the basis for the report's "use arc42 as a checklist, not a 12-section mandate" recommendation.

### Diátaxis (Daniele Procida)
<https://diataxis.fr/>

**Type:** Documentation framework (living) · **Label:** Established · **Used in:** §7, §11 (Example 4)

Diátaxis sorts documentation into four modes: tutorials, how-to guides, reference and explanation. The report uses it mainly to decide what to *exclude*. An architecture.md is explanation plus a reference-style codemap, so tutorials, how-tos and runbooks belong elsewhere.

### Canonical: "Diátaxis, a new foundation for Canonical documentation" (n.d.)
<https://ubuntu.com/blog/diataxis-a-new-foundation-for-canonical-documentation>

**Type:** Engineering blog (adoption case) · **Label:** Established · **Used in:** §7

This is evidence that Diátaxis has been adopted at organisational scale (Canonical/Ubuntu), not just proposed.

### Structurizr: Usage recommendations
<https://structurizr.com/help/usage-recommendations>

**Type:** Tool documentation (living) · **Label:** Established (tooling) · **Used in:** §10

This page recommends one workspace per software system plus a landscape-scoped workspace. It is the basis of the report's "one model, many views" pattern for multi-repo estates.

### Structurizr: Enterprise usage / Scope / enterprise workspace example
- Enterprise usage: <https://docs.structurizr.com/workspaces/enterprise>
- Scope: <https://docs.structurizr.com/workspaces/scope>
- Enterprise workspace example: <https://github.com/structurizr/structurizr.github.io/blob/main/workspaces/04-enterprise.md>

**Type:** Tool documentation · **Label:** Established (tooling) · **Used in:** §1, §10

These pages describe the enterprise pattern. People and systems are defined once in a shared system catalog. Each team's workspace extends that catalog, and a central landscape view is generated from the team workspaces. The pattern is well documented. Evidence on what it costs and returns at hundreds of teams is anecdotal (see report §15).

### edirand: `structurizr-enterprise-example` (GitHub)
<https://github.com/edirand/structurizr-enterprise-example>

**Type:** Community example repo · **Label:** Emerging · **Used in:** §10

This is a worked implementation of the Structurizr enterprise pattern and is useful as a starter. It is a single community example, not a reference architecture.

### Glen Thomas: "Mapping Complex Distributed Systems with C4 Diagrams and Structurizr DSL" (Aug 27, 2025)
<https://blog.glen-thomas.com/architecture/2025/08/27/mapping-complex-distributed-systems-with-c4-diagrams-and-structurizr-dsl.html>

**Type:** Practitioner blog · **Label:** Emerging · **Used in:** §1, §10

This is a recent practitioner account of modelling a distributed system in Structurizr DSL. It supports the claim that the pattern is practical. It is one author's experience.

---

## Part 3: Docs-as-code and developer portals

### Backstage: TechDocs documentation
<https://backstage.io/docs/features/techdocs/>

**Type:** Tool documentation (living) · **Label:** Established · **Used in:** §6, §12

This is the source for Spotify's internal TechDocs scale figure (5,000+ documentation sites, around 10,000 average daily hits). It is also the reference for the report's default publishing layer: docs live in repos and are rendered centrally. The figure is self-reported by Spotify/Backstage.

### Backstage blog: "Announcing TechDocs: Spotify's docs-like-code plugin for Backstage" (Sep 8, 2020)
<https://backstage.io/blog/2020/09/08/announcing-tech-docs/>

**Type:** Engineering blog · **Label:** Established · **Used in:** §6

This explains Spotify's reasoning for docs-like-code and the problems it was solving. The report uses it as the counterweight: Spotify found some content types, such as long-form Golden Path tutorials, harder to manage as docs-as-code. That is why the report presents a hybrid model rather than "everything in Git".

### Docs Like Code: "Ten tips for maintaining a long-term relationship with docs like code" (n.d.)
<https://www.docslikecode.com/articles/ten-tips-maintaining-long-term-docs-like-code/>

**Type:** Practitioner article · **Label:** Established · **Used in:** §6

This is cited for the Spotify lesson that being opinionated pays off: it declared one standard way to produce documentation instead of allowing many. It supports the report's recommendation that an enterprise pick one publishing path.

### bowenlabs/louise-toolkit, Issue #430: "Docs: freshness dates and owners on every published page"
<https://github.com/bowenlabs/louise-toolkit/issues/430>

**Type:** GitHub issue in a single OSS project · **Label:** Weak · **Used in:** §12

This is an example of the freshness-and-owner practice being proposed in the wild. It is anecdotal. The authoritative source for the practice is the *Software Engineering at Google* chapter in Part 1. Consider dropping it.

---

## Part 4: Documentation and organisational performance (DORA)

### DORA: "Documentation quality" capability
<https://dora.dev/capabilities/documentation-quality/>

**Type:** Research summary (living) · **Label:** Established (correlational) · **Used in:** §1, §5

This states DORA's link between documentation *quality* and organisational performance, and lists the quality attributes DORA measures. It underpins the report's "measure quality and usage, not existence" stance. The findings come from surveys and are correlational, not causal.

### Google Cloud blog: "2023 State of DevOps Report: Culture is everything" (Oct 2023)
<https://cloud.google.com/blog/products/devops-sre/announcing-the-2023-state-of-devops-report>
Full report: <https://dora.dev/dora-report-2023>

**Type:** Research announcement · **Label:** Established (correlational) · **Used in:** §5

This is the source of two figures. SRE practices are estimated to have 1.4× more impact on organisational performance when high-quality documentation is in place. High-quality documentation is associated with 25% higher team performance than low-quality documentation. Cite the full report for exact methodology.

### DORA: "Accelerate State of DevOps 2021" (Sep 2021)
PDF: <https://dora.dev/publications/pdf/state-of-devops-2021.pdf>
Announcement: <https://cloud.google.com/blog/products/devops-sre/announcing-dora-2021-accelerate-state-of-devops-report>

**Type:** Research report · **Label:** Established (correlational) · **Used in:** §5

This is the first year DORA measured internal documentation quality, and the year it called good documentation "foundational" for implementing DevOps capabilities. The documentation chapter starts around p. 21 of the PDF.

### Octopus Deploy: "Understanding the 4 DORA Metrics and Top Findings from 2024/25 DORA Report" (n.d.)
<https://octopus.com/devops/metrics/dora-metrics/>

**Type:** Vendor explainer · **Label:** Weak (secondary) · **Used in:** §12

This is a secondary summary of DORA findings, used as background for the metrics section. Prefer dora.dev primary sources in any formal version.

---

## Part 5: Software supply chain (Worked Example 1, CI/CD platform)

### SLSA v1.0: Security levels
<https://slsa.dev/spec/v1.0/levels>

**Type:** Industry specification (OpenSSF) · **Label:** Established · **Used in:** §11 (Example 1)

This is the source of the Build L1, L2 and L3 definitions used in the GOOD CI/CD architecture doc:

- **L1:** provenance exists.
- **L2:** signed provenance comes from a hosted build platform.
- **L3:** the build platform is hardened, with isolation between runs and signing secrets kept away from user-defined steps.

The report's "Zone U vs Zone T" trust-boundary diagram is a direct application of L3. Check whether a newer SLSA version has superseded v1.0 before you write it into policy.

### Sigstore documentation
<https://docs.sigstore.dev/>

**Type:** Project documentation (OpenSSF; living) · **Label:** Established · **Used in:** §11 (Example 1)

This covers keyless signing, which associates signatures with identities rather than long-lived keys, using short-lived Fulcio certificates and the Rekor transparency log. It backs the invariant "user-defined build steps never see signing material."

### NTIA: "The Minimum Elements for a Software Bill of Materials (SBOM)" (Jul 2021)
<https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom>

**Type:** U.S. government guidance · **Label:** Established (now superseded) · **Used in:** §11 (Example 1)

This is the original seven-field SBOM baseline. **The 2026 CISA guidance below replaces it.** Keep it only for historical context and for mapping older tooling.

### CISA et al.: "2026 Minimum Elements for a Software Bill of Materials (SBOM)" (Jul 29, 2026)
- Guidance PDF (hosted on IC3): <https://www.ic3.gov/CSA/2026/260729.pdf>
- Partner publication page (NCSC New Zealand, Jul 31, 2026): <https://www.ncsc.govt.nz/protect-your-organisation/2026-minimum-elements-for-a-software-bill-of-materials-sbom-published/>
- Press-release coverage (HSToday, republishing CISA's statement): <https://www.hstoday.us/subject-matter-areas/cybersecurity/cisa-updates-software-bill-of-materials-guidance-to-strengthen-supply-chain-security/>
- Analysis (FOSSA, Jul 30, 2026): <https://fossa.com/blog/cisa-releases-2026-minimum-sbom-elements/>
- Analysis (Paubox): <https://www.paubox.com/blog/cisa-updates-sbom-guidance-with-ten-new-data-fields>
- Analysis (TechInformed): <https://techinformed.com/sbom-guidance-adds-detail-but-no-test-for-missing-components/>

**Type:** U.S. and international joint government guidance (CISA, NSA, FBI and partners) · **Label:** Established (new) · **Used in:** §11 (Example 1), §15

This is the current SBOM baseline. It applies to all software, including open source, AI software and SaaS. It adds new required elements, including:

- Component Hash Algorithm
- Component License
- SBOM Tool Name
- SBOM Generation Context

It also adds a digital-signature field and renames several existing elements. Trade-press coverage describes the field count as going from 7 to 17. Take the exact element names from Appendix A of the PDF, not from news coverage, when you write policy gates. AI-specific fields are deferred to separate CISA/G7 AI SBOM guidance from May 2026.

---

## Part 6: AI agent-context files (standards and vendor guidance)

### AGENTS.md (project site)
<https://agents.md/>

**Type:** Open convention (living) · **Label:** Vendor/Emerging · **Used in:** §13

This is the canonical description of AGENTS.md as a "README for agents" with no required fields. The site claims adoption by more than 60k open-source projects; that number is self-reported.

### Linux Foundation: "Linux Foundation Announces the Formation of the Agentic AI Foundation (AAIF)…" (Dec 9, 2025)
<https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation>

**Type:** Press release · **Label:** Established (governance fact) · **Used in:** §13

This confirms that AGENTS.md (released by OpenAI in Aug 2025) was contributed to the new Agentic AI Foundation alongside Anthropic's MCP and Block's goose. The report uses it to show that AGENTS.md now has neutral stewardship, which reduces vendor lock-in risk for an enterprise standard.

### OpenAI: "OpenAI co-founds the Agentic AI Foundation under the Linux Foundation" (Dec 9, 2025)
<https://openai.com/index/agentic-ai-foundation/>

**Type:** Vendor announcement · **Label:** Vendor · **Used in:** §13

This is OpenAI's side of the same announcement. It gives the 60,000+ adoption figure and lists agents that read AGENTS.md (Amp, Codex, Cursor, Devin, Factory, Gemini CLI, GitHub Copilot, Jules, VS Code). The adoption number is vendor-reported.

### Anthropic: Claude Code "Best practices"
<https://code.claude.com/docs/en/best-practices>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** §4, §13

This advises keeping CLAUDE.md concise, because bloated files cause instructions to be ignored. It says to include commands Claude can't guess and to exclude anything Claude can work out by reading the code. That guidance is the basis of the report's "router, not encyclopedia" rule.

### Anthropic: Claude Code memory ("How Claude remembers your project")
<https://code.claude.com/docs/en/memory>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** §13

This source sets two points:

- The size target is under 200 lines per CLAUDE.md, because longer files use more context and reduce adherence.
- The docs cover CLAUDE.md scopes and recursive discovery up and down the directory tree.

**It also states that Claude Code reads CLAUDE.md, not AGENTS.md.** This matters for the report's "AGENTS.md canonical + tool shims" recommendation (§13.1): for Claude Code, the shim (an import or symlink) is *required*, not optional.

### GitHub Docs: "Adding custom instructions for GitHub Copilot CLI"
<https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** §11 (Example 5), §13

This documents Copilot's instruction files: repository-wide `.github/copilot-instructions.md`, path-scoped `.github/instructions/*.instructions.md` with `applyTo` globs, and support for AGENTS.md and CLAUDE.md. It is the basis for the report's nested and path-scoped instructions advice for monorepos.

### GitHub Changelog: "Copilot code review and coding agent now support agent-specific instructions" (Nov 12, 2025)
<https://github.blog/changelog/2025-11-12-copilot-code-review-and-coding-agent-now-support-agent-specific-instructions/>

**Type:** Vendor changelog · **Label:** Vendor · **Used in:** §13

This introduced `excludeAgent`, which lets one instruction file target the coding agent but not code review, or the reverse. It is relevant when the same repo serves several AI consumers.

### Skillwright: "GitHub Copilot Instructions: The Complete Guide" (n.d.)
<https://www.skillwright.app/blog/copilot-instructions-guide>

**Type:** Third-party guide · **Label:** Weak (secondary) · **Used in:** §13

This is a secondary walkthrough of Copilot instruction mechanics. Use the GitHub Docs page above as the authority.

### OpenAI: "Harness engineering: leveraging Codex in an agent-first world" (Feb 11, 2026)
<https://openai.com/index/harness-engineering/>

**Type:** Vendor engineering case study · **Label:** Vendor (case study) · **Used in:** §13

This is the most detailed public account of progressive disclosure in practice. OpenAI's team tried one big AGENTS.md, abandoned it, and moved to a short AGENTS.md of about 100 lines that serves as a table of contents. That file points into a structured `docs/` tree that includes ARCHITECTURE.md and is backed by mechanical freshness checks and a recurring doc-gardening agent.

**Caveat worth adding to the report:** the post says the team's *initial* AGENTS.md was written by Codex. That sits awkwardly with the report's "agent-context files are human-authored" rule. The post doesn't show whether later versions were human-curated. It is a single internal team with privileged tool access, so its results may not generalise.

---

## Part 7: AI agent-context files (empirical research)

### Gloaguen, Mündler, Müller, Raychev & Vechev: "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv Feb 2026; ICLR 2026)
- arXiv: <https://arxiv.org/abs/2602.11988> (PDF: <https://arxiv.org/pdf/2602.11988>)
- ICLR 2026 page: <https://iclr.cc/virtual/2026/10021142>

**Type:** Peer-reviewed controlled study (ETH Zurich / LogicStar.ai) · **Label:** Evidence (strongest available) · **Used in:** §1, §3, §13

This is the anchor study for the report's AI stance. Across SWE-bench Lite and a new benchmark of repos with developer-written context files:

- Context files did not generally improve task success.
- They raised inference cost by more than 20%.
- Repository overviews didn't help agents find relevant files faster.
- LLM-generated files did slightly worse than having none.
- Agents did follow the instructions they were given.

The authors conclude that human-written files should state only minimal requirements. **Version note:** an earlier version reported about +4% on average for developer-written files. The ICLR abstract summarises the overall result as no improvement. Cite the version you rely on.

### Lulla et al.: "On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents" (arXiv Jan 2026; v2 Mar 30, 2026)
<https://arxiv.org/abs/2601.20404>

**Type:** Empirical study (workshop-scale) · **Label:** Evidence (small) · **Used in:** §13

This study covered 10 repos and 124 PRs with one agent (Codex). The presence of AGENTS.md was associated with 28.64% lower median runtime and 16.58% fewer output tokens, with comparable task completion. It is the main evidence for an *efficiency* benefit. It is associational and single-agent.

### Khatri: "Do Context Files Help Coding Agents? A Two-Agent Ablation Study on Real Repositories" (arXiv Jul 28, 2026)
<https://arxiv.org/abs/2607.27250>
Hugging Face summary: <https://huggingface.co/papers/2607.27250>

**Type:** Controlled ablation (independent researcher; preprint) · **Label:** Evidence (small) · **Used in:** §13

This study ran Claude Code and Codex on 17 tasks from 3 repos, with 288 runs comparing no context, always-on context and selective context. It found no measurable effect on correctness, bounded to under 10–15 percentage points. Failures came from implementation skill rather than missing repository knowledge. The author also suggests that earlier contradictory results came from single-agent task selection. The sample is small and the work is not yet peer-reviewed.

### Chatlatanagulchai et al.: "Agent READMEs: An Empirical Study of Context Files for Agentic Coding" (arXiv Nov 2025)
<https://arxiv.org/abs/2511.12884>

**Type:** Large descriptive study · **Label:** Evidence (descriptive) · **Used in:** §13

This study examined 2,303 context files from 1,925 repos. The files tend to be long and hard to read, they are maintained like configuration, and security and performance instructions are rare (about 15% each). It supports the report's point that non-functional constraints are the real gap. It describes what files contain, not what effect they have.

### "Configuration Smells in AGENTS.md Files: Common Mistakes in Configuring Coding Agents" (arXiv Jun 2026)
<https://arxiv.org/abs/2606.15828> (HTML: <https://arxiv.org/html/2606.15828v2>)

**Type:** Descriptive study (UFMG) · **Label:** Evidence (descriptive) · **Used in:** §1, §13

In 100 popular repos, 91 had at least one smell. The smells and their rates were:

- lint leakage: 62%
- context bloat: 42%
- skill leakage: 35%
- conflicting instructions: 28%
- init fossilization: 24%
- blind references: 16%

This taxonomy is the basis for the report's "agent-context lint" CI check. It is a sample of popular OSS repos, so enterprise rates may differ.

**Press coverage of the smells paper** (secondary; useful for executive audiences):
- The Register (Jun 17, 2026): <https://www.theregister.com/ai-and-ml/2026/06/17/if-agentsmd-smells-ripe-your-code-wont-live-up-to-the-hype/5257951>
- InfoWorld (2026): <https://www.infoworld.com/article/4187057/ai-coding-agents-may-be-getting-bad-instructions-from-smelly-config-files.html>
- daily.dev (aggregates the InfoWorld piece): <https://daily.dev/posts/ai-coding-agents-may-be-getting-bad-instructions-from-smelly-config-files-ysuwpbrtf>
- Digital Applied (2026; practitioner take with fixes): <https://www.digitalapplied.com/blog/ai-agent-config-smells-claude-md-best-practices-2026>

---

## Part 8: AI-assisted doc/code drift detection

### DocPrism: "Local Categorization and External Filtering to Identify Relevant Code-Documentation Inconsistencies" (arXiv Nov 2025)
<https://arxiv.org/abs/2511.00215>

**Type:** Research prototype · **Label:** Emerging · **Used in:** §13.7

Adding a filtering step cut the inconsistency flag rate from 98% to 14% and raised accuracy from 14% to 94%. This is the main evidence that naive LLM drift detection drowns owners in false positives. It targets function-level and API-level docs, not architecture prose.

### CASCADE: "Detecting Inconsistencies between Code and Documentation with Automatic Test Generation" (arXiv Apr 2026; ACM)
- arXiv: <https://arxiv.org/abs/2604.19400>
- ACM DOI: <https://doi.org/10.1145/3808175>

**Type:** Peer-reviewed research · **Label:** Emerging · **Used in:** §13.7

CASCADE generates tests from documentation. It reports an inconsistency only when existing code fails a doc-derived test that independently generated code passes. That design makes it conservative by construction. Like DocPrism, it covers API-level docs. No equivalent benchmark exists for architecture documents (report §15).

---

## Part 9: Tools and standards named in the report (homepages; not evidence)

These are named in the templates, CI checks and worked examples. They are linked for convenience and were not re-checked while compiling this list.

| Tool / standard | Link | Where named |
|---|---|---|
| C4 model (home) | <https://c4model.com/> | §7, §10, §14 |
| arc42 (home) | <https://arc42.org/> | §7 |
| ADR community site / MADR | <https://adr.github.io/> · <https://adr.github.io/madr/> | §7, §12 |
| Structurizr DSL | <https://docs.structurizr.com/dsl> | §8, §10, §14 |
| Mermaid | <https://mermaid.js.org/> | §8, §11, §14 |
| PlantUML | <https://plantuml.com/> | §8 |
| Backstage catalog descriptor format | <https://backstage.io/docs/features/software-catalog/descriptor-format/> | §3, §10, §12 |
| MkDocs | <https://www.mkdocs.org/> | §6 |
| Docusaurus | <https://docusaurus.io/> | §6 |
| GitHub CODEOWNERS | <https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners> | §12 |
| Vale (prose linter) | <https://vale.sh/> | §12 |
| markdownlint | <https://github.com/DavidAnson/markdownlint> | §12 |
| Buf (protobuf breaking-change checks) | <https://buf.build/docs/> | §11, §12 |
| Bazel visibility | <https://bazel.build/concepts/visibility> | §11 (Example 5) |
| Nx module boundaries | <https://nx.dev/features/enforce-module-boundaries> | §11 (Example 5) |
| OpenTelemetry | <https://opentelemetry.io/> | §10, §12 |
| SPDX / CycloneDX (SBOM formats) | <https://spdx.dev/> · <https://cyclonedx.org/> | §11 (Example 1) |

---

## Part 10: Named in the research brief but not used as evidence

The brief asked for these books to be prioritised. The report did not draw on them directly; its framework claims rest on the free primary sources above. If you need book-level backing for the topology and trade-off sections, these are the gaps to fill:

- **Clements, Bachmann, Bass, Garlan, Ivers, Little, Merson, Nord & Stafford, *Documenting Software Architectures: Views and Beyond*, 2nd ed. (Addison-Wesley, 2010).** This is the SEI's views-based approach and the academic grounding for "document the views your stakeholders need."
- **Ford, Richards, Sadalage & Dehghani, *Software Architecture: The Hard Parts* (O'Reilly, 2021).** It covers trade-off analysis for distributed systems: service granularity, shared code and data ownership. It is directly relevant to Example 2 (shared libraries, no shared DB).
- **Bhatti, Corleissen, Lambourne, Nunez & Waterhouse, *Docs for Developers: An Engineer's Field Guide to Technical Writing* (Apress, 2021).** It covers developer-documentation craft, maintenance and measurement, and is relevant to §8 and §12.

*(Book links omitted. Use your library or the publisher's catalogue.)*

---

## Part 11: Corrections and caveats found while compiling this list

1. **Claude Code does not read AGENTS.md natively.** Its memory docs say it reads CLAUDE.md. In report §13.1, change "keep tool-specific files as thin shims or symlinks" to make the CLAUDE.md shim (import or symlink) **mandatory** wherever Claude Code is used.
2. **OpenAI's Harness case study started from a Codex-written AGENTS.md.** Report §13 presents that case as support for human-authored, short routers. The "short router" lesson holds; the "human-authored" lesson is not demonstrated by that source. The human-authorship recommendation rests on Gloaguen et al.
3. **Gloaguen et al. versions differ.** The ICLR abstract says "no improvement"; an earlier arXiv version reported about +4% for developer-written files. Cite a specific version.
4. **CISA 2026 SBOM:** coverage reports the field count going from 7 to 17 and a new digital-signature field. Use Appendix A of the guidance PDF as the authority for exact element names in policy-as-code.
5. **Two sources are weak and replaceable:** the Goodreads quote page (replace with abseil.io) and the louise-toolkit GitHub issue (anecdotal).
6. **Secondary explainers** (Medium ADR post, Octopus, Skillwright, daily.dev) can be dropped without losing any claim. Each is covered by a primary source above.
