# Reliability and scalability reviewer — worked examples

These are original, synthetic illustrations of [the skill](./SKILL.md), not executed load tests, failure experiments, restore drills, approvals, or evidence of objective attainment. All entities, data, and sources are fictional; no secrets, personal information, confidential content, or live systems are present. Fixture paragraphs are raw input facts, not abbreviated shared output registers. Confirmed means explicitly stated in that fixture. Each design has its own identifier namespace.

Apply the exact [shared contracts](./references/common/requirement-schema.md), [principles](./references/common/architecture-principles.md), [severity model](./references/common/severity-model.md), [checklist](./references/common/review-checklist.md), and [orchestration](./references/orchestration.md). [Sources](./references/SOURCES.md) supply conceptual provenance only. Use [the manual tests](./tests.md) for a separate evaluation. All numerical attempt counts below are explicitly supplied synthetic inputs; derived bounds are conditional arithmetic, never measured performance, service defaults, or chosen settings.

## Example 1: Lost acknowledgement and nested retries can repeat an effect

### User prompt

Review the retry and correctness behavior of SYN-REL-A. Use REL-101 for a finding if the written allocation invariant is broken. Explain aggregate attempt amplification, mitigation, residual risk, and validation without selecting technology or inventing an end-to-end deadline.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-101 | Synthetic allocation contract and retry behavior | fixture:SYN-REL-A/allocation | Synthetic user input; authorized for written correctness analysis only | Available inline |

ACT-101 is a simulated allocation requester. CMP-101 is a deployable Request coordinator; CMP-102 is a deployable Allocation adapter; CMP-103 is the Allocation store and owns DATA-101, generated inventory allocations with no personal content. DATA-101's purpose is simulated allocation; owner, lifecycle, and geographic constraints are unspecified. TB-101 is the caller-to-allocation authority boundary. Caller authorization is described; no broader security review result is supplied. FLW-101 is a logical allocation from ACT-101 through those components. INT-101 connects CMP-101 to CMP-102; INT-102 connects CMP-102 to CMP-103. Both are synchronous conceptual contracts, with no product or protocol selected.

SRC-101 explicitly supplies **2 total outer attempts per logical request** at CMP-101 and **3 total inner attempts per outer attempt** at CMP-102, including the initial attempt in both counts. These are toy input values, not recommended defaults. Every restarted outer attempt can repeat the complete inner loop. An acknowledgement may be lost after a durable store commit. CMP-103 explicitly treats repeated allocation submissions as fresh effects: there is no durable operation deduplication in the written contract. No end-to-end retry owner, deadline, timeout values, capacity measurements, RTO, RPO, or SLO is supplied. The request is limited to retry correctness; no sizing or placement conclusion is requested.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-101 | Functional (FR) | Allocate generated inventory for an authorized logical request | SRC-101, capability | Must | Confirmed | High — explicit fixture statement | High — allocation effect and response | None — capability is explicit |
| BR-101 | Business rule (BR) | A logical request must not cause a repeated allocation effect | SRC-101, correctness invariant | Must | Confirmed | High — explicit fixture statement | High — retry and commit boundary | None — invariant is explicit; implementation compliance is not established |

### Expected activation

Yes — the request concerns reliability and correctness under supplied retry/commit semantics.

### Expected response

**Scope excerpt — not a full review.** The exhibited target, finding, question, and traceability records keep every required field. Other reliability concerns and the full ordered artifact are not claimed complete by this excerpt.

