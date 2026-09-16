# Security architecture reviewer — worked examples

These original, synthetic examples illustrate [the skill](./SKILL.md), not an executed review, control test, certification, or approved design. Every entity, classification, source, and scenario below is fictional. There are no real secrets, personal records, confidential identifiers, endpoints, or copied source passages. Fixture prose is raw input, not a shortened shared output register. Confirmed means stated in the fixture, never verified in production. Identifiers belong to the stated design; separate examples have separate namespaces.

Use the exact [shared contracts](./references/common/requirement-schema.md), [principles](./references/common/architecture-principles.md), [severity model](./references/common/severity-model.md), [checklist](./references/common/review-checklist.md), and [orchestration](./references/orchestration.md). [Sources](./references/SOURCES.md) provide conceptual provenance, not subject-system policy. The [manual tests](./tests.md) evaluate behavior independently of these expected answers. No numeric risk scores, service settings, owners, or policy defaults are implied.

## Example 1: Documented object-authorization defect

### User prompt

Review object authorization in design SYN-SEC-A using this authorized synthetic baseline. Identify the design defect, a proportionate mitigation, residual risk, and safe validation. Use SEC-101 for the finding. Do not certify security or perform access tests.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-101 | Synthetic baseline and access rule supplied for this example | fixture:SYN-SEC-A/baseline | Synthetic user input; authorized for this written review only | Available inline; no external access authorized |

Architecture facts in SRC-101: ACT-101 is a simulated signed-in reader. CMP-101 is the independently deployable Card reader; CMP-102 is the Card store and owns DATA-101. DATA-101 consists only of generated story cards, classified sensitive inside this fictional scenario; its collection purpose is group-restricted reading, owner is not supplied, and retention, deletion, and geographic constraints are unspecified. TB-101 separates the caller's asserted resource/group context from server-side entitlement enforcement. FLW-101 carries a card request from ACT-101 through CMP-101 to CMP-102 and returns the card. INT-101 is that synchronous read contract; it explicitly authenticates the caller but performs no object/group authorization in either component. A valid session can read another group's card within this bounded collection. Anonymous access is rejected. There is no bulk export or administrative interface in this scoped baseline. Other security controls and numeric operating targets are outside the supplied evidence, not presumed absent. No runtime results or approval evidence are supplied.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-101 | Functional (FR) | Return a requested story card to an entitled signed-in reader | SRC-101, capability | Must | Confirmed | High — explicit fixture statement | High — read boundary and response | None — capability is explicit |
| BR-101 | Business rule (BR) | A reader may read only cards belonging to its authorized group | SRC-101, access rule | Must | Confirmed | High — explicit fixture statement | High — resource authorization | None — entitlement rule is explicit |

### Expected activation

Yes — an authorized, focused architectural security review with sufficient evidence of a documented design absence.

### Expected response

**Scope excerpt — not a full review.** This exhibits the handoff, selected coverage, finding, validation, and traceability records. It does not replace the full ordered artifact or its security coverage matrix. Unshown areas must not be described as reviewed or secure.

**Handoff**

- Design ID: SYN-SEC-A.
- Contract version: 1.0.0.
- Artifact: security-architecture-reviewer — scenario-based security review.
- Artifact status: Ready — the focused authorization finding is supported by the written baseline; this is not a complete security review or a system-readiness recommendation.
- Baseline: SRC-101 at fixture:SYN-SEC-A/baseline; version Unspecified because none was supplied.
- Evidence summary: Confirmed — BR-101 and the explicitly absent resource check in INT-101; Inferred — the resulting cross-group disclosure path; Assumed — None, no assumption is needed for this finding; Proposed — entitlement enforcement and validation below; Unresolved — implementation behavior and controls outside this scope.
- Changed IDs: Add SEC-101. Proposed changes to CMP-101 and INT-101 are not authoritative edits. No inherited record is superseded.
- Open questions: None in this focused active batch; no deferred questions supplied. Broader policy and lifecycle evidence remain outside this excerpt, not answered by it.
- Validation: Expected written comparison of BR-101 with INT-101 only; this example records no performed checks or tool results. Authorization tests, implementation inspection, and stakeholder mitigation agreement are Not run — pending evaluation.
- Next handoff: reliability-scalability-reviewer receives SEC-101, the unchanged source/requirement records, affected entity and flow facts, and validation gaps. integration-designer and high-level-design-generator receive the proposed INT-101/CMP-101 delta; rerun affected security checks after incorporation. HLD assembly targets sections 18 and 27.

