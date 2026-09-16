# Architecture Style Selector — Worked Examples

Original synthetic test fixtures, not historical runs or accepted designs. All IDs, roles, files described as data, and source statements are illustrative; no real data, external capabilities, policy authority, or approval is supplied. Numeric objectives are absent unless explicitly introduced in fixture input.
Use [./SKILL.md](./SKILL.md), [./references/common/requirement-schema.md](./references/common/requirement-schema.md), and [./references/common/architecture-principles.md](./references/common/architecture-principles.md). Ownership follows [./references/orchestration.md](./references/orchestration.md). Every response is a narrowly scoped comparison excerpt or routing response, not a full HLD.

## Example 1: Compose a local stencil-validation exercise

- User prompt: “Compare all eleven styles for this synthetic stencil-validation exercise. Recommend a neutral composition only within the supplied exercise constraints; do not select products or allocate components.”
- Supplied evidence and safe IDs: Design ID STENCIL-EXERCISE, baseline A. SRC-001, requirements, and DRV records below are synthetic upstream input with complete fields, not a claimed prior run. DEC-001 and Q-001 are illustrative output IDs.
- Source content: The objective is to inspect validation outcomes for synthetic stencil batches. The sandbox operator may submit batches and inspect their reports. Must validate complete batches and report after each entire batch finishes; interactive results, continuous feeds, external integrations, production availability, and real data are excluded from this exercise.
- Source content: Must run the exercise in exactly one deployable process with coordinated releases. Domain rules Should be testable without file input/output. Input-batch size/profile and numerical service objectives are not supplied. Runtime products, data persistence, and organizational approval are not specified.
- Expected activation: Yes.
- Expected response: Provisional recommendation of a modular monolith with hexagonal/clean rule boundaries and batch processing, DEC-001 Proposed. Compare layered organization as a credible alternative; leave runtime selection and resource validation unresolved. This composition is not a production recommendation or proof of feasibility at an unspecified workload.
- Excerpt scope: Input registers, all-style evaluation, candidate/decision records, and question only. Full skill output also includes its envelope, decision-scope narrative, assumptions and reversal gates; this is not a full HLD.

### Supplied Sources, Requirements, and Drivers

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | Synthetic stencil-exercise baseline | Inline Example 1: capabilities, deployment, rule isolation | User-authorized fixture input; no approval authority | Supplied |

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | Functional (FR) | Validate complete synthetic stencil batches submitted by the sandbox operator. | SRC-001, capabilities | Must | Confirmed | High | Medium — bounded batch processing | None — capability supplied |
| FR-002 | Functional (FR) | Report validation outcomes after the entire submitted batch finishes. | SRC-001, capabilities | Must | Confirmed | High | Medium — completion semantics | None — deferred reporting supplied |
| CON-001 | Constraint (CON) | Execute the exercise in exactly one deployable process with coordinated releases. | SRC-001, deployment | Must | Confirmed | High | High — deployment boundary | None — exercise constraint supplied |
| NFR-001 | Non-functional (NFR) | Keep domain validation rules testable without file input/output. | SRC-001, rule isolation | Should | Confirmed | High | Medium — internal dependency direction | None — isolation goal supplied |

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-001 | Shared exercise deployment | CON-001 | Inferred | High — separate deployment would change an explicit scope constraint | Keep deployment topology compatible with the supplied process boundary | No runtime capability evidence | Check candidate deployment semantics against CON-001; validate runtime separately |
| DRV-002 | Rule/adapter separation | NFR-001 | Inferred | Medium — dependency refactoring is bounded but nontrivial | Isolate domain tests from file adapters without dictating deployment | No boundary test results | Change a fixture adapter while running domain tests without file access |
| DRV-003 | Whole-batch reporting | FR-001, FR-002 | Inferred | Medium — result timing and reruns affect the processing boundary | Make completion and partial-run outcomes explicit | Input-batch profile Unresolved | Validate whole-batch completion and failure outcomes using a supplied profile |

### All-Style Evaluation

Each row evaluates this fixture, not a generic ranking. Advantages for excluded styles are conditional benefits outside the current scope, not recommendations to add them.

