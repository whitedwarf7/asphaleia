# Structured Requirements — Employee Document Service

## Handoff

- Design ID: EMPLOYEE-DOCS.
- Contract version: 1.0.0.
- Artifact: requirement-analyzer — synthetic requirements baseline.
- Artifact status: Provisional — lifecycle authority and numeric quality targets remain unresolved; independent neutral design may continue.
- Baseline: SRC-001, version 1.0, [sample brief](./sample-requirements.md).
- Evidence summary: Confirmed source statements below; Inferred architecture impact; Assumed none in requirement statements; Proposed design premises only in the ASM register; Unresolved Q records.
- Changed IDs: initial source, requirement, entity, ASM and Q registers below.
- Open questions: Q-001 blocks irreversible deletion; Q-002–Q-007 are the remaining active batch; Q-008–Q-012 are queued.
- Validation: source-to-record extraction inspected as an authored example; no stakeholder approval, runtime test, or Copilot behavioral execution claimed.
- Next handoff: architecture-driver-analyzer receives this complete baseline and unchanged question ledger; changed source meaning returns to requirement-analyzer.

## Objective and scope

Enable controlled, reliable employee document access within one organization. In scope: authorized upload/list/retrieval, explicit grants, lifecycle/hold recording, existing identity integration, audit, operations/recovery, and accessible browser journeys. No business KPI or organizational owner is supplied. Confirmed exclusions are CON-001 and CON-003, not guessed deferrals.

## Sources

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | Original synthetic employee-document brief, version 1.0 | [S01–S10](./sample-requirements.md) | User-authorized fixture; no architecture approval or real organization policy | Supplied synthetic input |

## Structured Requirements

Every row preserves one source-backed requirement. High confidence means the statement is explicit, not that feasibility or implementation has been validated. Architecture impact is analysis. Missing target values are questions, not newly invented NFRs.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | Functional | Employees can upload work documents and obtain a result. | SRC-001 S01, S04 | Must | Confirmed | High | High — content ingestion and persistence | Q-002, Q-003 |
| FR-002 | Functional | Employees can list and retrieve documents they are authorized to access. | SRC-001 S01, S02 | Must | Confirmed | High | High — authorized query and retrieval path | Q-008, Q-012 |
| FR-003 | Functional | Records administrators can create and revoke explicit document access grants without an automatic content-reading privilege. | SRC-001 S02 | Must | Confirmed | High | High — separate administrative and content rights | Q-008, Q-012 |
| FR-004 | Functional | Records administrators can record retention dispositions and legal-hold state. | SRC-001 S05 | Must | Confirmed | High | High — lifecycle authority and irreversible actions | Q-001 |
| FR-005 | Functional | Users of each supplied role sign in through the existing corporate identity system; do not create a new identity directory. | SRC-001 S03 | Must | Confirmed | High | High — external identity trust boundary | Q-008 |
| FR-006 | Functional | Operations personnel can inspect service health and initiate recovery through authorized procedures without implicit document browsing rights. | SRC-001 S07 | Must | Confirmed | High | High — operational privilege and recovery boundaries | Q-005, Q-006, Q-011 |
| BR-001 | Business rule | Every list and retrieval evaluates current document grants and denies access when a required decision cannot be made. | SRC-001 S02, S03 | Must | Confirmed | High | High — fail-closed authorization and freshness | Q-008 |
| BR-002 | Business rule | Upload success follows durable recording of both content and metadata as a retrievable version; invalid or incomplete content is not available. | SRC-001 S04 | Must | Confirmed | High | High — cross-store publication correctness | Q-002, Q-003 |
| BR-003 | Business rule | Repeating the same upload intent does not publish a second document version. | SRC-001 S04 | Must | Confirmed | High | High — retry-safe identity and commit semantics | None — invariant explicit; mechanism Proposed |
| BR-004 | Business rule | Legal hold prevents document deletion. | SRC-001 S05 | Must | Confirmed | High | High — lifecycle enforcement and restoration | Q-001 |
| NFR-001 | Non-functional | Protect sensitive document contents, titles, entitlement links, and identity information from unauthorized access. | SRC-001 S06 | Must | Confirmed | High | High — authorization, minimization and protected stores | Q-007, Q-008 |
| NFR-002 | Non-functional | Diagnostic logs, metrics and traces contain no document content, document titles, personal information or credentials. | SRC-001 S06 | Must | Confirmed | High | High — telemetry filtering and bounded dimensions | None — exclusion explicit; implementation unverified |
| NFR-003 | Non-functional | Upload results, document access, grant changes and lifecycle actions have attributable, access-restricted audit records. | SRC-001 S06 | Must | Confirmed | High | High — durable audit and protected evidence | Q-009 |
| NFR-004 | Non-functional | Consider protected backups of authoritative content, metadata, grants, lifecycle state and audit evidence, and propose restore validation. | SRC-001 S07 | Must | Confirmed | High | High — coherent recovery and deletion reconciliation | Q-005, Q-006, Q-007 |
| NFR-005 | Non-functional | Employee and records-administrator browser journeys are accessible. | SRC-001 S08 | Must | Confirmed | High | Medium — presentation and acceptance tests | Q-010 |
| NFR-006 | Non-functional | Keep component responsibilities clear and support tested, compatible changes to the browser and data representation without obscuring access rules. | SRC-001 S08 | Must | Confirmed | High | High — module boundaries and change compatibility | ASM-001 |
| CON-001 | Constraint | Limit scope to one organization; exclude public sharing and external-tenant hosting. | SRC-001 S01, S09 | Must | Confirmed | High | High — scope and authorization boundaries | None — explicit exclusion |
| CON-002 | Constraint | Remain technology-neutral; platform mapping requires a later explicit request and approved neutral baseline. | SRC-001 S10 | Must | Confirmed | High | High — product-selection gate | None — explicit mapping constraint |
| CON-003 | Constraint | Exclude content editing, full-text search, OCR, automatic classification and AI extraction from the initial design. | SRC-001 S09 | Must | Confirmed | High | Medium — prevents speculative components | None — explicit exclusion |