**Review scope and evidence**

Reviewed scope: object authorization across TB-101 in FLW-101. Excluded scope and rationale: unrelated security areas lack evidence and are not part of this focused request. Baseline limitations: written design only, not implementation evidence. Supplied authority: SRC-101 permits written analysis, not scans, testing, risk acceptance, or deployment. Carry the complete source and requirement rows above unchanged.

**Security coverage — selected row only**

| Area | Review status | Evidence and affected IDs | Threat scenario or gap | Finding or risk IDs | Validation required |
| --- | --- | --- | --- | --- | --- |
| Authorization | Gap | SRC-101; BR-101; ACT-101, CMP-101, CMP-102, DATA-101, TB-101, FLW-101, INT-101 | An authenticated reader can receive another group's restricted card because the written contract explicitly omits entitlement enforcement | SEC-101 | Review enforcement location and validate permitted and denied group/resource combinations using generated data |

**Security findings**

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SEC-101 | High | Resource authorization is explicitly absent from the scoped read design | CMP-101 and CMP-102; ACT-101, DATA-101, TB-101, FLW-101, INT-101 | Finding type: Confirmed defect in the supplied design, not a confirmed deployed defect. Safe threat scenario: an authenticated reader requests a card outside its authorized group and receives restricted content. Evidence class and source: Confirmed absence and access rule in SRC-101; disclosure consequence Inferred from that contract. Exposure and known controls: bounded collection; authentication excludes anonymous callers but does not enforce group membership. Severity rationale: serious confidentiality harm from a demonstrably missing major control; the evidence does not establish widespread unrestricted system access or a catastrophic path justifying Critical. Severity status: assessed for this written scope; runtime behavior Unresolved. | Proposed: enforce server-derived action/resource/group entitlements before reading or returning DATA-101; deny on missing context and prevent a direct store path bypassing the check. Compare enforcement in CMP-101 with a reusable boundary policy, considering consistency and added coupling. Ask the integration and HLD owners to incorporate the chosen design; do not select a product. | Stale entitlements, inconsistent enforcement on later flows, and previously exposed copies may remain after the proposed change. No risk acceptance or complete isolation is implied. | Inspect the revised INT-101 and entitlement derivation; in an authorized sandbox use generated allowed-group and denied-group cases, missing-context cases, and access revocation cases. Expected outcome: allowed reads succeed, denied reads return no card content, and safe audit evidence distinguishes the decisions. Confirm implementation and stakeholder mitigation agreement before the detailed-design gate. | FR-101, BR-101 |

**Validation plan — proposed, not executed**

| Finding or risk ID | Validation scenario | Evidence required | Expected observable outcome | Owner, if supplied | Status | Residual uncertainty |
| --- | --- | --- | --- | --- | --- | --- |
| SEC-101 | Allowed, denied, revoked, and absent group context at the same read boundary | Revised contract, authorized entitlement review, and redacted results using generated cards | Returned content always agrees with the resolved entitlement; denials expose no card content | Unassigned — no owner supplied | Not run — pending evaluation | Tests cover only the supplied read path; additional paths and entitlement propagation still require review |

**Requirements traceability — all requirements in this fixture**

| Requirement ID | Component IDs | Decision IDs | Flow or integration IDs | Validation | Coverage status |
| --- | --- | --- | --- | --- | --- |
| FR-101 | CMP-101, CMP-102 | None — no decision record supplied or accepted | FLW-101, INT-101 | Allowed and denied read validation under SEC-101, not run. The response path exists, but the entitlement defect remains | Partial |
| BR-101 | CMP-101, CMP-102 | None — mitigation is a proposed owner-directed delta | FLW-101, INT-101 | Entitlement and revocation validation under SEC-101, not run. Enforcement is explicitly absent from the written design | Unaddressed |

High-priority gate: an agreed mitigation/validation plan is required before the final owner recommends detailed-design readiness. The scoped artifact being Ready does not satisfy that gate. No shared RISK is fabricated merely to duplicate SEC-101.

