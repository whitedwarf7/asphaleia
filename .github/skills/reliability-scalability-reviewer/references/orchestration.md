# Skill Orchestration

Contract version: 1.0.0. This is an explicit runbook, not a claim that Copilot automatically executes a deterministic pipeline. Invoke only skills relevant to the request. A complete HLD uses the sequence below; a focused review or diagram may start at its skill when the equivalent inputs are supplied.

## Execution sequence and ownership

1. `requirement-analyzer` owns source, requirement, actor, external-system, and initial data/assumption/question registers.
2. `architecture-driver-analyzer` owns architecture-driver ranking and missing quality scenarios.
3. `architecture-style-selector` owns the style comparison and style DEC recommendations.
4. `high-level-design-generator` owns the neutral baseline, component register, 28-section document, and traceability assembly.
5. `system-context-diagram-generator` owns the single-system context view and interaction explanation.
6. `container-diagram-generator` owns the container view and component/view reconciliation.
7. `data-flow-designer` owns critical scenarios and sequence diagrams.
8. `integration-designer` owns conceptual boundary contracts and failure responsibilities.
9. `security-architecture-reviewer` owns security findings, not requirements or risk acceptance.
10. `reliability-scalability-reviewer` owns reliability/scalability/performance findings, not invented SLOs.
11. `observability-operations-designer` owns signal, monitoring, runbook, incident and operating recommendations.
12. `technology-mapper` runs only if explicitly requested; it owns a separate Proposed mapping against an approved neutral baseline.
13. `architecture-decision-record-generator` owns ADR documentation, not independent architecture selection or approval.
14. `architecture-reviewer` owns final assessment and readiness recommendation, not certification or production authorization.

## Shared execution state

Carry the handoff envelope and full relevant registers, not only a prose summary. Preserve design ID, baseline/version when supplied, stable entity IDs, source links, evidence classes, changed records, blocked decisions, active clarification batch, deferred questions, decisions, findings, risks, and checks not run.

Ready means the producer's required checks succeeded for its stated scope. Provisional means useful work exists with explicit assumptions or missing evidence. Blocked means dependent work must stop; it is a truthful result, not a successful full artifact. Not applicable requires a scope-based rationale.

After stages 5–13, merge each owned output into the corresponding HLD sections and update traceability. This is assembly by the HLD owner, not a second unannounced decision process. Reconcile source/status/IDs before final review. A skeleton with future diagram handoffs may be a stage-4 Provisional artifact, but cannot be called a complete reviewed HLD.

## Transition contract

Every row also carries the shared state. Questions must follow the shared Critical/Important/Optional policy, with at most seven in the entire active batch. A material Critical unknown blocks only the dependent decision; assumptions never grant access, approve a platform, fabricate a target, or resolve a conflict.