- Design ID: SYN-REL-A.
- Contract version: 1.0.0.
- Artifact: reliability-scalability-reviewer — reliability and scalability review.
- Artifact status: Provisional — the written correctness defect is assessable; the end-to-end retry budget remains unresolved and no runtime validation exists.
- Baseline: SRC-101 at fixture:SYN-REL-A/allocation; version Unspecified.
- Evidence summary: Confirmed — supplied attempt counts, commit/acknowledgement behavior, absent deduplication, and BR-101; Inferred — a lost acknowledgement can permit a repeated effect and composed retries can amplify attempts; Assumed — None, the bound below is explicitly conditional; Proposed — durable idempotency and coordinated retry ownership; Unresolved — deadline, implementation results, and objectives outside this scope.
- Changed IDs: Add REL-101 and Q-101. Changes to INT-101/INT-102/CMP-102/CMP-103 remain owner-directed proposals, not authoritative edits.
- Open questions: Active Q-101 Important; no deferred or inherited questions supplied. It blocks quantitative end-to-end retry budgeting, not the supported correctness finding.
- Validation: This worked example has no executed checks or tools. The attempt-bound derivation is illustrative; fault, contract, recovery, and stakeholder mitigation checks are Not run — pending evaluation.
- Next handoff: observability-operations-designer receives REL-101, Q-101, attempt-bound uncertainty, safe outcome signals, and unchanged source/requirement/entity/flow facts. integration-designer owns the retry/idempotency delta; high-level-design-generator owns component changes and HLD assembly in sections 19, 20, 26, and 27. Rerun affected security, reliability, and operations reviews after incorporation.

**Scope and objective ledger — selected objective**

| Operation / related IDs | Target or objective | Value and units | Observer and window | Source / evidence class | Missing evidence / Q ID | Decision affected |
| --- | --- | --- | --- | --- | --- | --- |
| Allocation / FR-101, BR-101, FLW-101, INT-101, INT-102 | End-to-end deadline for a logical request | Unresolved — no time value or unit supplied | Unresolved — no objective observer/window supplied; CMP-101 is a Proposed observation boundary only | SRC-101 explicitly lacks the deadline; Unresolved objective, Proposed observer | Q-101 | Allocation of timeout/retry budgets and terminal handling; not the existence of the invariant defect |

**Failure and capacity analysis — correctness slice**

If every outer attempt invokes the full inner loop, the maximum store-attempt count for a logical request under the supplied loops is $A_{store} \leq A_{outer} \times A_{inner} = 2 \times 3 = 6$. Successful early completion reduces attempts. This is a conditional count bound, not measured load, a latency bound, or evidence that six writes succeed. Unmodeled clients or retry layers invalidate this scope's bound and require new evidence; no traffic rate or capacity is inferred from it.

The distinct correctness scenario is commit followed by lost acknowledgement followed by retry. Because the supplied store contract treats the retry as fresh work, a repeated effect is permitted even without exhausting the loops. Bounded attempts alone do not satisfy BR-101. No broker delivery feature would establish end-to-end exactly-once effects.

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REL-101 | High | The retry contract permits a repeated allocation after an unobserved durable commit | CMP-101, CMP-102, CMP-103; ACT-101, DATA-101, TB-101, FLW-101, INT-101, INT-102 | Finding type: Confirmed defect in the supplied design, not an observed production failure. Scenario: the store commits, its acknowledgement is lost, and a permitted retry creates another allocation for the same logical request. Evidence class and source: Confirmed contract behavior and BR-101 in SRC-101; failure consequence Inferred. Exposure and known controls: caller authorization and finite attempt loops exist, but neither deduplicates committed effects. Severity rationale: a demonstrably absent correctness control permits serious integrity harm to the core allocation invariant; the input does not establish catastrophic irreversible loss or workload-wide failure justifying Critical. Severity status: assessed for the written contract; runtime frequency and implementation behavior remain Unresolved. | Proposed: define a stable logical-operation identity with durable deduplication atomically coupled to the allocation effect in the existing authoritative store. Assign one accountable retry owner for each retry scope and bound aggregate behavior within a sourced end-to-end deadline. Compare a single retry layer with explicitly composed nested budgets; specify safe reconciliation for uncertain outcomes. Return contract/component changes to their owners, without claiming either option approved. | Identity reuse mistakes, deduplication/effect atomicity gaps, unavailable storage, later flows outside the deduplication boundary, and uncertain outcomes may remain. Retries can still consume resources, and no availability or latency objective is thereby met. | In an authorized sandbox, lose a response after commit, repeat the same logical operation, and verify that the durable allocation effect is not repeated and the result is stable or explicitly unresolved pending reconciliation. Observe aggregate attempts across both layers and terminal behavior within the agreed deadline once supplied. Validate crash/recovery atomicity and stakeholder mitigation agreement; no test is claimed run. | FR-101, BR-101 |

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-101 | Important | What end-to-end deadline definition is required for FLW-101? | Per-layer retries cannot be responsibly budgeted against an unknown user-visible deadline | Supply the required deadline and measurement definition; explicitly defer the objective | Numeric deadline remains Unresolved; continue qualitative correctness analysis only | Numeric timeout/retry budgeting and a claim of deadline satisfaction | Open | FR-101, BR-101 |

