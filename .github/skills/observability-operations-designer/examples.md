# Observability and operations designer — worked examples

These original synthetic examples illustrate [the skill](./SKILL.md); they are not executed telemetry checks, incident actions, approved operating policies, or evidence of service performance. All sources, identifiers, and data are fictional. No actual secrets, personal information, confidential content, raw payloads, or live endpoints appear. Fixture prose is raw evidence, not a shortened shared output register. Confirmed means supplied in the fixture, never independently verified at runtime. Each design has a separate identifier namespace.

Apply the exact [shared contracts](./references/common/requirement-schema.md), [principles](./references/common/architecture-principles.md), [severity model](./references/common/severity-model.md), [checklist](./references/common/review-checklist.md), and [orchestration](./references/orchestration.md). [Sources](./references/SOURCES.md) explain conceptual provenance, not a mandate for a vendor, SLO, owner, or policy. [Manual tests](./tests.md) are separate evaluations. The objective values in Example 1 are explicitly synthetic user inputs, not defaults; no prices, thresholds, burn rates, sampling rates, or retention periods are inferred from them.

## Example 1: Preview SLI with a supplied objective and a gated runbook

### User prompt

Design neutral outcome measurement, diagnostic signals, health checks, and a proposed change/recovery runbook for SYN-OPS-A. Preserve the supplied preview objective exactly. Do not select a telemetry vendor, invent paging rules, or authorize anyone to operate the service.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-101 | Synthetic preview baseline, measurement definition, and operating gaps | fixture:SYN-OPS-A/preview | Synthetic user input authorized for written operating-plan design only | Available inline |

ACT-101 is a generated authorized caller. CMP-101 is a deployable Preview service; CMP-102 is its Layout store, owning DATA-101, generated public layout definitions without personal content. Purpose is preview generation; human owner, lifecycle policy, and geography are not supplied. TB-101 separates caller-supplied context from trusted service attribution. FLW-101 admits a preview request, reads its layout, and returns a preview or explicit failure; INT-101 is the synchronous service/store contract. Dependency failure prevents a successful preview but does not itself imply a stuck process. CMP-101 supports a reversible version change with a compatible read contract; execution authority and runbook owners are not supplied. There are no telemetry products or extra operational components in this baseline.

**Synthetic objective inputs, not defaults:** the source defines eligibility as each schema-valid, authorized preview attempt admitted at CMP-101, including admitted retries and cancellations. Requests rejected before admission and separately tagged health probes are excluded. A good event returns the expected preview response at the CMP-101 response boundary within **350 ms** of admission. The supplied objective is **at least 99% good events over a rolling 7-day window**. These values and that observer are fixture requirements, not production measurements or stakeholder approval evidence. Admission without observed completion is not a good event. Missing counters make completeness unknown rather than proving success. No client-network observation is supplied.

SRC-101 also declares the permitted bounded outcome classes good, late, failed, and unobserved, and a fixed preview operation name. Request content, authorization values, raw paths, caller/resource references, and job/request identifiers must not be metric dimensions. Opaque service-generated correlation may be proposed for access-controlled diagnostic logs/traces only; incoming context is untrusted. Diagnostic retention, sampling policy, alert thresholds/evaluation windows, routing, authorized operational access, and prices are not supplied. No signal, control, load, deployment, or restore test results are supplied. This example's scope excludes full restore/failback and cost-budget design; those need their own evidence, not fabricated settings.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-101 | Functional (FR) | Return a preview or explicit failure for each admitted attempt | SRC-101, journey | Must | Confirmed | High — explicit fixture text | Medium — terminal outcome observation | None — response contract is explicit |
| NFR-101 | Non-functional (NFR) | At least 99% of eligible admitted preview attempts return the expected preview at CMP-101 within 350 ms over a rolling 7-day window, using the supplied eligibility/exclusions | SRC-101, objective definition | Must | Confirmed | High — values, observer, and window supplied | High — measurement and change assessment | None for the objective; Q-101 for an alert policy, not a new SLO |
| CON-101 | Constraint (CON) | Exclude content, authorization values, raw paths, and caller/resource/request identifiers from metric dimensions | SRC-101, telemetry constraint | Must | Confirmed | High — explicit fixture constraint | High — telemetry privacy and cardinality | Q-103 for diagnostic lifecycle, not permission to ignore minimization |

