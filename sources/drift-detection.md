[Overview](../README.md) › [Sources](README.md)

# AI-assisted doc/code drift detection (Part 8)

## DocPrism: "Local Categorization and External Filtering to Identify Relevant Code-Documentation Inconsistencies" (arXiv Nov 2025)
<https://arxiv.org/abs/2511.00215>

**Type:** Research prototype · **Label:** Emerging · **Used in:** [AI-generated docs and drift detection](../ai-agents/generated-docs-and-drift.md)

Adding a filtering step cut the inconsistency flag rate from 98% to 14% and raised accuracy from 14% to 94%. This is the main evidence that naive LLM drift detection drowns owners in false positives. It targets function-level and API-level docs, not architecture prose.

## CASCADE: "Detecting Inconsistencies between Code and Documentation with Automatic Test Generation" (arXiv Apr 2026; ACM)
- arXiv: <https://arxiv.org/abs/2604.19400>
- ACM DOI: <https://doi.org/10.1145/3808175>

**Type:** Peer-reviewed research · **Label:** Emerging · **Used in:** [AI-generated docs and drift detection](../ai-agents/generated-docs-and-drift.md)

CASCADE generates tests from documentation. It reports an inconsistency only when existing code fails a doc-derived test that independently generated code passes. That design makes it conservative by construction. Like DocPrism, it covers API-level docs. No equivalent benchmark exists for architecture documents (report [Open questions](../topics/open-questions.md)).
