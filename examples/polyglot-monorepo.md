[Overview](../README.md) › [Worked examples](README.md)

# Example 5 — Large polyglot monorepo (stress test: one repo, many systems)

**GOOD:**

- A root `ARCHITECTURE.md` of ≤200 lines covering package taxonomy (apps/, services/, libs/, tools/), dependency-direction rules ("libs never depend on apps") enforced by the build graph (Bazel visibility, Nx module boundaries), and directory ownership via CODEOWNERS.
- A short architecture.md for each package with real internals.
- Nested AGENTS.md files for package-specific commands. Claude Code reads CLAUDE.md files recursively, and Copilot supports nested/path-scoped instructions.

**BAD:** a 3,000-line root doc covering every package, or 400 package READMEs with no dependency rules.

**Where it breaks down:** "one architecture.md per deployable" maps to packages, not repos. The system doc lives at `/architecture/`. Dependency rules belong in the build graph, with prose explaining *why*.

---

## Sources

- [GitHub Docs: "Adding custom instructions for GitHub Copilot CLI"](../sources/agent-context-guidance.md#github-docs-adding-custom-instructions-for-github-copilot-cli) (AI agent-context files: standards and vendor guidance)
