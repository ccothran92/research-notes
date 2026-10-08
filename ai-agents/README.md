[Overview](../README.md)

# AI-era addendum

*Labels: [Evidence] = controlled or empirical study; [Vendor] = tool-maker guidance; [Emerging] = practitioner reports; [Synthesis] = this report's recommendation. Dates are given because this area moves quickly.*

## Agent-context files (as of Oct 2026)

- **AGENTS.md** is an open Markdown convention, "a README for agents", with no required fields. It emerged from OpenAI Codex, Amp, Google Jules, Cursor and Factory, was formalized around August 2025, and has been stewarded by the Linux Foundation's Agentic AI Foundation since December 9, 2025. The site claims use by "over 60k open-source projects". [Vendor]
- **CLAUDE.md** (Claude Code) is loaded every session, with enterprise/user/project/local scopes and recursive discovery. Anthropic's best-practices page: "Keep it concise. Bloated CLAUDE.md files cause Claude to ignore your actual instructions!" It says to include "Bash commands Claude can't guess" and exclude "anything Claude can figure out by reading code". Anthropic's own memory docs ("How Claude remembers your project") set the budget: "target under 200 lines per CLAUDE.md file", because "Files over 200 lines consume more context and may reduce adherence." [Vendor]
- **GitHub Copilot** uses `.github/copilot-instructions.md`, path-scoped `.github/instructions/*.instructions.md` with `applyTo` globs (`excludeAgent` added Nov 2025), and also reads AGENTS.md and CLAUDE.md. [Vendor]
- **Recommendation:** make AGENTS.md canonical and keep tool-specific files as thin shims or symlinks. [Synthesis]

> **Correction:** Claude Code reads CLAUDE.md, not AGENTS.md, so where Claude Code is used the CLAUDE.md shim (import or symlink) is mandatory, not optional. See [corrections and caveats](../sources/README.md#corrections-and-caveats-found-while-compiling-the-sources), item 1.

## In this section

- [What the evidence says](evidence.md): the 2025–2026 studies and what they imply
- [Structuring agent context](structuring-agent-context.md): how AGENTS.md relates to architecture.md, what agents need, progressive disclosure
- [AI-generated docs and drift detection](generated-docs-and-drift.md): risks of generated docs, using AI to detect drift
- [Template: AGENTS.md](../templates/agents-md.md)

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
