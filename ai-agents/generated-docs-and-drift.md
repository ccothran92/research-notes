[Overview](../README.md) › [AI agents](README.md)

# AI-generated docs and drift detection

*Labels: [Evidence] = controlled or empirical study; [Vendor] = tool-maker guidance; [Emerging] = practitioner reports; [Synthesis] = this report's recommendation. Dates are given because this area moves quickly.*

## Risks of AI-generated architecture docs

- **Confident drift.** LLMs describe what they see, not intent. They cannot recover invariants expressed as absences, and they fill gaps plausibly. ETH recommends "omitting LLM-generated context files for the time being."
- **Init fossilization.** `/init`-generated files capture day-one structure and rot (24% in the UFMG sample).
- **Authority laundering.** Polished output earns unearned trust.
- **Policy [Synthesis]:** AI may draft architecture docs and ADRs. A named human verifies every structural claim and invariant before merge, and AI-drafted PRs are labeled. Agent-context files are human-authored.

## Using AI to detect doc/code drift

Research shows promise and a false-positive problem:

- **DocPrism:** filtering cut its inconsistency flag rate from 98% to 14% and raised accuracy from 14% to 94%.
- **CASCADE (FSE):** generates tests from docs and reports an inconsistency only when existing code fails while doc-derived code passes.

Both target API- and function-level docs, not architecture prose. **Practical pattern [Emerging]:** a scheduled doc-gardening job checks ARCHITECTURE.md claims deterministically first (named modules and directories exist; declared dependencies match the import graph, catalog and traces), then uses LLM judgment. It opens suggestion PRs routed to the owner and never auto-merges.

---

## Sources

- [DocPrism: "Local Categorization and External Filtering to Identify Relevant Code-Documentation Inconsistencies" (arXiv Nov 2025)](../sources/drift-detection.md#docprism-local-categorization-and-external-filtering-to-identify-relevant-code-documentation-inconsistencies-arxiv-nov-2025) (AI-assisted doc/code drift detection)
- [CASCADE: "Detecting Inconsistencies between Code and Documentation with Automatic Test Generation" (arXiv Apr 2026; ACM)](../sources/drift-detection.md#cascade-detecting-inconsistencies-between-code-and-documentation-with-automatic-test-generation-arxiv-apr-2026-acm) (AI-assisted doc/code drift detection)