### Expected activation

Yes — the request is neutral observability and operating-plan design against a supplied journey and objective.

### Expected response

**Scope excerpt — not a full operations design or review.** The exhibited SLI, signal, operating-activity, question, and handoff records retain all their exact required fields. Other journeys, full audit lifecycle, restore/failback, capacity, and cost plans are not represented as completed by this excerpt.

- Design ID: SYN-OPS-A.
- Contract version: 1.0.0.
- Artifact: observability-operations-designer — observability and operations proposal.
- Artifact status: Provisional — the measurement proposal is grounded; alert policy, routing, diagnostic lifecycle, and action authority remain unresolved. Writing a runbook does not authorize execution.
- Baseline: SRC-101 at fixture:SYN-OPS-A/preview; version Unspecified.
- Evidence summary: Confirmed — source journey, objective definition, outcome classes, privacy constraints, and reversible version-change capability; Inferred — outcome and dependency signals can distinguish service impairment from telemetry loss; Assumed — None, no objective/policy substituted; Proposed — instrumentation, health semantics, and runbook; Unresolved — Q-101 through Q-104, sampling policy, and runtime effectiveness.
- Changed IDs: Add Q-101 through Q-104 and the named proposed SLI/signal/runbook records below. No new CMP, approved policy, accepted decision, or authoritative contract change.
- Open questions: Active Q-104 Critical, then Q-101/Q-102/Q-103 Important; no deferred questions supplied. Action authority is Blocked; independent measurement design continues. Sampling remains an explicitly unresolved design choice pending safe validation, not an invented rate.
- Validation: No collection, alert, health, deployment, rollback, or incident test performed. Planned checks are Not run — pending evaluation; policy and execution gates remain stakeholder work.
- Next handoff: high-level-design-generator receives the neutral proposal and exact source/requirement/question records for section 21 and affected sections 22, 26, and 27. architecture-decision-record-generator receives proposed choices and mapping Not requested; no mapper execution is implied. New components, changed contracts, failure semantics, or privacy rules return to their owners for affected review reruns.

**Operating scope and objective ledger — prose scope statement**

The journey is FLW-101 preview at the supplied service-side observation boundary. NFR-101 is a supplied objective, not an observed success rate. Client delivery, full restoration, on-call staffing, and budgets are not evidenced. Q-104 prevents role-dependent actions, not drafting safe proposed steps. Carry the full source and requirement rows above unchanged.

**SLI and SLO plan**

| Journey / related IDs | SLI definition and eligible population | Observer / aggregation / missing-data behavior | SLO and window | Source / evidence class | Open question | Validation required |
| --- | --- | --- | --- | --- | --- | --- |
| Preview response / FR-101, NFR-101, FLW-101, CMP-101, INT-101 | Proposed indicator: good admitted preview attempts divided by all eligible admitted attempts. Good means expected response within the supplied 350 ms boundary; late, failed, cancelled, and unobserved admitted attempts are not good. Each admitted retry is a separate eligible attempt. Exclude only pre-admission rejects and separately tagged probes as SRC-101 specifies. | Observe admission and response at CMP-101, aggregate good/eligible counts across the supplied rolling 7-day window, not averages of per-instance percentages. Reconcile admissions and outcomes; unknown completions are not successes. Counter loss/discontinuity marks completeness Unresolved, and an empty denominator yields undefined, never perfect availability. This is not a client-observed SLI. | Supplied: at least 99% good over rolling 7 days; good boundary 350 ms. Preserve NFR-101 unchanged; no additional objective or error-budget policy. | SRC-101 / Confirmed objective and population; Proposed instrumentation; Unresolved runtime completeness | None for the objective definition; Q-101 governs separate alert policy, Q-102 routing, Q-104 authorized operational access | Use generated eligible/rejected/probe/retry/late/cancelled cases and missing-counter intervals; reconcile counts to the source population and demonstrate that missing/empty data cannot report success. Compare a complete aggregate only with NFR-101, without claiming a test has run. |

**Signal, health, dashboard, and alert plan — selected records**

