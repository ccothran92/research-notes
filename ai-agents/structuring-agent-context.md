[Overview](../README.md) › [AI agents](README.md)

# Structuring agent context

*Labels: [Evidence] = controlled or empirical study; [Vendor] = tool-maker guidance; [Emerging] = practitioner reports; [Synthesis] = this report's recommendation. Dates are given because this area moves quickly.*

## How AGENTS.md relates to architecture.md

```
AGENTS.md  (≤ ~100–200 lines; loaded every session)
  ├─ Commands: build, single test, lint, typecheck, run
  ├─ Non-default conventions the agent would get wrong
  ├─ Do-not-touch zones + why (generated, migrations, vendored, security-critical)
  ├─ Invariants as one-liners WITH pointer to canonical section
  └─ "Read before structural changes": ARCHITECTURE.md §Boundaries, docs/adr/index.md
ARCHITECTURE.md, docs/adr/, specs, generated/  (read on demand)
```

What counts as a non-default convention, with three filled-in AGENTS.md files: [Non-default conventions](../concepts/non-default-conventions.md).

**Rules [Synthesis, grounded in the sources above]:**

- AGENTS.md never restates the overview; it points to it (the ETH result, and OpenAI's "map" framing).
- Invariants are the one exception worth duplicating as one-liners, because they change agent *decisions*. Each carries a pointer to its canonical section so drift is checkable.
- Use nested AGENTS.md files for package-specific commands, not to arbitrarily split one big file.
- Task-specific instructions go into on-demand skill files, which addresses "skill leakage".

## What agents need that humans can often infer

- **Exact commands.** Humans ask a colleague; agents guess.
- **Explicit negative constraints:** "never edit `gen/`", "never add a dependency to `core`". See [Absences](../concepts/absences.md).
- **Enforcement over prose.** Move constraints into import linters, build-graph visibility, architecture tests and IAM. A failing check steers an agent more reliably than a sentence, which is consistent with the "lint leakage" finding. [Synthesis]
- **Dependency direction**, stated once in a consistent place.

## Context-window economics and progressive disclosure

Always-loaded text is paid for every session and competes with the task. OpenAI names the failure modes: crowding, guidance overload, rapid rot, unverifiability. Use three tiers:

1. An always-on router (AGENTS.md).
2. On-demand canonical docs (ARCHITECTURE.md, ADR and contract indexes).
3. Generated reference (schemas, specs).

Consistent headings across repos (`## Codemap`, `## Boundaries`, `## Architecture invariants`) make tier-2 retrieval reliable for agents and searchable for humans. A small YAML front matter (owner, last_reviewed, system ID, status) is cheap and useful. Elaborate per-section schemas are unsupported by evidence; AGENTS.md itself deliberately has "no required fields". [Emerging/Synthesis]
