[Overview](../README.md) › [Sources](README.md)

# Architecture documentation practice (Part 1)

## matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)
<https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html>

**Type:** Practitioner essay · **Label:** Established · **Used in:** [Executive summary](../executive-summary.md), [When to write](../topics/when-to-write.md), [Level of detail](../topics/level-of-detail.md), [Single-repo topology](../topics/single-repo.md), [Templates](../templates/README.md)

This is the origin of the ARCHITECTURE.md convention and the report's main model for the repo-level doc. Its core claims are:

- Keep the doc short so it rarely goes out of date.
- Give a codemap, a map of the country rather than an atlas.
- Name modules but don't hyperlink files, because links rot.
- Make invariants explicit, especially those expressed as the absence of something.
- Revisit the doc a couple of times a year instead of syncing it with every change.

The essay targets projects of roughly 10k–200k LOC. It is opinion from one experienced maintainer, not research. It has been very widely adopted, though, and it fits with Google's and OpenAI's guidance.

## rust-analyzer: `docs/dev/architecture.md` (pinned commit)
<https://github.com/rust-lang/rust-analyzer/blob/067b4a32dd267d2599f908fc556491be45af7176/docs/dev/architecture.md>
Repo: <https://github.com/rust-lang/rust-analyzer>

**Type:** Real-world exemplar · **Label:** Established (example) · **Used in:** [Single-repo topology](../topics/single-repo.md)

This is matklad's own reference implementation, and the report critiques it as a GOOD example with caveats. It has a strong codemap and labelled "Architecture Invariant" and "API Boundary" callouts. At this commit it runs to about 518 lines, it contains a TBA stub, it has no ADRs and it has little security content. The pinned URL is deliberate: the file has since moved within the repo, which shows that links into docs rot too.

## rust-analyzer: `CLAUDE.md`
<https://github.com/rust-lang/rust-analyzer/blob/master/CLAUDE.md>

**Type:** Real-world exemplar · **Label:** Emerging · **Used in:** [Single-repo topology](../topics/single-repo.md)

This is cited next to the architecture.md as a live example of the agent-context file of a project that already has a strong architecture doc. Its contents were not analysed in depth for the report. Read the current version before you use it as a model.

## Backstage: "Architecture overview"
<https://backstage.io/docs/overview/architecture-overview/>

**Type:** Real-world exemplar (OSS project docs) · **Label:** Established (example) · **Used in:** [Single-repo topology](../topics/single-repo.md)

This is the contrast example to rust-analyzer. It is a well-diagrammed tour of the frontend, backend, plugins and databases, and it keeps the threat model on a separate, linked page, which the report endorses. Its weaknesses are that it mixes concept material with how-to material and uses time-relative wording ("at this time of writing"), which the report flags as a drift smell.

## Malte Ubl: "Design Docs at Google" (2020)
<https://www.industrialempathy.com/posts/design-docs-at-google/>

**Type:** Practitioner essay (ex-Google engineer) · **Label:** Established · **Used in:** [Executive summary](../executive-summary.md), [When to write](../topics/when-to-write.md), [Where docs live](../topics/where-docs-live.md), [Level of detail](../topics/level-of-detail.md), [Maintenance and review](../topics/maintenance-and-review.md)

This is the main source for when to write a design doc and when not to. Skip the doc when it would be only an "implementation manual" with no trade-offs, and recognise that design-doc overhead may not suit rapid prototyping. It also supplies the standard section set (context, goals and non-goals, design, alternatives, cross-cutting concerns), the warning against pasting API or schema definitions that go stale, the warning that design reviews can turn into overhead, and the observation that Google writes most design docs in Google Docs. It describes one company's culture, so calibrate it to your own.

## Software Engineering at Google, Ch. 10 "Documentation" (Winters, Manshreck & Wright, O'Reilly, 2020)
<https://abseil.io/resources/swe-book/html/ch10.html>
Community mirror: <https://github.com/xgqfrms/Software-Engineering-At-Google/blob/main/docs/ch10-documentation.md>

**Type:** Book chapter (free HTML from Google/Abseil) · **Label:** Established · **Used in:** [Executive summary](../executive-summary.md), [Where docs live](../topics/where-docs-live.md), [Maintenance and review](../topics/maintenance-and-review.md), [Maintenance and review](../topics/maintenance-and-review.md)

This is the source for g3doc: docs live beside source, which establishes de facto ownership and lets one change update both code and docs. It also covers freshness metadata (owner and last-reviewed date) that triggers reminders. Cite the abseil.io version; the GitHub mirror is unofficial.

## Goodreads: quote page for the same chapter (freshness dates)
<https://www.goodreads.com/quotes/11341657-at-google-we-often-attach-freshness-dates-to-documentation-such>

**Type:** Quote aggregator · **Label:** Weak (secondary) · **Used in:** [Executive summary](../executive-summary.md), [Maintenance and review](../topics/maintenance-and-review.md)

This was picked up during research as a secondary copy of the Google freshness-dates passage. **Replace it with the abseil.io chapter above** in any formal version of the report.

## Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)
<https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions>

**Type:** Practitioner essay; origin of the ADR format · **Label:** Established · **Used in:** [Executive summary](../executive-summary.md), [When to write](../topics/when-to-write.md), [Level of detail](../topics/level-of-detail.md), [Maintenance and review](../topics/maintenance-and-review.md)

This is the canonical ADR source. It defines the Title / Status / Context / Decision / Consequences structure. ADRs are numbered, short and superseded rather than edited, and numbers are never reused. Lost rationale leads later decisions to undo earlier ones. Every ADR template in common use, including MADR, descends from this post.

## ADR explainers (secondary)

- **Nolo Mokgosi, "Basics of Architecture Decision Records (ADR)"** (Medium, n.d.): <https://medium.com/@nolomokgosi/basics-of-architecture-decision-records-adr-e09e00c636c6>
- **"Lightweight Architecture Documentation with ADRs"** (production-ready.de, Dec 28, 2023): <https://www.production-ready.de/2023/12/28/lightweight-architecture-documentation-adr-en.html>
- **University of Queensland, CSSE6400 "Architectural Decision Records" lecture handout** (n.d.): <https://csse6400.uqcloud.net/handouts/adr.pdf>

**Type:** Tutorials / course notes · **Label:** Established (secondary) · **Used in:** [Executive summary](../executive-summary.md), [When to write](../topics/when-to-write.md)

These were used only to support general ADR claims: lightweight format, numbering and supersession. They add no evidence beyond Nygard and can be dropped if you want a primary-only source list.