| Signal / related IDs | Purpose and safe fields | Correlation and trust boundary | Health / dashboard use | Alert condition and routing | Privacy / cardinality / sampling / cost controls | Runbook link | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Preview outcomes / NFR-101, CON-101, CMP-101, FLW-101 | Proposed counts of eligible attempts and the source-declared good/late/failed/unobserved outcomes; fixed preview operation label | Aggregate at CMP-101; do not propagate caller context or attach request IDs as metric labels across TB-101 | Journey dashboard shows SLI, eligible count, and completeness separately; not a liveness probe | Proposed intent: investigate degraded outcomes or incomplete measurement. Numeric condition and evaluation window Unresolved under Q-101; routing Unassigned under Q-102; no paging commitment | Keep the supplied bounded dimensions; no raw path or hashed/unhashed caller/resource/request label. Sampling must not bias SLI denominators; diagnostic sampling is separate. Measure telemetry volume/overhead before cost conclusions; no budget or price invented | Preview change and recovery runbook, the operating record below | Compare aggregates with generated admitted attempts; simulate observation loss; verify no unbounded label dimension, false healthy display, or claimed alert delivery without configured routing |
| Preview diagnostics and causal traces / FR-101, CON-101, CMP-101, CMP-102, INT-101 | Proposed structured events: operation name, source outcome class, component identity, failure category; traces follow the existing service/store call, not payload content | Use newly generated opaque internal correlation only in access-controlled logs/traces; validate or replace incoming context at TB-101; correlation grants no access | Dependency/diagnosis view explains whether failure occurred before response or at INT-101; deployment version may be diagnostic context, not an unbounded metric label | Enrich the symptom investigation, not independent pages for every event. Routing remains Unassigned; no incident classification invented | Exclude payloads and authorization values; minimize diagnostic context; retention unresolved Q-103. Compare trace sampling with rare-failure bias; required audit events need a separate capture/lifecycle design, not silent sampling. Collection loss must not fail the preview request | Preview change and recovery runbook below | Check generated failure events contain only approved safe fields, incoming context cannot spoof trusted attribution, and telemetry unavailability remains visible without causing application failure |
| Preview process and dependency health / FR-101, CMP-101, CMP-102, INT-101 | Proposed startup-complete, local-progress, serving-eligibility, and dependency-outcome states; no content or caller identifiers | Internal states stay within the approved boundary; any future external probe is a proposed owner-directed change, not a new component here | Startup describes initialization; liveness describes local progress; readiness describes ability to serve safely. Dependency degradation is distinct from a stuck process. Journey probes remain separate from these states | Investigate a mismatch between readiness and observed outcomes; thresholds/window unresolved Q-101 and routing Unassigned Q-102. No dependency-driven restart automation | Bounded state categories; no per-request health labels, arbitrary probe payload, or invented interval. Validate health overhead and telemetry-loss behavior before adoption | Preview change and recovery runbook below | Simulate dependency failure and slow startup in an authorized sandbox; show no restart loop from dependency failure alone, no readiness before valid startup, and truthful end-user outcome visibility |

**Operating procedures — selected activity**

| Activity / related IDs | Trigger | Prerequisites and authorized access | Steps and stop conditions | Rollback / recovery and success evidence | Owner, if supplied | Validation required |
| --- | --- | --- | --- | --- | --- | --- |
| Preview change and recovery runbook / FR-101, NFR-101, CON-101, CMP-101, CMP-102, FLW-101; activity status Unresolved — policy/authority gates remain | Planned version change or investigation of degraded preview outcomes/incomplete telemetry; not an instruction to operate now | Authorized diagnostic access and any deployment/rollback role must be supplied under Q-104; compatible previous version and truthful observation coverage required. Q-101/Q-102 gate alert-trigger/routing automation | Proposed: compare pre-change outcome/completeness context; correlate version and INT-101 symptoms during change; inspect startup/readiness independently from liveness; verify generated preview results afterward. Stop if access is unauthorized, measurement is incomplete, compatibility is unknown, or an action would change data semantics. Do not automatically restart due solely to dependency failure; escalation destination remains unresolved | Proposed return to the previous compatible version only under supplied authority; otherwise retain a blocked-action checkpoint. Recovery evidence is correct previews, reconciled eligible/outcome counts, truthful readiness, and no new failure category; compare complete-window aggregates with NFR-101 when available, not a claim that a brief drill proves the rolling objective | Unassigned — no operational owner supplied | Authorized tabletop and sandbox rehearsal covering change, unavailable dependency, missing telemetry, and rollback compatibility. Require observable stop behavior without unauthorized changes. Status: Not run — pending evaluation; residual uncertainty includes future correlated failures and incomplete client-side visibility |