| Style | Dimension | Applicability | Driver and requirement IDs | Evidence and assumptions | Advantages | Limitations | Operational complexity | Major risks | Select conditions | Avoid conditions | Validation gates |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Modular monolith | Deployment | Applicable — shared unit required | DRV-001, CON-001 | SRC-001 Confirmed constraint | Local module coordination | Shared release and failure scope | Shared release path plus module checks | Module boundaries may erode | Exercise unit remains fixed | Separate release becomes essential | Check deployable interpretation and module seams |
| Layered | Internal organization | Conditional — adapter direction must satisfy isolation | DRV-002, NFR-001 | Proposed alternative | Clear responsibility tiers | Ordinary layers may couple rules to file access | Dependency and compatibility checks | Passing file dependencies through domain code | Dependency direction keeps rules independent | Layers frustrate rule tests | Demonstrate file-free domain tests |
| Microservices | Deployment | Not applicable — separate units conflict with exercise constraint | DRV-001, CON-001 | SRC-001 Confirmed shared unit | Independent evolution if scope changes | Violates current release boundary | Separate release and recovery paths | Unjustified remote failure | Constraint is authoritatively revised | Shared deployable constraint holds | Verify scope before reconsidering |
| Event-driven | Interaction | Conditional — decoupling not required | DRV-003, FR-002 | SRC-001 requires final reports, not events | Decoupled reporting if later needed | Adds delivery semantics not demanded here | Replay and completion tracking | Duplicate or premature report effects | Explicit decoupled interaction need emerges | Direct completion suffices | Validate acknowledgement and rerun meaning before adding events |
| SOA | Integration | Not applicable — external capabilities excluded | DRV-003, FR-001 | SRC-001 scope exclusion | Reusable business contracts if scope expands | No integration capability to govern | Cross-owner contracts would add work | Broad unnecessary coupling | External capability reuse becomes evidenced | Exercise stays local | Reconfirm integration scope |
| Serverless | Runtime | Conditional — runtime constraints unverified | DRV-001, CON-001 | Runtime evidence Unresolved | Host-management effort may shift | Execution limits and state behavior unknown | Trigger, dependency, and recovery management | Unsupported runtime fit | Verified runtime fits the exercise unit | Runtime contradicts process semantics | Obtain runtime evidence; no product or cost claim |
| Hexagonal/clean | Internal organization | Applicable — file-independent rules requested | DRV-002, NFR-001 | SRC-001 isolation goal; effect Inferred | Testable domain behind adapters | Additional interfaces and indirection | Adapter and boundary tests | Abstraction without useful seams | Rules must remain independent of file access | Rules become trivial enough that indirection adds no value | Exercise adapter replacement and rule tests |
| Data-centric | Data emphasis | Conditional — lasting authority not supplied | DRV-003, FR-001 | Synthetic batches Confirmed; persistence Unresolved | Explicit validation/data meaning | Does not imply a shared database | Lineage and lifecycle work if persistence appears | Inventing a store or shared authority | Data lifecycle becomes a material driver | No durable authority is needed | Establish data ownership before persistence choices |
| Batch | Processing | Applicable — completed-batch report supplied | DRV-003, FR-001, FR-002 | SRC-001 Confirmed timing semantics | Explicit completion and rerun boundaries | No continuous intermediate results | Completion and partial-run handling | Reporting incomplete work as complete | Whole-batch results remain sufficient | Interactive deadlines replace the source meaning | Exercise success and interrupted-batch outcomes |
| Streaming | Processing | Not applicable — continuous results excluded | DRV-003, FR-002 | SRC-001 scope exclusion | Incremental results if later required | Unneeded event-time/state semantics | Lag and state recovery would add work | Extra state correctness burden | Continuous reaction becomes evidenced | Whole-batch completion remains sufficient | Revisit only with changed processing requirements |
| Hybrid | Cross-dimension composition | Applicable — named dimensions have distinct drivers | DRV-001, DRV-002, DRV-003, CON-001, NFR-001, FR-002 | Proposed composition | Aligns deployment, dependencies, and processing | Shared runtime still limits isolation | Joint lifecycle and boundary checks | Vague all-styles composition hides trade-offs | Every composed dimension has a driver | Added mechanisms lack a purpose | Check compatibility of the named composition |

### Candidate Compositions and Decision

| Candidate | Composed dimensions | Boundary and ownership implications | Related driver IDs | Evidence status | Trade-offs | Validation gates |
| --- | --- | --- | --- | --- | --- | --- |
| Modular monolith + hexagonal/clean + batch | Shared deployment; adapter-isolated rules; whole-batch processing; runtime unspecified | Logical rule, file-adapter, and reporting responsibilities inside the supplied deployable; no new CMP IDs or teams | DRV-001, DRV-002, DRV-003 | Proposed | Direct rule isolation versus interface overhead; shared failure/release scope | Boundary tests, interrupted-batch behavior, Q-001 resource evidence |
| Modular monolith + layered + batch | Same deployment/processing; layered internal organization | Responsibility tiers must keep file dependencies out of domain rules; ownership remains logical | DRV-001, DRV-002, DRV-003 | Proposed | Familiar tiers versus dependency leakage and cross-layer changes | Demonstrate equivalent file-free rule tests and completion behavior |

| Decision ID | Decision | Status | Rationale | Alternatives | Trade-offs | Related requirements | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DEC-001 | Recommend modular monolith + hexagonal/clean + batch for the bounded exercise | Proposed | Satisfies supplied deployment/processing constraints and directly expresses adapter-independent rules | Modular monolith + layered + batch if dependency tests support it; revisit separate units only after scope changes | Adapter indirection and shared lifecycle remain; no runtime or cost guarantee | FR-001, FR-002, CON-001, NFR-001 | Rule/adapter and completion tests; input profile Q-001; stakeholder validation not supplied |

