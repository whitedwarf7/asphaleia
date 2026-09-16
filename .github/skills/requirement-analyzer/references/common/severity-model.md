# Severity Model

Severity is a qualitative triage judgment for this proposal, not CVSS, a legal conclusion, or a numerical risk calculation. Keep requirement priority and clarification priority separate.

| Severity | Scenario-based criterion | Review response |
| --- | --- | --- |
| Critical | Credible design path to widespread unauthorized sensitive-data access, irreversible core-data loss, or similarly catastrophic failure with no viable control | Block affected readiness; require an explicit mitigation and stakeholder validation. |
| High | Credible serious confidentiality, integrity, availability, privacy, or recovery harm; major control/invariant is absent or demonstrably inadequate | Prioritize design change, evidence gathering, and validation before detailed-design readiness. |
| Medium | Bounded, recoverable harm or significant maintainability/operability weakness with a credible workaround | Plan mitigation and verify its scope and residual risk. |
| Low | Limited-impact ambiguity, hygiene, or documentation weakness that does not materially change architecture | Track a proportionate improvement without overstating urgency. |

## Assigning severity

1. State the affected component/requirement, plausible failure or threat scenario, exposure, consequence, and known controls.
2. Distinguish Confirmed defect, Proposed design risk, and Missing evidence. Missing documentation is not proof that a live system lacks a control.
3. Use the highest justified consequence, considering credible mitigations and scope. Do not assume an adversary's capability, a workload level, a recovery target, or a legal obligation without evidence.
4. A finding must have one of the four severities and a reason. If evidence is incomplete, explicitly mark the severity provisional and list validation that could raise or lower it. Do not automatically label every unknown Critical or High.
5. A raw risk that cannot yet be assessed may have Likelihood Unknown, Impact Unknown, and Severity Unassessed. Convert it to a severity-classified finding only with a stated plausible scenario and rationale.
6. Record mitigation as Proposed until implemented evidence is supplied. Residual risk is the remaining exposure after the proposed mitigation, not an assertion that the risk is eliminated.
7. Do not invent owners, acceptance authority, probability percentages, compliance status, or risk appetite. Use Unassigned when no owner is supplied.

## Cross-review reconciliation

- The skill that raised a finding owns its canonical record: SEC for security, REL for reliability, OPS for operations, REV only for a distinct final-review or net cross-cutting issue. Other reviewers link the original ID and shared RISK rather than duplicating the scenario.
- If overlapping findings describe the same cause, impact and mitigation, propose consolidation to their owners; retain original IDs through explicit supersession. A final reviewer cannot silently close a finding or change its severity.
- For a combined scenario, use the highest severity justified by the combined consequence and known controls, not a sum of finding counts. Explain any proposed change from specialist severity and request owner validation. Different scenarios may retain different justified severities even when they share a RISK.
- Keep finding severity, requirement priority, clarification priority and readiness independent. A Critical Q blocks its dependent decision without automatically becoming a Critical security finding. A Ready review artifact can recommend Not ready for the design.

## Calibration examples

- High, provisional: a confidential-document proposal describes authentication but no document authorization. Cross-user disclosure is plausible; confirm entitlements and test negative access paths. Absence from the proposal is not evidence of a production breach.
- Medium: a retry policy has bounded attempts but no single owner, creating a plausible bounded amplification risk. Verify composed retry behavior before concluding impact.
- Low: component names differ between two diagrams but the same stable ID makes ownership unambiguous. Correct labels and rerun consistency review.
- Unassessed risk: recovery objectives are absent and business impact is unknown. Ask for objectives and impact rather than fabricating a severity from an assumed outage duration.