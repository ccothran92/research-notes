[Overview](../README.md)

# Why: problems solved, by reader (Q2)

| Reader | What they need | Theater to avoid |
|---|---|---|
| New engineers | Codemap, entry points, invariants, glossary | Org history; aspirational architecture presented as current |
| Cross-team consumers | Contract index, versioning policy, SLOs, owner, sync/async semantics | Internal component detail |
| Security/compliance | Trust boundaries, data classification, enforcement points, evidence links | Copy-pasted policy text, config screenshots |
| Incident responders | Dependency direction, failure modes, degradation, runbook links | Runbook steps embedded in prose |
| Architects/leadership | Context/container views, ADR index, risks | 40-page arc42 full of "N/A" |
| AI coding agents | Commands, non-default conventions, do-not-touch paths, pointers | Repository overviews; rules a linter already enforces |

**Evidence of value.** DORA's "Documentation quality" capability page says DORA "has found a clear link between documentation quality and organizational performance", which it defines as "the organization's ability to meet their performance and profitability goals." Google Cloud's announcement of the 2023 Accelerate State of DevOps Report puts numbers on it: "SRE practices are estimated to have 1.4x more impact on organizational performance when high-quality documentation is in place," and "high-quality documentation leads to 25% higher team performance relative to low-quality documentation." The 2021 report called documentation "foundational". DORA measures quality (eight metrics such as clarity, findability and reliability), not volume. [Established, correlational survey research]

**Theater test [Synthesis].** Verify three random claims in the doc against the system. If any is wrong and nobody has filed an issue, the doc has no readers.

---

## Sources

- [DORA: "Documentation quality" capability](../sources/dora.md#dora-documentation-quality-capability) (Documentation and organisational performance (DORA))
- [Google Cloud blog: "2023 State of DevOps Report: Culture is everything" (Oct 2023)](../sources/dora.md#google-cloud-blog-2023-state-of-devops-report-culture-is-everything-oct-2023) (Documentation and organisational performance (DORA))
- [DORA: "Accelerate State of DevOps 2021" (Sep 2021)](../sources/dora.md#dora-accelerate-state-of-devops-2021-sep-2021) (Documentation and organisational performance (DORA))
