---
name: write-vex-statement
description: Writes or updates Vulnerability Exploitability eXchange (VEX) statements for exact software artifacts and vulnerabilities. Use when asked to create or update exploitability evidence for a VEX consumer, rather than to triage a scan or patch a dependency.
---

# How to Write VEX Text

VEX communicates a vulnerability's status in a specific product. It does not repair software or prove an entire artifact is safe. Inspect the artifact identity, scan finding, advisory, dependency evidence, and intended consumer before choosing a format or status. Prefer OpenVEX when the user and consuming tools do not require another format.

Read and apply `write-text` from the active skill catalog or [sibling skill](../write-text/SKILL.md) when drafting explanations. Reuse it if already loaded.

## Evidence and Status

Bind each statement to the exact product version or image digest and vulnerability identifier. Include component identifiers when needed to match the finding. Mutable tags alone are insufficient to establish which artifact was assessed.

Choose a status from the selected format using evidence for that artifact:

* `under_investigation` when applicability or exploitability remains unresolved.
* `affected` when the vulnerability affects the product. Include the required action statement describing remediation or mitigation.
* `fixed` when evidence establishes that the identified product contains the fix. An available upstream patch does not establish that the artifact is fixed.
* `not_affected` only when evidence establishes why the vulnerability does not affect the product. For OpenVEX, supply a supported justification or impact statement. Prefer a machine-readable justification supported by the evidence and explain its basis.

Do not infer `not_affected` from a low severity, lack of a public exploit, an unsuccessful test, or a desire to suppress a finding. Record the evidence, assumptions, and deployment conditions behind the assessment. Keep a proposed dependency update separate from the current artifact's status.

## Authoring and Verification

Preserve valid document identity, author, timestamps, version, and statement history. Use actual issuer information and issuance times. Reassess statements when the artifact or relevant conditions change.

Use tooling that supports the selected format and the installed version's documented commands. For supported versions of `vexctl`, `vexctl validate vex.json` checks OpenVEX conformance. Review warnings as well as errors. JSON syntax checks alone do not validate VEX semantics or evidence.

Verify that the intended consumer matches the exact artifact and vulnerability and interprets the status correctly. Preserve the original scan evidence when comparing results. Do not attach, sign, publish, or suppress findings beyond the authorized task. Report unresolved assessments and consumer limitations.

## Official References

* [OpenVEX specification](https://github.com/openvex/spec/blob/main/OPENVEX-SPEC.md) for document fields, status requirements, and justifications.
* [OpenVEX vexctl](https://github.com/openvex/vexctl) for supported authoring, validation, and consumer workflows.
