[Overview](../README.md) › [AI agents](README.md)

# What the evidence says (mixed; read carefully)

*Labels: [Evidence] = controlled or empirical study; [Vendor] = tool-maker guidance; [Emerging] = practitioner reports; [Synthesis] = this report's recommendation. Dates are given because this area moves quickly.*

| Study (date) | Design | Finding | Implication |
|---|---|---|---|
| Gloaguen et al., ETH Zurich/LogicStar, arXiv 2602.11988 (Feb 2026; revised through Sept 2026) [Evidence] | SWE-bench Lite + CTXBENCH (138 tasks, 12 repos with developer-committed files); several agents/LLMs | Context files "do not generally improve task success rates, while increasing inference cost by over 20%"; repository overviews "are not helpful". In v1, developer-provided files improved performance over none "(an increase of 4% on average), while LLM-generated context files have a small negative effect". Instructions *are* followed | Don't auto-generate; no overviews; include only what's "not already present in the README (e.g., specific conventions or non-functional requirements)" |
| Lulla et al., arXiv 2601.20404 (Jan 2026; workshop paper) [Evidence, small] | 10 repos, 124 PRs, Codex CLI only | Associated with 28.64% lower median runtime, 16.58% fewer output tokens, "comparable" completion | Efficiency gain possible; one agent, associational |
| Khatri, arXiv 2607.27250 (Jul 2026) [Evidence, small] | Claude Code + Codex, 17 tasks, 288 runs; none / always-on / selective | No measurable correctness effect (bounded <10–15pp); failures were implementation skill, not missing repo knowledge | Context files don't fix capability gaps |
| Chatlatanagulchai et al., arXiv 2511.12884 (Nov 2025) [Evidence, descriptive] | 2,303 files, 1,925 repos | Long, hard to read, maintained like config; security (14.8%) and performance (14.5%) instructions rare | NFR constraints are the real gap |
| UFMG "Configuration Smells", arXiv 2606.15828 (Jun 2026) [Evidence, descriptive] | 100 popular repos | 91/100 had ≥1 smell: lint leakage 62%, context bloat 42%, skill leakage 35%, conflicting instructions 28%, init fossilization 24%, blind references 16% | Lint for these in CI |
| OpenAI "Harness engineering" (Feb 11, 2026) [Vendor case study] | Agent-built internal product | "One big AGENTS.md" failed; switched to ~100-line AGENTS.md as "table of contents" pointing to a structured `docs/` "system of record" incl. ARCHITECTURE.md, mechanical freshness/cross-link checks, recurring "doc-gardening" agent | Progressive disclosure + mechanical enforcement |

**Bottom line [Synthesis].** The best-supported claims are negative: long, overview-heavy or generated context files don't help and cost tokens. The positive effects (efficiency, adherence to non-standard conventions) are modest and come from small studies. No study yet shows that architecture.md *itself* improves agent outcomes. That benefit is plausible for structural tasks but unproven.

> **Caveats:** Gloaguen et al. versions differ (the ICLR abstract says "no improvement"; an earlier arXiv version reported about +4% for developer-written files), so cite a specific version. OpenAI's Harness case started from a Codex-written AGENTS.md, so it supports the "short router" lesson but not the "human-authored" one. See [corrections and caveats](../sources/README.md#corrections-and-caveats-found-while-compiling-the-sources), items 2–3.

---

## Sources

- [AGENTS.md (project site)](../sources/agent-context-guidance.md#agentsmd-project-site) (AI agent-context files: standards and vendor guidance)
- [Linux Foundation: "Linux Foundation Announces the Formation of the Agentic AI Foundation (AAIF)…" (Dec 9, 2025)](../sources/agent-context-guidance.md#linux-foundation-linux-foundation-announces-the-formation-of-the-agentic-ai-foundation-aaif-dec-9-2025) (AI agent-context files: standards and vendor guidance)
- [OpenAI: "OpenAI co-founds the Agentic AI Foundation under the Linux Foundation" (Dec 9, 2025)](../sources/agent-context-guidance.md#openai-openai-co-founds-the-agentic-ai-foundation-under-the-linux-foundation-dec-9-2025) (AI agent-context files: standards and vendor guidance)
- [Anthropic: Claude Code "Best practices"](../sources/agent-context-guidance.md#anthropic-claude-code-best-practices) (AI agent-context files: standards and vendor guidance)
- [Anthropic: Claude Code memory ("How Claude remembers your project")](../sources/agent-context-guidance.md#anthropic-claude-code-memory-how-claude-remembers-your-project) (AI agent-context files: standards and vendor guidance)
- [GitHub Docs: "Adding custom instructions for GitHub Copilot CLI"](../sources/agent-context-guidance.md#github-docs-adding-custom-instructions-for-github-copilot-cli) (AI agent-context files: standards and vendor guidance)
- [GitHub Changelog: "Copilot code review and coding agent now support agent-specific instructions" (Nov 12, 2025)](../sources/agent-context-guidance.md#github-changelog-copilot-code-review-and-coding-agent-now-support-agent-specific-instructions-nov-12-2025) (AI agent-context files: standards and vendor guidance)
- [Skillwright: "GitHub Copilot Instructions: The Complete Guide" (n.d.)](../sources/agent-context-guidance.md#skillwright-github-copilot-instructions-the-complete-guide-nd) (AI agent-context files: standards and vendor guidance)
- [OpenAI: "Harness engineering: leveraging Codex in an agent-first world" (Feb 11, 2026)](../sources/agent-context-guidance.md#openai-harness-engineering-leveraging-codex-in-an-agent-first-world-feb-11-2026) (AI agent-context files: standards and vendor guidance)
- [Gloaguen, Mündler, Müller, Raychev & Vechev: "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv Feb 2026; ICLR 2026)](../sources/agent-context-research.md#gloaguen-mündler-müller-raychev--vechev-evaluating-agentsmd-are-repository-level-context-files-helpful-for-coding-agents-arxiv-feb-2026-iclr-2026) (AI agent-context files: empirical research)
- [Lulla et al.: "On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents" (arXiv Jan 2026; v2 Mar 30, 2026)](../sources/agent-context-research.md#lulla-et-al-on-the-impact-of-agentsmd-files-on-the-efficiency-of-ai-coding-agents-arxiv-jan-2026-v2-mar-30-2026) (AI agent-context files: empirical research)
- [Khatri: "Do Context Files Help Coding Agents? A Two-Agent Ablation Study on Real Repositories" (arXiv Jul 28, 2026)](../sources/agent-context-research.md#khatri-do-context-files-help-coding-agents-a-two-agent-ablation-study-on-real-repositories-arxiv-jul-28-2026) (AI agent-context files: empirical research)
- [Chatlatanagulchai et al.: "Agent READMEs: An Empirical Study of Context Files for Agentic Coding" (arXiv Nov 2025)](../sources/agent-context-research.md#chatlatanagulchai-et-al-agent-readmes-an-empirical-study-of-context-files-for-agentic-coding-arxiv-nov-2025) (AI agent-context files: empirical research)
- ["Configuration Smells in AGENTS.md Files: Common Mistakes in Configuring Coding Agents" (arXiv Jun 2026)](../sources/agent-context-research.md#configuration-smells-in-agentsmd-files-common-mistakes-in-configuring-coding-agents-arxiv-jun-2026) (AI agent-context files: empirical research)
