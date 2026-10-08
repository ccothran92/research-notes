[Overview](../README.md) › [Sources](README.md)

# Software supply chain (Part 5)

## SLSA v1.0: Security levels
<https://slsa.dev/spec/v1.0/levels>

**Type:** Industry specification (OpenSSF) · **Label:** Established · **Used in:** [Example 1: CI/CD platform](../examples/cicd-platform.md)

This is the source of the Build L1, L2 and L3 definitions used in the GOOD CI/CD architecture doc:

- **L1:** provenance exists.
- **L2:** signed provenance comes from a hosted build platform.
- **L3:** the build platform is hardened, with isolation between runs and signing secrets kept away from user-defined steps.

The report's "Zone U vs Zone T" trust-boundary diagram is a direct application of L3. Check whether a newer SLSA version has superseded v1.0 before you write it into policy.

## Sigstore documentation
<https://docs.sigstore.dev/>

**Type:** Project documentation (OpenSSF; living) · **Label:** Established · **Used in:** [Example 1: CI/CD platform](../examples/cicd-platform.md)

This covers keyless signing, which associates signatures with identities rather than long-lived keys, using short-lived Fulcio certificates and the Rekor transparency log. It backs the invariant "user-defined build steps never see signing material."

## NTIA: "The Minimum Elements for a Software Bill of Materials (SBOM)" (Jul 2021)
<https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom>

**Type:** U.S. government guidance · **Label:** Established (now superseded) · **Used in:** [Example 1: CI/CD platform](../examples/cicd-platform.md)

This is the original seven-field SBOM baseline. **The 2026 CISA guidance below replaces it.** Keep it only for historical context and for mapping older tooling.

## CISA et al.: "2026 Minimum Elements for a Software Bill of Materials (SBOM)" (Jul 29, 2026)
- Guidance PDF (hosted on IC3): <https://www.ic3.gov/CSA/2026/260729.pdf>
- Partner publication page (NCSC New Zealand, Jul 31, 2026): <https://www.ncsc.govt.nz/protect-your-organisation/2026-minimum-elements-for-a-software-bill-of-materials-sbom-published/>
- Press-release coverage (HSToday, republishing CISA's statement): <https://www.hstoday.us/subject-matter-areas/cybersecurity/cisa-updates-software-bill-of-materials-guidance-to-strengthen-supply-chain-security/>
- Analysis (FOSSA, Jul 30, 2026): <https://fossa.com/blog/cisa-releases-2026-minimum-sbom-elements/>
- Analysis (Paubox): <https://www.paubox.com/blog/cisa-updates-sbom-guidance-with-ten-new-data-fields>
- Analysis (TechInformed): <https://techinformed.com/sbom-guidance-adds-detail-but-no-test-for-missing-components/>

**Type:** U.S. and international joint government guidance (CISA, NSA, FBI and partners) · **Label:** Established (new) · **Used in:** [Example 1: CI/CD platform](../examples/cicd-platform.md), [Open questions](../topics/open-questions.md)

This is the current SBOM baseline. It applies to all software, including open source, AI software and SaaS. It adds new required elements, including:

- Component Hash Algorithm
- Component License
- SBOM Tool Name
- SBOM Generation Context

It also adds a digital-signature field and renames several existing elements. Trade-press coverage describes the field count as going from 7 to 17. Take the exact element names from Appendix A of the PDF, not from news coverage, when you write policy gates. AI-specific fields are deferred to separate CISA/G7 AI SBOM guidance from May 2026.