Prefer the named composition only for the evidenced exercise. Reverse the recommendation if independent releases become required, whole-batch results no longer suffice, or adapter isolation fails; do not promote DEC-001 to Accepted without supplied authority evidence. Validation and experiments: Not run.

### Clarification Ledger

Active Batch below; Queued Questions and answered history: None supplied. Runtime/resource questions do not become invented target values.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Important | What input-batch profile should resource validation use? | Bounds a future exercise without claiming capacity for unknown inputs | Supply a representative synthetic profile; leave the profile undecided | Unresolved — no numeric size, throughput, or duration | Resource adequacy claims, not bounded style comparison | Open | FR-001, FR-002 |

Why: Deployment, internal organization, and processing solve different concerns. Comparing all styles does not require treating them as mutually exclusive or allocating HLD components.

## Example 2: Defer incompatible release boundaries

- User prompt: “Revisit the stencil composition: an equally authorized note now requires ingestion and reporting to be separately deployable. Keep the resource question and do not invent precedence.”
- Supplied evidence and safe IDs: Inherit Example 1 as STENCIL-EXERCISE baseline B. SRC-002, CON-002, and DRV-004 below are supplied synthetic upstream delta records. CON-001 retains its statement, source, priority and impact; its conflict status, confidence rationale and Q link are updated below. Allocate Q-002 and update DEC-001 without erasing its previous Proposed history or changing unaffected IDs.
- Expected activation: Yes.
- Expected response: Blocked for deployment choice; No supported winner. Re-evaluate all eleven styles, making deployment-sensitive applicability conditional on conflict resolution rather than carrying forward obsolete exclusions. Retain the rule-isolation comparison independently; do not choose a compromise topology.
- Excerpt scope: Conflict-linked records and active ledger only, not the full revised all-style matrix or a full HLD. The upstream owner must resolve requirement authority before dependent HLD selection proceeds.

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-002 | Synthetic separate-release instruction | Inline Example 2 | User-authorized fixture input; no precedence over SRC-001 | Supplied |

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| CON-001 | Constraint (CON) | Execute the exercise in exactly one deployable process with coordinated releases. | SRC-001, deployment | Must | Conflicted | High — explicit statement, unresolved authority | High — deployment boundary | Q-002 |
| CON-002 | Constraint (CON) | Execute ingestion and reporting as separate independently deployable units. | SRC-002, separate-release instruction | Must | Conflicted | High — explicit competing statement | High — deployment boundary | Q-002 |

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-004 | Independent release requirement | CON-002 | Inferred | High — split deployment changes release and failure boundaries | Requires separate deployability if source authority resolves in its favor | CON-001 conflict, Q-002; separate-unit operating evidence | Resolve authority through requirement owner, then reassess deployment/support effects |

| Decision ID | Decision | Status | Rationale | Alternatives | Trade-offs | Related requirements | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DEC-001 | Defer deployment composition pending the release-boundary conflict | Deferred | Supplied constraints cannot jointly define the same deployable units | Shared modular unit if CON-001 prevails; separately deployable boundaries if CON-002 prevails, subject to driver/operations rework | Shared lifecycle versus distributed contracts; neither alternative is approved | FR-001, FR-002, CON-001, CON-002, NFR-001 | Q-002 authority resolution; rerun affected drivers/styles and later component views |

Active Batch below retains the inherited question; Queued Questions: None. No ASM may replace the Critical source decision.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-002 | Critical | Which deployability instruction governs this exercise? | Shared and separate deployable units imply incompatible topology choices | Authorized precedence; revised scopes that separate the instructions | None — blocked | DEC-001 deployment choice and dependent HLD components | Open | CON-001, CON-002 |
| Q-001 | Important | What input-batch profile should resource validation use? | Bounds a future exercise without claiming capacity for unknown inputs | Supply a representative synthetic profile; leave the profile undecided | Unresolved — no numeric size, throughput, or duration | Resource adequacy claims, not bounded style comparison | Open | FR-001, FR-002 |

Why: The active batch stays below seven and keeps Critical first. Simplicity, recency, and a prior Proposed recommendation cannot resolve authority or turn an alternative into an Accepted decision.

## Example 3: Route a diagram-only request

- User prompt: “Only draw a context view for the stencil exercise and its sandbox operator; no style comparison is requested.”
- Supplied evidence and safe IDs: Example 1 SRC-001 supplies the subject, permitted operator actions, and absence of external integrations. No actor ID has been allocated in this fixture; the context owner must first establish its minimal written register, not invent internal components.
- Expected activation: No.
- Expected response: “Route to system-context-diagram-generator with the supplied subject and actor facts. Establish the written actor/interaction register, show the subject as one box with no internals, and disclose actual parser/render status.”
- Excerpt scope: Routing only, not a selection artifact or full HLD.
- Why: Standalone context generation belongs to its view owner under [./references/common/diagram-guidelines.md](./references/common/diagram-guidelines.md); a familiar domain does not authorize a style tournament.