## Actors and systems

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs |
| --- | --- | --- | --- | --- | --- |
| ACT-001 | Actor | Employee | Upload, list and retrieve authorized documents | SRC-001 S01; Confirmed | FR-001, FR-002, BR-001 |
| ACT-002 | Actor | Records administrator | Manage grants, dispositions and holds; content access still requires a grant | SRC-001 S02, S05; Confirmed | FR-003, FR-004, BR-004 |
| ACT-003 | Actor | Operations personnel | Health inspection and authorized recovery; no implied content browsing | SRC-001 S07; Confirmed | FR-006, NFR-004 |
| EXT-001 | External system | Corporate identity system | Existing authority for sign-in, not document grants; protocol and freshness unknown | SRC-001 S03; Confirmed existence, capabilities Unresolved | FR-005, BR-001, NFR-001 |

Stakeholders are the three supplied roles. No named business owner, approver, security team, legal authority, or operating team is supplied. Do not invent those assignments.

## Data inventory

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs |
| --- | --- | --- | --- | --- | --- |
| DATA-001 | Data entity | Document content | Authoritative uploaded payload; sensitive | SRC-001 S01, S04, S06; Confirmed | FR-001, FR-002, BR-002, NFR-001 |
| DATA-002 | Data entity | Document metadata and upload intent | Title/version association is source-backed; intent/publication bookkeeping Proposed | SRC-001 S04, S06; Confirmed entity, Proposed bookkeeping | FR-001, FR-002, BR-002, BR-003 |
| DATA-003 | Data entity | Document grants and principal links | Explicit document entitlements distinct from identity credentials | SRC-001 S02, S03, S06; Confirmed | FR-003, FR-005, BR-001, NFR-001 |
| DATA-004 | Data entity | Audit evidence | Attributable records of required actions; restricted access | SRC-001 S06; Confirmed need, record structure Proposed | NFR-003, NFR-004 |
| DATA-005 | Data entity | Diagnostic telemetry | Safe service outcomes, timing and health; exclude sensitive payloads | SRC-001 S06, S07; Confirmed need, fields Proposed | FR-006, NFR-002 |
| DATA-006 | Data entity | Lifecycle dispositions and hold state | Retention/hold decisions; deletion tombstone bookkeeping Proposed | SRC-001 S05; Confirmed entity, Proposed bookkeeping | FR-004, BR-004, NFR-004 |