| Transition | Information passed | Required completion conditions | Return to previous owner when | Ask the user when | Continue with assumption when |
| --- | --- | --- | --- | --- | --- |
| 1 → 2 | Structured FR/NFR/BR/CON, sources, actors, systems, data, constraints, ASM/Q records | Stable IDs and evidence/priority/status fields exist; missing requirements and conflicts are explicit | A requirement lacks source/meaning or duplicate IDs conflict | Objective, actor authority, or core scope cannot be interpreted responsibly | Workload detail can stay Unresolved and drivers can be ranked provisionally without selecting topology |
| 2 → 3 | Driver register, ranked effects, quality scenarios, unresolved targets | Impact rationale and related requirements exist; critical decision blockers are explicit | A driver invents a target or misses a confirmed constraint | Workload, consistency, recovery, residency, operational capacity or team boundaries would reverse style selection | Compare conditional alternatives with ASM/Q links; do not force a winner |
| 3 → 4 | Applicable-style matrix, preferred composition or alternatives, DEC records and validation gates | Advantages, limitations, complexity, risks, selection/rejection conditions recorded | Recommendation contradicts a driver or treats independent style dimensions as exclusive | Material drivers leave incompatible topology choices with no responsible provisional baseline | Create separate conceptual alternatives or a clearly reversible baseline marked Proposed |
| 4 → 5 | Context narrative, one subject, ACT/EXT/TB registers, scope and important interactions | System boundary and source-backed actors/dependencies identified | The context needs a new actor, external system, or changed scope | Subject ownership/boundary is missing or contradictory | Label nonessential interaction detail Proposed; do not invent external dependencies |
| 5 → 6 | Validated or explicitly unchecked context view plus baseline CMP/TB records | Context is one box, interactions labeled, prose agrees, parser status truthful | Context introduces undocumented components or obscures system ownership | A trust crossing changes the proposed decomposition materially | Keep protocol or nonessential placement unspecified with an explicit note |
| 6 → 7 | Container view, component responsibility map, trust boundaries, candidate flows | Diagram nodes reconcile with written CMP/EXT/ACT IDs; major components represented | A container has no responsibility/requirement or an important dependency is missing | Unknown deployment/ownership changes critical data movement | Preserve logical groupings as Proposed and select an evidenced scenario without inventing containers |
| 7 → 8 | FLW records, sequences, data classification, state transitions, error paths | Producers/consumers/stores and acknowledgement points clear or explicitly blocked | A flow adds a component, contradicts ownership, or changes a business rule | Ordering, atomicity, sensitive-data handling, retention or success semantics change correctness | Make noncritical transformation details Proposed; numeric timeouts remain Unresolved |
| 8 → 9 | INT contracts, auth boundaries, retry ownership, idempotency, schemas, failure/reconciliation | Every important boundary interaction accounted for or justified Not applicable | Contracts contradict flow semantics, external capabilities, or requirements | External authority or authentication/data handling cannot be safely assumed | Propose controls and protocol alternatives without asserting support by an external system |
| 9 → 10 | Security findings, severity rationale, residual risks, affected IDs, validation plan | Every required security area reviewed or justified; no certification claims | Mitigation changes topology/flow/requirements; send to owning stage and rerun affected security review | Entitlement, classification, exposure or legal authority prevents a safe design choice | Use a conservative Proposed control while keeping policy and risk acceptance unresolved |
| 10 → 11 | Reliability findings, capacity/failure scenarios, supplied objectives, missing targets | Failure, retry, scaling, recovery and bottleneck areas reviewed; target provenance retained | Findings require different data consistency, components, contracts or style | Recovery objectives, load envelope or degradation behavior materially changes architecture | Define provisional monitoring/recovery approach without inventing numeric SLOs |
| 11 → 12, requested only | Neutral baseline, operations model, reviews, DEC records, user mapping request and platform | Explicit mapping request and baseline approval evidence; requested platform or explicit provider-comparison authorization | Mapping needs a logical architecture change or reveals a missing driver | Platform choice, organization standard, location, product capability or baseline approval is missing and material | Product alternatives may stay Proposed only after the request/approval gate; never assume permission |
| 11 → 13, mapping skipped | Neutral consolidated design, decisions, reviews, operations, mapping Not requested record | Stage 12 skipped explicitly; major decisions identifiable and traceable | Operations introduced a new unreviewed component or decision | Major decision outcome or approval evidence is unclear | Write Proposed/Deferred ADRs without inferring approval, date, or product selection |
| 12 → 13 | Separate Proposed platform mapping, alternatives, capability evidence, cost/residency risks, unchanged logical IDs | Major selections have credible alternatives, trade-offs, status and validation; no silent logical changes | Service constraints invalidate baseline; route to driver/style/HLD owner and repeat relevant reviews | A product-dependent decision lacks acceptable evidence or explicit approval | Record Proposed or Deferred product decisions and dated evidence only if supplied |
| 13 → 14 | Complete HLD, traceability, diagrams, reviews, ASM/Q/RISK records, consistent DEC/ADR statuses | Required ADR fields and meaningful options/consequences/validation present; no orphan IDs | ADRs conflict with baseline decisions or hide unresolved choices | Claim of Accepted decision lacks approval evidence, or a material decision is unexplained | Retain Proposed/Deferred status and review readiness with explicit conditions |
| 14 → owning stage | Findings, gaps, coverage/consistency failures, affected record IDs and readiness conditions | Final matrix and all required assessment sections completed; checks not run disclosed | Any substantive gap needs requirements, drivers, style, design, views, contracts, reviews or ADR rework | A remaining Critical choice, policy/acceptance authority or contradictory requirement blocks readiness | Only bounded, visible noncritical gaps can support conditional stakeholder review |

## Rework and convergence

- Requirements changes return to stage 1; recompute affected drivers and decisions. Never silently rewrite the source baseline.
- Style changes return to stage 3 and invalidate affected components, diagrams, contracts, mapping, and ADRs.
- A new component must be added by the HLD owner before a diagram or mapping can use it; assign one stable CMP ID.
- Integration changes re-run affected data-flow, security, reliability, operations, and final checks.
- Platform constraints that change the logical design require a new neutral baseline and explicit approval before mapping resumes.
- Only the owner of an artifact changes its authoritative records. Other skills issue proposed deltas with affected IDs.
- Do not loop indefinitely on unchanged missing evidence. Return a precise Blocked/Provisional checkpoint with completed work, next required source/answer, affected IDs, and next skill.

## Clarification ledger example

A missing latency objective is not silently set to a conventional value. Record an Important question about the user-visible operation and measurement window. Default: qualitative critical-path analysis only; numeric objective Unresolved. If operation safety requires a deadline to choose a responsible architecture, classify that specific decision's question Critical and leave it blocked instead.

## Final handoff

Deliver the assembled HLD, optional mapping only if requested, ADRs, complete traceability, unresolved questions, risk/finding registers, diagram validation status, and final review. State which stakeholder gates remain and which tests have actually run. Do not describe the recommended sequence itself as proof that all skills executed.