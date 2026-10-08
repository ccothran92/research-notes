[Overview](../README.md) › [Sources](README.md)

# AI agent-context files: standards and vendor guidance (Part 6)

## AGENTS.md (project site)
<https://agents.md/>

**Type:** Open convention (living) · **Label:** Vendor/Emerging · **Used in:** [AI agents](../ai-agents/README.md)

This is the canonical description of AGENTS.md as a "README for agents" with no required fields. The site claims adoption by more than 60k open-source projects; that number is self-reported.

## Linux Foundation: "Linux Foundation Announces the Formation of the Agentic AI Foundation (AAIF)…" (Dec 9, 2025)
<https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation>

**Type:** Press release · **Label:** Established (governance fact) · **Used in:** [AI agents](../ai-agents/README.md)

This confirms that AGENTS.md (released by OpenAI in Aug 2025) was contributed to the new Agentic AI Foundation alongside Anthropic's MCP and Block's goose. The report uses it to show that AGENTS.md now has neutral stewardship, which reduces vendor lock-in risk for an enterprise standard.

## OpenAI: "OpenAI co-founds the Agentic AI Foundation under the Linux Foundation" (Dec 9, 2025)
<https://openai.com/index/agentic-ai-foundation/>

**Type:** Vendor announcement · **Label:** Vendor · **Used in:** [AI agents](../ai-agents/README.md)

This is OpenAI's side of the same announcement. It gives the 60,000+ adoption figure and lists agents that read AGENTS.md (Amp, Codex, Cursor, Devin, Factory, Gemini CLI, GitHub Copilot, Jules, VS Code). The adoption number is vendor-reported.

## Anthropic: Claude Code "Best practices"
<https://code.claude.com/docs/en/best-practices>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** [When to write](../topics/when-to-write.md), [AI agents](../ai-agents/README.md)

This advises keeping CLAUDE.md concise, because bloated files cause instructions to be ignored. It says to include commands Claude can't guess and to exclude anything Claude can work out by reading the code. That guidance is the basis of the report's "router, not encyclopedia" rule.

## Anthropic: Claude Code memory ("How Claude remembers your project")
<https://code.claude.com/docs/en/memory>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** [AI agents](../ai-agents/README.md)

This source sets two points:

- The size target is under 200 lines per CLAUDE.md, because longer files use more context and reduce adherence.
- The docs cover CLAUDE.md scopes and recursive discovery up and down the directory tree.

**It also states that Claude Code reads CLAUDE.md, not AGENTS.md.** This matters for the report's "AGENTS.md canonical + tool shims" recommendation ([AI agents](../ai-agents/README.md)): for Claude Code, the shim (an import or symlink) is *required*, not optional.

## GitHub Docs: "Adding custom instructions for GitHub Copilot CLI"
<https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions>

**Type:** Vendor documentation (living) · **Label:** Vendor · **Used in:** [Example 5: polyglot monorepo](../examples/polyglot-monorepo.md), [AI agents](../ai-agents/README.md)

This documents Copilot's instruction files: repository-wide `.github/copilot-instructions.md`, path-scoped `.github/instructions/*.instructions.md` with `applyTo` globs, and support for AGENTS.md and CLAUDE.md. It is the basis for the report's nested and path-scoped instructions advice for monorepos.

## GitHub Changelog: "Copilot code review and coding agent now support agent-specific instructions" (Nov 12, 2025)
<https://github.blog/changelog/2025-11-12-copilot-code-review-and-coding-agent-now-support-agent-specific-instructions/>

**Type:** Vendor changelog · **Label:** Vendor · **Used in:** [AI agents](../ai-agents/README.md)

This introduced `excludeAgent`, which lets one instruction file target the coding agent but not code review, or the reverse. It is relevant when the same repo serves several AI consumers.

## Skillwright: "GitHub Copilot Instructions: The Complete Guide" (n.d.)
<https://www.skillwright.app/blog/copilot-instructions-guide>

**Type:** Third-party guide · **Label:** Weak (secondary) · **Used in:** [AI agents](../ai-agents/README.md)

This is a secondary walkthrough of Copilot instruction mechanics. Use the GitHub Docs page above as the authority.

## OpenAI: "Harness engineering: leveraging Codex in an agent-first world" (Feb 11, 2026)
<https://openai.com/index/harness-engineering/>

**Type:** Vendor engineering case study · **Label:** Vendor (case study) · **Used in:** [AI agents](../ai-agents/README.md)

This is the most detailed public account of progressive disclosure in practice. OpenAI's team tried one big AGENTS.md, abandoned it, and moved to a short AGENTS.md of about 100 lines that serves as a table of contents. That file points into a structured `docs/` tree that includes ARCHITECTURE.md and is backed by mechanical freshness checks and a recurring doc-gardening agent.

**Caveat worth adding to the report:** the post says the team's *initial* AGENTS.md was written by Codex. That sits awkwardly with the report's "agent-context files are human-authored" rule. The post doesn't show whether later versions were human-curated. It is a single internal team with privileged tool access, so its results may not generalise.