Sensitivity is supplied for content/titles/identity/entitlement links; treating lifecycle and audit links as sensitive is a Proposed conservative handling rule, not a fabricated formal classification policy. Telemetry has a source-backed exclusion rule, not a guarantee that a deployed collector filters it. Organizational owners are Unassigned. Collection purposes are the linked capabilities. Retention/deletion rules are Unresolved under Q-001/Q-009; geographic constraints are Unresolved under Q-007. No real payloads should appear in generated artifacts.

## Integration inventory

| External system ID | Direction relative to subject | Business purpose | Data exchanged | Source or evidence class | Related requirement IDs | Open questions |
| --- | --- | --- | --- | --- | --- | --- |
| EXT-001 | Bidirectional — sign-in interaction and identity result | Authenticate supplied roles | Identity assertions/validation metadata; never copy actual tokens into design artifacts | SRC-001 S03; integration Confirmed, protocol Unresolved | FR-005, BR-001, NFR-001 | Q-008 |

No external downstream service is supplied. Do not add notification, scanning, search, analytics, or cloud services merely from convention. Internal interactions will be proposed by later design skills.

## Assumptions

| Assumption ID | Assumption | Reason | Consequence if false | Validation required | Status | Related IDs |
| --- | --- | --- | --- | --- | --- | --- |
| ASM-001 | A coordinated application release is acceptable for the provisional design | No independent-team or independent-release requirement is supplied | Reconsider module deployment and contract boundaries before implementation | Stakeholder confirmation; use service alternatives if independent releases are required | Proposed | NFR-006, CON-002 |
| ASM-002 | Prototype discussion can continue while irreversible deletion remains disabled pending an authorized lifecycle rule | Q-001 has no safe default policy | A real deployment may require different retention controls; do not use this premise to bypass law or approval | Resolve Q-001 before any irreversible deletion or production lifecycle approval | Proposed | FR-004, BR-004, NFR-004 |
| ASM-003 | Diagnostic telemetry failure may degrade diagnosis without stopping document access, while required audit capture has a separate durable path | Separates optional diagnosis from mandatory audit evidence without inventing SLOs | If policy requires another failure behavior, revisit access and audit availability trade-offs | Resolve Q-009 and validate bounded buffering/failure behavior | Proposed | FR-006, NFR-002, NFR-003 |

## Ambiguities and conflicts

No direct contradictory source requirements are present. "Accessible," identity freshness, and lifecycle authority lack acceptance detail; preserve those gaps. Missing numeric quality targets are not conflicting NFRs. A future conflicting source must retain both versions and user-supplied precedence before resolution.

## Clarification ledger

