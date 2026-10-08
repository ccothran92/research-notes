[Overview](../README.md) › [Sources](README.md)

# AI agent-context files: empirical research (Part 7)

## Gloaguen, Mündler, Müller, Raychev & Vechev: "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv Feb 2026; ICLR 2026)
- arXiv: <https://arxiv.org/abs/2602.11988> (PDF: <https://arxiv.org/pdf/2602.11988>)
- ICLR 2026 page: <https://iclr.cc/virtual/2026/10021142>

**Type:** Peer-reviewed controlled study (ETH Zurich / LogicStar.ai) · **Label:** Evidence (strongest available) · **Used in:** [Executive summary](../executive-summary.md), [Decision framework](../topics/decision-framework.md), [AI agents](../ai-agents/README.md)

This is the anchor study for the report's AI stance. Across SWE-bench Lite and a new benchmark of repos with developer-written context files:

- Context files did not generally improve task success.
- They raised inference cost by more than 20%.
- Repository overviews didn't help agents find relevant files faster.
- LLM-generated files did slightly worse than having none.
- Agents did follow the instructions they were given.

The authors conclude that human-written files should state only minimal requirements. **Version note:** an earlier version reported about +4% on average for developer-written files. The ICLR abstract summarises the overall result as no improvement. Cite the version you rely on.

## Lulla et al.: "On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents" (arXiv Jan 2026; v2 Mar 30, 2026)
<https://arxiv.org/abs/2601.20404>

**Type:** Empirical study (workshop-scale) · **Label:** Evidence (small) · **Used in:** [AI agents](../ai-agents/README.md)

This study covered 10 repos and 124 PRs with one agent (Codex). The presence of AGENTS.md was associated with 28.64% lower median runtime and 16.58% fewer output tokens, with comparable task completion. It is the main evidence for an *efficiency* benefit. It is associational and single-agent.

## Khatri: "Do Context Files Help Coding Agents? A Two-Agent Ablation Study on Real Repositories" (arXiv Jul 28, 2026)
<https://arxiv.org/abs/2607.27250>
Hugging Face summary: <https://huggingface.co/papers/2607.27250>

**Type:** Controlled ablation (independent researcher; preprint) · **Label:** Evidence (small) · **Used in:** [AI agents](../ai-agents/README.md)

This study ran Claude Code and Codex on 17 tasks from 3 repos, with 288 runs comparing no context, always-on context and selective context. It found no measurable effect on correctness, bounded to under 10–15 percentage points. Failures came from implementation skill rather than missing repository knowledge. The author also suggests that earlier contradictory results came from single-agent task selection. The sample is small and the work is not yet peer-reviewed.

## Chatlatanagulchai et al.: "Agent READMEs: An Empirical Study of Context Files for Agentic Coding" (arXiv Nov 2025)
<https://arxiv.org/abs/2511.12884>

**Type:** Large descriptive study · **Label:** Evidence (descriptive) · **Used in:** [AI agents](../ai-agents/README.md)

This study examined 2,303 context files from 1,925 repos. The files tend to be long and hard to read, they are maintained like configuration, and security and performance instructions are rare (about 15% each). It supports the report's point that non-functional constraints are the real gap. It describes what files contain, not what effect they have.

## "Configuration Smells in AGENTS.md Files: Common Mistakes in Configuring Coding Agents" (arXiv Jun 2026)
<https://arxiv.org/abs/2606.15828> (HTML: <https://arxiv.org/html/2606.15828v2>)

**Type:** Descriptive study (UFMG) · **Label:** Evidence (descriptive) · **Used in:** [Executive summary](../executive-summary.md), [AI agents](../ai-agents/README.md)

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
