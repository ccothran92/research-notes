[Overview](../README.md)

# When to create architecture docs (Q1)

## Triggers [Established + Synthesis]

| Trigger | Why | Minimum response |
|---|---|---|
| Repo passes ~10k LOC or a second team contributes | matklad targets 10k–200k LOC. Newcomers take "2x more time to write a patch" but "10x more time to figure out *where*" to change code | architecture.md with codemap |
| New service/deployable | New container; consumers need contract + owner | Catalog entry + stub + spec |
| New system or trust boundary | Boundaries "constrain all *possible* implementations" but are hard to find from code | Boundaries section + ADR |
| Architecturally significant decision | Lost rationale lets later decisions "defeat the earlier decisions" (Nygard) | ADR |
| Onboarding pain | Direct evidence that the mental map is missing | Codemap + "where to start" |
| Compliance/audit | Auditors need boundaries, controls, evidence | Controls section |
| Recurring incidents or agent mistakes rooted in misunderstanding | Invariant was implicit | Write the invariant (and an AGENTS.md one-liner) |

## When NOT to write [Established]

- **Too early:** Google notes design-doc overhead "may not be compatible with prototyping and rapid iteration", but agility is "not an excuse" to ignore known problems.
- **Too volatile:** C4's FAQ says component diagrams "may change frequently" and code diagrams "potentially become outdated very quickly". Document only the invariants you are sure of.
- **Trivially derivable:** class lists, endpoints, dependency graphs, env vars. Anthropic gives agents the same rule: exclude "anything Claude can figure out by reading code".
- **No ambiguity:** no design doc. Any ADR is three sentences.

---

## Sources

- [matklad (Alex Kladov): "ARCHITECTURE.md" (Feb 6, 2021)](../sources/architecture-practice.md#matklad-alex-kladov-architecturemd-feb-6-2021) (Architecture documentation practice)
- [Malte Ubl: "Design Docs at Google" (2020)](../sources/architecture-practice.md#malte-ubl-design-docs-at-google-2020) (Architecture documentation practice)
- [Michael Nygard: "Documenting Architecture Decisions" (Nov 15, 2011)](../sources/architecture-practice.md#michael-nygard-documenting-architecture-decisions-nov-15-2011) (Architecture documentation practice)
- [ADR explainers (secondary)](../sources/architecture-practice.md#adr-explainers-secondary) (Architecture documentation practice)
- [C4 model: FAQ](../sources/frameworks-and-modelling.md#c4-model-faq) (Frameworks and architecture modelling)
- [Anthropic: Claude Code "Best practices"](../sources/agent-context-guidance.md#anthropic-claude-code-best-practices) (AI agent-context files: standards and vendor guidance)