## Example 2: Conflicting sharing rules and undocumented enforcement

### User prompt

Review the sharing boundary in SYN-SEC-B. The sources disagree about who may read a shared card, and the design describes login but says nothing about resource checks. Show what can be concluded safely; do not choose the entitlement policy for us.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-201 | Synthetic creator-group access statement and baseline | fixture:SYN-SEC-B/creator-rule | Synthetic user input; written review authorized; precedence not supplied | Available inline |
| SRC-202 | Synthetic shared-card access statement, described as newer | fixture:SYN-SEC-B/sharing-rule | Synthetic user input; no overriding authority supplied | Available inline |

ACT-201 is a simulated signed-in group member. CMP-201 is the Card reader, and CMP-202 is the store owning DATA-201, generated cards treated as sensitive only in this fixture. Purpose is group sharing; owner, lifecycle policy, and geographic constraints are unspecified. TB-201 is the caller-to-resource authorization boundary. FLW-201 is card retrieval through CMP-201 and CMP-202; INT-201 describes synchronous authentication and retrieval but omits authorization details. Neither source says an implemented check is absent. There is no definition of what makes a card shared, no evidence of a security incident, and no supplied authority to resolve the conflict. The scoped collection is bounded; there is no evidence of whole-system export.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| BR-201 | Business rule (BR) | Only the creating group may read a card | SRC-201, creator rule | Must | Conflicted | High — text is explicit; precedence is not | High — entitlement boundary | Q-201 |
| BR-202 | Business rule (BR) | Any signed-in group may read a shared card | SRC-202, sharing rule | Must | Conflicted | High — text is explicit; shared-card meaning is unresolved | High — entitlement boundary | Q-201 |

### Expected activation

Yes — security review can preserve the conflict and assess an evidence gap without granting access or inventing policy.

### Expected response

**Scope excerpt — not a full review.** All fields of the exhibited finding and question records are retained. The remaining security matrix and inherited registers are not represented as completed here.

**Handoff**

- Design ID: SYN-SEC-B.
- Contract version: 1.0.0.
- Artifact: security-architecture-reviewer — scenario-based security review.
- Artifact status: Provisional — useful evidence-gap analysis exists; the entitlement-dependent control choice is Blocked by Q-201.
- Baseline: SRC-201 and SRC-202 at their safe fixture locators; version Unspecified.
- Evidence summary: Confirmed — both conflicting source statements and the omission from INT-201; Inferred — possible disclosure if the intended group check is not enforced; Assumed — None, no access-granting assumption is permitted; Proposed — conservative boundary enforcement and validation after resolution; Unresolved — sharing semantics, source precedence, and actual enforcement.
- Changed IDs: Add SEC-201, Q-201, and Q-202. Preserve BR-201 and BR-202 as Conflicted; do not supersede the older source.
- Open questions: Active Q-201 Critical and Q-202 Important, in that order; no deferred questions supplied. Q-201 blocks entitlement selection; Q-202 prevents a supported conclusion about control implementation.
- Validation: No source conflict has been resolved and no control test was executed. Contract, negative-access, and stakeholder checks are Not run — pending evaluation.
- Next handoff: requirement-analyzer receives Q-201 and both unchanged requirements; integration-designer receives the INT-201 evidence request. reliability-scalability-reviewer receives SEC-201 and the blocked decision without treating mitigation as implemented. Rerun affected security areas after authoritative answers.