**Requirements traceability — all fixture requirements**

| Requirement ID | Component IDs | Decision IDs | Flow or integration IDs | Validation | Coverage status |
| --- | --- | --- | --- | --- | --- |
| FR-101 | CMP-101, CMP-102, CMP-103 | None — no decision record supplied | FLW-101, INT-101, INT-102 | Normal and uncertain-outcome allocation checks under REL-101, not run. The normal path exists, but failure behavior is deficient | Partial |
| BR-101 | CMP-102, CMP-103 | None — idempotency change is Proposed for the owners | FLW-101, INT-102 | Durable effect/deduplication atomicity and duplicate-request checks, not run. The written repeat behavior violates the invariant | Unaddressed |

No duplicate shared RISK is invented for REL-101. Missing owners remain Unassigned in subsequent risk/validation records. High findings require an agreed mitigation/validation plan before the final reviewer recommends detailed-design readiness; this response does not provide that agreement.

## Example 2: Recoverable reports without recovery objectives or restore evidence

### User prompt

For SYN-REL-B, review recovery of generated reports. The proposal mentions replication and says recovery should be quick. Does that establish recovery readiness? Keep absent objectives unresolved and give a useful conditional review without selecting more regions.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-201 | Synthetic report baseline and qualitative recovery statement | fixture:SYN-REL-B/recovery | Synthetic user input for written recovery review | Available inline |

CMP-201 is a deployable Report builder. CMP-202 is its Result store, owning DATA-201, generated public report output. EXT-201 is a synthetic retained input catalog, authoritative for DATA-202, immutable generated public seed data used to rebuild the reports. Owners, retention durations, and geographic constraints are unspecified; the fixture explicitly states seed retention for this rebuilding scenario but gives no organizational retention rule. FLW-201 rebuilds DATA-201 from DATA-202 through CMP-201 into CMP-202; INT-201 is the conceptual catalog read. A bounded set of reports can be rebuilt manually from the retained seed catalog, and their temporary absence does not destroy original data. Replication is described, but topology, restore dependencies, authorized restore access, and exercised rebuild/restore procedures are undocumented. No failed recovery drill or actual missing backup is evidenced. Availability, RTO, RPO, and load targets are explicitly not supplied. The request is recovery-only, not a capacity or topology selection task.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-201 | Functional (FR) | Rebuild generated reports from the retained immutable catalog | SRC-201, rebuild capability | Must | Confirmed | High — explicit fixture statement | Medium — recovery dependency and procedure | None — source capability is explicit; exercise evidence is missing |
| NFR-201 | Non-functional (NFR) | Recovery should be quick | SRC-201, recovery statement | Unspecified | Unresolved | Unknown — target and measurement definition are absent | Unknown — recovery choices depend on the missing objective | Q-201 |
| NFR-202 | Non-functional (NFR) | Recoverable report-data loss objective is not yet specified | SRC-201, explicit objective gap | Unspecified | Unresolved | Unknown — no allowed loss is supplied | Unknown — regeneration and backup requirements depend on the objective | Q-202 |