No OPS severity finding is manufactured merely because an operational proposal has open policy questions. The named records are proposals, not newly invented services. A full operations design must separately cover audit integrity/lifecycle, incident communication, restore/failback, capacity signals, and cost measurements or justify their scope exclusions; this excerpt does not mark them Covered.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-104 | Critical | Which authorized role, if any, may execute the preview change/recovery procedure? | A runbook cannot grant diagnostic or state-changing access | Supply authorized role/scope evidence; restrict the work to design/tabletop review; leave authority unresolved | None — blocked | Role-dependent operational steps, automation, and any execution | Open | FR-101, CON-101 |
| Q-101 | Important | What symptom-alert policy should govern this preview objective? | An SLO does not by itself supply a paging threshold or evaluation policy | Supply an authorized alert definition; request a later proposal for evaluation; leave it undecided | Alert condition/window remain Unresolved; dashboard measurement design only | Executable alert rules and claims of actionable alert configuration | Open | NFR-101 |
| Q-102 | Important | What supplied destination owns a preview symptom investigation? | An actionable notification needs an authorized receiving route | Supply a supported destination and ownership evidence; keep routing undecided | Routing and operational owner remain Unresolved/Unassigned | Notification delivery and escalation commitments | Open | FR-101, NFR-101 |
| Q-103 | Important | What diagnostic retention/deletion rule is authorized? | Useful correlation must not imply indefinite storage or a legal obligation | Supply an applicable lifecycle rule; request a minimization review; leave undecided | Diagnostic lifecycle remains Unresolved; do not authorize collection/retention by assumption | Stored diagnostic lifecycle configuration | Open | CON-101 |

The active batch is the stated four questions, Critical before Important, with no hidden questions or invented Optional detail. Remaining risk includes measurement loss, observer bias, unknown authorized response capacity, and rare failures missed by a future sampling choice. Proposed controls do not eliminate those risks or meet stakeholder gates.

## Example 2: Missing SLO and conflicting audit lifecycle rules

### User prompt

Design a conservative job-outcome indicator and audit lifecycle plan for SYN-OPS-B. There is no SLO, and the sources disagree about deleting the same job events. Preserve both rules and give useful independent work without inventing a retention period or approving deletion.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-201 | Synthetic publication baseline and withdrawal deletion rule | fixture:SYN-OPS-B/withdrawal | Synthetic user input; written plan authorized; precedence not supplied | Available inline |
| SRC-202 | Synthetic event-retention rule described as newer | fixture:SYN-OPS-B/retention | Synthetic user input of equal supplied authority, not an overriding approval | Available inline |

CMP-201 is a Publication worker; CMP-202 is its Journal owning DATA-201, generated job state, and DATA-202, generated per-job audit events classified restricted only inside this fixture. Purpose is resumable publication and attributable outcome history; human owners and geography are unspecified. TB-201 separates submitted job context from trusted internal attribution. FLW-201 durably accepts a validated job, publishes a result, and records a terminal outcome through INT-201. Duplicate committed effects are suppressed using internal journal identity; no such identity is approved as a metric label. The same DATA-202 events are covered by both lifecycle rules; withdrawal may occur before an authorized retention release. Neither source supplies authority to resolve that conflict. SLO, eligible-cohort observation window, RTO/RPO, alert policy, retention duration, staffing, and execution permission are explicitly absent. No legal obligation, runtime result, or compliance evidence is supplied.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-201 | Functional (FR) | Expose durable accepted-job and terminal outcome state without duplicate committed effects | SRC-201, job capability | Must | Confirmed | High — explicit fixture text | High — outcome measurement and reconciliation | Q-202/Q-203 for objective and cohort definition, not replacement of the capability |
| BR-201 | Business rule (BR) | Delete all per-job diagnostic and audit events when the job is withdrawn | SRC-201, withdrawal rule | Must | Conflicted | High — text explicit; precedence absent | High — audit lifecycle | Q-201 |
| CON-201 | Constraint (CON) | Retain the same per-job audit events until an authorized retention release | SRC-202, retention rule | Must | Conflicted | High — text explicit; precedence absent | High — incompatible lifecycle action | Q-201 |

### Expected activation