Carry the complete source and requirement rows above unchanged. Reviewed scope is group/resource authorization only; other areas are excluded from this excerpt because evidence was not supplied. Supplied authority allows analysis, not policy resolution. Authentication documentation is not authorization evidence.

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SEC-201 | High | Resource enforcement is undocumented while sharing entitlement is conflicted | CMP-201, CMP-202; ACT-201, DATA-201, TB-201, FLW-201, INT-201 | Finding type: Missing evidence, not a confirmed missing production control. Safe threat scenario: a signed-in group could receive another group's restricted card if the intended entitlement is not enforced. Evidence class and source: Confirmed omission in SRC-201's contract description and Confirmed conflicting statements in SRC-201/SRC-202; the exposure is Inferred and conditional. Exposure and known controls: authentication is described; resource checks are unknown; scope is a bounded sensitive collection. Severity rationale: plausible serious confidentiality harm justifies provisional High, but neither an observed breach nor a catastrophic unrestricted path is evidenced. Severity status: Provisional; correct enforcement could remove this concern, while broader evidenced exposure could raise it. | Proposed: retain a deny-on-unresolved-entitlement design option and compare it with explicitly authorized sharing after Q-201 is answered. Request the actual enforcement description under Q-202. Do not implement a denial policy, grant cross-group access, choose the newer rule, or mark either requirement satisfied on the reviewer's authority. | Even after a resolved rule and proposed check, stale sharing state, revocation propagation, and copies can retain exposure. The effectiveness and acceptability of that remaining risk are not established. | Obtain an authorized resolution identifying the scope of shared cards and an authorized redacted enforcement description. Then validate allowed, disallowed, revoked-sharing, and missing-context cases in a permitted sandbox; observable results must match the resolved rule without returning disallowed content. Reassess severity from this evidence. | BR-201, BR-202 |

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-201 | Critical | Which authorized entitlement rule governs cards described as shared? | Choosing a rule could grant rights denied by the other source | Supply an authorized scope/precedence resolution; clarify distinct scopes for both rules; leave the conflict unresolved | None — blocked | Group/resource entitlement selection in INT-201 and dependent mitigation validation | Open | BR-201, BR-202 |
| Q-202 | Important | What resource-authorization evidence is available for INT-201? | An undocumented check must not be asserted absent or effective | Authorized redacted design evidence; authorized negative-case results; evidence unavailable | Enforcement remains Unresolved; continue only the conditional evidence-gap review | A supported claim about enforcement and final SEC-201 severity | Open | BR-201, BR-202 |

These are the entire active batch, not additional questions after a hidden batch. No Optional question is needed. If an upstream ledger existed, it would be retained inside the combined limit of seven, with overflow deferred. No assumption accepts risk or resolves Q-201. No duplicate RISK is added solely for SEC-201. Owner remains Unassigned where needed; nobody has accepted this risk.

## Example 3: Container-view request belongs to its diagram owner

### User prompt

For SYN-SEC-C, draw only a container view from the supplied facts. Do not conduct a security review, choose controls, or certify the system.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-301 | Synthetic written container-view facts | fixture:SYN-SEC-C/view | Synthetic user input for diagram planning only | Available inline |

ACT-301 is a generated test requester. CMP-301 is a deployable Lookup service; CMP-302 is its Card store, owning DATA-301, generated public cards with no personal content. TB-301 separates client input from the service. FLW-301 carries a lookup to CMP-301, then a read from CMP-302, then a reply. Protocol, policies, operating targets, and owners are unspecified. No security assessment was requested.

### Expected activation

No — the primary request is container-diagram generation, owned by `container-diagram-generator`.

### Expected response

This request belongs to `container-diagram-generator`. Pass the supplied IDs and written interactions to that owner for a stable Mermaid container view and bidirectional reconciliation with the written records. Do not infer that a trust-boundary outline proves isolation, and do not claim parser or render checks were performed.

- Design ID: SYN-SEC-C.
- Contract version: 1.0.0.
- Artifact: security-architecture-reviewer — scenario-based security review.
- Artifact status: Not applicable — no security review requested.
- Baseline: SRC-301 at fixture:SYN-SEC-C/view; version Unspecified.
- Evidence summary: Confirmed — the supplied diagram facts; Inferred — None, no security inference attempted; Assumed — None; Proposed — routing only; Unresolved — protocol and operating details not supplied and not resolved by routing.
- Changed IDs: None — no security or upstream records changed.
- Open questions: None — routing does not require a security clarification batch; no deferred ledger supplied.
- Validation: No diagram, security, parser, render, or control checks performed; downstream checks remain Not run — pending evaluation.
- Next handoff: container-diagram-generator receives the complete SRC-301 record and declared ACT-301/CMP-301/CMP-302/DATA-301/TB-301/FLW-301 facts. It must retain unspecified protocols and reconcile both directions; this response does not claim that handoff execution occurred.

No SEC finding or security coverage result is fabricated to fill a non-activation response. This is a routing example, not a full review or proof that no defects exist.