### Expected activation

Yes — the supplied baseline supports a qualitative recovery review but not an objective-satisfaction claim.

### Expected response

**Scope excerpt — not a full review.** This displays a complete finding and objective/question records, not an exhaustive reliability matrix or assembled HLD.

- Design ID: SYN-REL-B.
- Contract version: 1.0.0.
- Artifact: reliability-scalability-reviewer — reliability and scalability review.
- Artifact status: Provisional — recoverable operational exposure can be reviewed; target-dependent recovery conclusions are Blocked pending evidence and Q-201/Q-202.
- Baseline: SRC-201 at fixture:SYN-REL-B/recovery; version Unspecified.
- Evidence summary: Confirmed — regeneration capability, retained seeds, replication description, and explicit target gaps; Inferred — undocumented rebuild dependencies can delay restoration; Assumed — None, no outage duration or objective is assumed; Proposed — dependency/runbook review and authorized recovery exercise; Unresolved — actual restore controls, objectives, and timing.
- Changed IDs: Add REL-201, Q-201, and Q-202. Preserve NFR-201/NFR-202 as Unresolved; propose no authoritative topology change.
- Open questions: Active Q-201 and Q-202, both Important; no deferred questions supplied. Missing nonessential targets stay Unresolved rather than becoming numerical assumptions.
- Validation: No rebuild, restore, failover, failback, or performance test is evidenced. Planned checks are Not run — pending evaluation.
- Next handoff: observability-operations-designer receives the rebuild scenario, REL-201, source/objective records, questions, and required success evidence. requirement-analyzer obtains recovery definitions; high-level-design-generator retains unresolved recovery design in sections 20, 23, 26, and 27. Rerun affected review after objective or recovery-baseline changes.

| Operation / related IDs | Target or objective | Value and units | Observer and window | Source / evidence class | Missing evidence / Q ID | Decision affected |
| --- | --- | --- | --- | --- | --- | --- |
| Report recovery / FLW-201, NFR-201 | RTO | Unresolved — no duration or unit supplied | Unresolved — recovery start/end observation definition absent | SRC-201; Unresolved objective | Q-201 | Whether proposed rebuild/restore behavior meets required restoration time |
| Report recovery / DATA-201, DATA-202, NFR-202 | RPO | Unresolved — no permitted loss or unit supplied | Unresolved — recoverable-state reference point absent | SRC-201; Unresolved objective | Q-202 | Whether regeneration from retained seeds preserves the required recovery state |