Yes — independent neutral SLI design can continue while the conflicting lifecycle action is blocked.

### Expected response

**Scope excerpt — not a full operations design or review.** These are complete displayed SLI/activity/question records, not an exhaustive operating model. Conflicting requirements remain authoritative input records, not rewritten policies.

- Design ID: SYN-OPS-B.
- Contract version: 1.0.0.
- Artifact: observability-operations-designer — observability and operations proposal.
- Artifact status: Provisional — a measurement proposal can be drafted; lifecycle action is Blocked by Q-201 and no SLO-based alert can be finalized.
- Baseline: SRC-201 and SRC-202 at their safe fixture locators; version Unspecified.
- Evidence summary: Confirmed — job states and both contradictory lifecycle texts; Inferred — deleting or retaining on the reviewer's choice could violate the other rule; Assumed — None, no policy or target replaced; Proposed — cohort-based outcome measurement and gated audit procedure; Unresolved — lifecycle precedence, objective, cohort window, and operating authority.
- Changed IDs: Add Q-201/Q-202/Q-203 and the named Proposed plan records. BR-201 and CON-201 stay Conflicted; no Accepted/Closed risk or authoritative component change.
- Open questions: Active Q-201 Critical, then Q-202/Q-203 Important; no deferred questions supplied. The unanswered lifecycle question blocks only dependent configuration/action.
- Validation: No measurement, deletion, retention release, audit integrity, or recovery check performed. Proposed evaluation is Not run — pending evaluation; authorized policy resolution and execution access remain required.
- Next handoff: requirement-analyzer receives Q-201 and both unchanged rules; security-architecture-reviewer receives privacy/audit implications. high-level-design-generator and architecture-decision-record-generator receive the neutral proposal and unresolved choices; mapping Not requested. Rerun affected lifecycle/security/operations checks after authorized source updates.

| Journey / related IDs | SLI definition and eligible population | Observer / aggregation / missing-data behavior | SLO and window | Source / evidence class | Open question | Validation required |
| --- | --- | --- | --- | --- | --- | --- |
| Publication outcomes / FR-201, CMP-201, CMP-202, DATA-201, FLW-201, INT-201 | Proposed completion indicator: successfully committed publication outcomes for a cohort divided by its distinct durably accepted jobs. Report failed and still-pending members separately; internal retry attempts must not inflate logical-job counts. Eligibility begins after durable validation/acceptance; cohort closure is not invented. | Proposed observer is CMP-202's durable state reconciled with CMP-201 outcomes. Window/cohort closure Unresolved under Q-203, so no numerical ratio is reported yet. Missing state/completion evidence makes completeness unknown; an empty denominator is undefined, and pending jobs are not automatically successes. Internal identity is for reconciliation, not a metric label. | Unresolved — no supplied SLO, time limit, or measurement window. A Proposed SLI is not an agreed objective, burn rate, or availability promise. | SRC-201 / Confirmed state semantics; Proposed measurement; Unresolved SLO/window. SRC-202 does not supply an objective. | Q-202 for an objective; Q-203 for the measurement definition; Q-201 only gates conflicting DATA-202 lifecycle actions | Use generated accepted/succeeded/failed/pending/retried cases to reconcile distinct job populations; simulate missing journal/telemetry evidence. Confirm no success is fabricated from absent data and no job/tenant identifier becomes a metric dimension. Compare to a target only after one is supplied. |