### Active batch — seven questions

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Critical | Which lifecycle disposition rule is approved for irreversible document deletion? | Retention and hold-release authority determine whether destruction is permitted | Supply approved disposition/hold rules; keep deletion undecided | None — blocked; ASM-002 permits prototype discussion only | Irreversible-deletion policy and production lifecycle readiness | Open | FR-004, BR-004, NFR-004 |
| Q-002 | Important | What workload profile must the upload and retrieval paths support? | Concurrency, payload shape and growth determine validation, storage and scaling pressure | Supply measured profile; supply planning scenarios; defer sizing | Unresolved — qualitative analysis only, no numeric capacity | Capacity qualification and resource sizing | Open | FR-001, FR-002, BR-002 |
| Q-003 | Important | What user-visible latency objective and measurement definition are required? | Success versus validation duration affects synchronous design feasibility | Supply operation/percentile/window; permit provisional flow analysis | Unresolved — no latency target | Performance qualification | Open | FR-001, FR-002, BR-002 |
| Q-004 | Important | What availability objective and measurement scope apply? | Redundancy and dependency availability need an agreed user-visible objective | Supply SLO and scope; defer availability selection | Unresolved — no percentage or measurement window | Deployment redundancy commitment | Open | FR-001, FR-002, FR-006 |
| Q-005 | Important | What recovery-time objective applies to the service? | Restore orchestration and standby alternatives depend on tolerable interruption | Supply RTO; defer recovery-time qualification | Unresolved — no restore-time guarantee | Recovery topology and restore-time acceptance | Open | FR-006, NFR-004 |
| Q-006 | Important | What recoverable-data-loss objective applies? | Backup/checkpoint consistency and replication choices depend on acceptable loss | Supply RPO; defer data-loss qualification | Unresolved — no data-loss target | Backup frequency and recovery-data acceptance | Open | BR-002, NFR-004 |
| Q-007 | Important | Which data-placement jurisdictions are permitted? | Content, audit, telemetry and backup placement can constrain topology | Supply permitted locations; defer placement | Unresolved — no region or cross-border copy selected | Physical placement and regional recovery approval | Open | NFR-001, NFR-004, CON-002 |

### Queued questions — not asked in the active batch

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-008 | Important | What identity-assertion and account-disable freshness contract does EXT-001 support? | Authentication validation and revocation behavior cannot be guessed | Supply authorized identity contract; defer protocol choice | Unresolved — fail closed when required identity/grant validation is unavailable | Identity integration acceptance | Open | FR-005, BR-001, NFR-001 |
| Q-009 | Important | What audit-evidence retention and outage-handling policy is approved? | Durable recording, buffer exhaustion, evidence access and retention affect service availability and privacy | Supply policy; defer operational acceptance | ASM-003 — separate diagnosis from audit capture; policy remains Unresolved | Audit lifecycle and failure-policy approval | Open | NFR-002, NFR-003, NFR-004 |
| Q-010 | Optional | Which accessibility acceptance criterion should be used? | Refines test evidence for the supplied accessible-journey requirement | Supply organizational criterion; review proposed keyboard/screen-reader checks | Unresolved — propose checks without claiming a conformance level | Formal accessibility assessment, not neutral component design | Open | NFR-005 |
| Q-011 | Important | Which authorized role owns incident and recovery decisions? | Runbooks and privileged restoration need supplied responsibility and authority | Supply approved assignment; defer operating handoff | Unresolved — owner Unassigned, no on-call policy invented | Operating handoff approval | Open | FR-006, NFR-004 |
| Q-012 | Important | Which initial document grants are created for a new upload? | Upload capability does not imply automatic content-reading rights; initial visibility cannot be invented | Supply initial-grant rule; require explicit grants through FR-003 | Unresolved — do not infer an automatic uploader read grant | Initial document-visibility policy, not the existing employee upload capability | Open | FR-001, FR-002, FR-003, BR-001 |

Q-009 describes one audit-policy decision, not an extra active batch. An evaluator can split its topics if separate authorities answer them; preserve IDs through explicit supersession. No unanswered Critical question is bypassed by a queued Important question.

## Architecture-driving gaps and next handoff

Capacity, latency, availability, RTO/RPO, placement, identity freshness, lifecycle/audit policy and operating ownership remain explicit gaps. Cost can only be discussed through qualitative drivers without workload/budget/placement evidence. The next skill must rank effects, not manufacture quantities, legal obligations, source authority, or a preferred provider.