| Concern | Result | Scenario and evidence | Affected IDs | Finding / risk links | Validation required |
| --- | --- | --- | --- | --- | --- |
| Restore dependencies and runbook evidence | Unresolved | SRC-201 documents a manual rebuild option but no exercised dependency/access sequence | CMP-201, CMP-202, EXT-201, DATA-201, DATA-202, FLW-201, INT-201 | REL-201 | Review authorized access and catalog/configuration availability, then exercise rebuild without relying on the failed store |
| Replication versus backup | Unresolved | SRC-201 describes replication only; this neither proves independent backups exist nor proves they are absent | CMP-202, DATA-201 | REL-201 | Obtain protected-copy and restore descriptions; assess need against the resolved objectives |

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REL-201 | Medium | Rebuild and restore dependencies are undocumented despite a stated regeneration workaround | CMP-201, CMP-202, EXT-201; DATA-201, DATA-202, FLW-201, INT-201 | Finding type: Missing evidence, not a confirmed absent backup or failed recovery. Scenario: an unavailable result store is followed by a stalled manual rebuild because dependency/access prerequisites are not established. Evidence class and source: Confirmed regeneration option and documentation gap in SRC-201; operational delay Inferred. Exposure and known controls: a bounded public report set is regenerable from retained immutable inputs; original data is not lost in the supplied scenario. Severity rationale: a recoverable operability weakness with a credible manual workaround supports Medium, not an assumed catastrophic loss or missed numeric target. Severity status: Provisional; dependency evidence or a changed consequence could lower or raise it. | Proposed: document and validate the independent seed/configuration/access dependencies and a safe rebuild/restore sequence; distinguish replication, backup, failover, restore, and failback evidence. Compare regeneration with protected-copy recovery after Q-201/Q-202 are answered. Do not add replicas or regions as an unsupported winner. | Rebuild duration, seed/catalog availability during a correlated failure, and restored-result correctness remain uncertain after documentation. A successful exercise would not guarantee every future recovery or establish the missing objectives. | Obtain authorized redacted dependency and protected-copy evidence. In a permitted sandbox make the result store unavailable, rebuild from retained seeds, and reconcile generated outputs against those seeds. Capture recovery observations for later comparison with the supplied RTO/RPO definitions; no pass against absent objectives is possible. | FR-201, NFR-201, NFR-202 |

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-201 | Important | What recovery-time objective definition applies to report availability? | Quick does not define an assessable recovery duration or observation boundary | Supply an authorized RTO definition; explicitly defer the objective | RTO remains Unresolved; continue qualitative dependency review only | RTO satisfaction and target-dependent recovery-option comparison | Open | NFR-201, FR-201 |
| Q-202 | Important | What recoverable-state objective applies to generated reports? | Regeneration must be compared with required state preservation, not an assumed loss tolerance | Supply an authorized RPO/state-loss definition; explicitly defer the objective | RPO remains Unresolved; no allowed loss is assumed | RPO satisfaction and the adequacy of regeneration versus protected copies | Open | NFR-202, FR-201 |

Carry the complete source and requirement records above unchanged. These are the entire active batch; no hidden or deferred batch is invented. A broader review with no credible impact or workaround evidence would use an exact shared risk with Unknown likelihood/impact and Unassessed severity instead of fabricating a classified finding. Here the bounded, regenerable scenario supports the provisional Medium finding. No risk acceptance, owner assignment, or recovery guarantee is made.

## Example 3: Requirement extraction is an upstream task

### User prompt

For SYN-REL-C, extract requirements only from the supplied operational note. Do not review reliability, pick a topology, or turn the word quick into a numeric target.

### Supplied evidence

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-301 | Synthetic stakeholder note: generated reports should recover quickly | fixture:SYN-REL-C/note | Synthetic user input for requirement extraction | Available inline |

No components, flows, workload, recovery definitions, supplied priorities, owners, or approval records exist in this fixture. The note concerns only generated public reports and includes no personal or confidential content.

### Expected activation

No — the requested output is source-grounded requirement extraction, owned by `requirement-analyzer`.

### Expected response

Route the note to `requirement-analyzer`. Preserve its source and the ambiguity of quickly; do not convert it into an SLO, RTO, RPO, replication count, or reliability conclusion.

- Design ID: SYN-REL-C.
- Contract version: 1.0.0.
- Artifact: reliability-scalability-reviewer — reliability and scalability review.
- Artifact status: Not applicable — only requirement extraction is requested.
- Baseline: SRC-301 at fixture:SYN-REL-C/note; no architecture version supplied.
- Evidence summary: Confirmed — the supplied qualitative note; Inferred — None; Assumed — None; Proposed — routing to the requirement owner; Unresolved — objective meaning, priority, and architecture context.
- Changed IDs: None — the reviewer creates no requirements, REL findings, or components.
- Open questions: None raised for routing; no inherited or deferred ledger supplied. The requirement owner should clarify material objective meaning without inheriting an invented answer.
- Validation: No reliability, capacity, failure, or recovery checks performed; downstream evaluation remains Not run — pending evaluation.
- Next handoff: requirement-analyzer receives the complete SRC-301 row and the unchanged qualitative note. Invoke reliability review only after a neutral baseline and assessable scope or explicit target gaps exist.

No finding is created to populate a non-activation response. This is a routing result, not a full review or evidence of reliability.