| Activity / related IDs | Trigger | Prerequisites and authorized access | Steps and stop conditions | Rollback / recovery and success evidence | Owner, if supplied | Validation required |
| --- | --- | --- | --- | --- | --- | --- |
| Audit lifecycle resolution and validation / BR-201, CON-201, CMP-202, DATA-202, FLW-201; activity status Unresolved — conflicting rules block action | Proposed lifecycle configuration review or a simulated withdrawal-before-release case; not a live deletion instruction | Authorized resolution of Q-201 and separately supplied execution/access authority. Existing source access permits written analysis only; no assumed retention, legal hold, or deletion power | Proposed: inventory the shared DATA-202 event category and both rules; document the withdrawal-before-release conflict; prepare generated-data cases for the resolved policy. Stop before configuring retention, deleting events, releasing a hold, or asserting legal applicability while conflict/authority is unresolved. Keep required audit capture separate from diagnostic trace sampling | Deletion may be irreversible; do not promise rollback. Before any authorized change, require a source-backed recovery/lifecycle plan consistent with the resolution. Success evidence is traceable event treatment matching the resolved rule across declared copies, no unauthorized exposure, and preserved required audit integrity; nothing has been performed | Unassigned — no lifecycle or incident owner supplied | Stakeholder policy resolution followed by an authorized generated-event lifecycle/integrity exercise. Show that unresolved rules block action, then validate the resolved rule without assuming sampled diagnostics satisfy required audit evidence. Status: Not run — pending evaluation; remaining copy/recovery obligations need supplied evidence |

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-201 | Critical | Which authorized rule governs DATA-202 when withdrawal precedes retention release? | Either action can violate a supplied rule; newer text grants no precedence | Supply authorized precedence or disjoint scopes; retain the conflict | None — blocked | Audit deletion/retention configuration and dependent validation | Open | BR-201, CON-201 |
| Q-202 | Important | What service objective, if any, is supplied for publication outcomes? | An indicator cannot create a target or paging policy | Supply an objective with its definition; confirm no objective is agreed | SLO remains Unresolved; propose measurement only | Objective-satisfaction and SLO-based alert claims | Open | FR-201 |
| Q-203 | Important | What cohort observation definition should measure accepted-job outcomes? | Pending, retried, and late-completing jobs need an explicit population/window treatment | Supply cohort/window/closure semantics; request alternatives for later validation; defer | Window and closure remain Unresolved; no numeric ratio or deadline claim | Computable time/cohort aggregation of the Proposed indicator | Open | FR-201 |

Carry the exact source and requirement rows above unchanged. Do not choose indefinite retention as a conservative default or assume immediate deletion is reversible. No OPS severity finding is needed merely to fill the output; an assessable material finding, if added in a full review, must use all nine shared fields with scenario-based severity, residual risk, and validation. No hidden questions, invented owners, legal conclusions, or policy acceptance are implied.

## Example 3: Monitoring-product selection is not operations design

### User prompt

For SYN-OPS-C, map the proposed monitoring responsibilities to Azure products only. Do not draft SLIs or runbooks. There is no approved neutral baseline or product-approval evidence.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-301 | Synthetic neutral monitoring responsibilities and explicit mapping request | fixture:SYN-OPS-C/mapping | Synthetic user input; no baseline approval supplied | Available inline |

CMP-301 is a Proposed Report worker; CMP-302 is its Proposed Journal, owning generated non-sensitive DATA-301. FLW-301 records job completion via INT-301. The request names Azure and asks for products for logs, metrics, and traces, but supplies no objective, budget, region, service capability evidence, accepted decision, or organizational standard.

### Expected activation

No — product selection belongs to `technology-mapper`, not the neutral operations-design skill.

### Expected response

Route to `technology-mapper`; **mapping is Blocked** pending neutral-baseline approval and the mapper's remaining evidence gates. Record it as explicitly requested, not Not requested. Do not select a backend, infer approval, or add a telemetry store/component under an operations assumption.

- Design ID: SYN-OPS-C.
- Contract version: 1.0.0.
- Artifact: observability-operations-designer — observability and operations proposal.
- Artifact status: Not applicable — only product mapping is requested.
- Baseline: SRC-301 at fixture:SYN-OPS-C/mapping; version Unspecified; approval absent.
- Evidence summary: Confirmed — requested mapping, named platform, and supplied responsibilities; Inferred — None; Assumed — None; Proposed — neutral CMP records remain Proposed; Unresolved — baseline approval, product evidence, objectives, cost, and placement constraints.
- Changed IDs: None — no SLI, OPS finding, accepted decision, component, or product assignment created.
- Open questions: No operations question batch; no inherited/deferred ledger supplied. Missing approval is passed to the mapping owner for the shared clarification ledger.
- Validation: No product, price, capability, parser, render, or operational checks performed; downstream evaluation remains Not run — pending evaluation.
- Next handoff: technology-mapper receives the unchanged SRC-301 and declared CMP-301/CMP-302/DATA-301/FLW-301/INT-301 facts, explicit request, platform, and approval gap. Only independent ADR work may proceed while mapping is blocked if its own evidence exists; no such work is claimed here.

No operating plan is fabricated for a non-trigger. Routing is not proof of operational readiness or a claim that another skill has executed.