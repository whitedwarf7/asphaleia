# Architecture Driver Analyzer — Worked Examples

These original synthetic fixtures are not historical analyses or executed tests. IDs are local to the declared design; no real users, allocation records, endpoints, organizational policies, or approval are implied. Only explicitly supplied quantities may appear as targets.
Apply [./SKILL.md](./SKILL.md), [./references/common/requirement-schema.md](./references/common/requirement-schema.md), and [./references/common/architecture-principles.md](./references/common/architecture-principles.md). Follow [./references/orchestration.md](./references/orchestration.md) for ownership. Each response below is a narrowly scoped excerpt or routing response, not a full HLD.

## Example 1: Separate architectural impact from business priority

- User prompt: “Rank the architectural effects of this synthetic tile-allocation baseline, including recovery. Do not choose an architecture style or change the supplied priorities.”
- Supplied evidence and safe IDs: Design ID TILE-LEDGER, baseline A. SRC-001 and the complete requirement rows below are authorized synthetic input, not outputs of a claimed prior skill run. DRV-001 through DRV-003 and Q-001 are newly allocated illustrative output records.
- Source content: A sandbox operator requests vacant synthetic tiles; competing requests may target the same tile. The objective is to track active allocations. The operator may request allocations and read labels, but no decision-approval authority is supplied. Real holders and external integrations are excluded.
- Source content: The supplied priorities intentionally make the single-holder invariant Could and uppercase display Must. Recovery is explicitly Should priority: make the last recoverable committed ledger readable within 25 minutes of the fixture's declared stoppage signal; the signal starts the clock and usable reads stop it. This is a target, not an observed result. Recoverable-loss tolerance is not supplied.
- Expected activation: Yes.
- Expected response: Provisional driver analysis: the invariant has High boundary impact despite Could priority; formatting is Low impact despite Must priority. Recovery has High impact, with a supplied time objective but unresolved loss tolerance. Do not infer topology, replication adequacy, or target achievement.
- Excerpt scope: Sources, requirements, drivers, material quality scenarios, dispositions, a question, and handoff. Full analysis also examines every concern in the skill matrix and supplies omitted sections with evidence or explicit gaps; this is not a full HLD.

### Supplied Sources and Requirements

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | Synthetic tile-allocation baseline | Inline Example 1: allocation, invariant, display, recovery | User-authorized fixture input; priorities supplied, no approval authority | Supplied |

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | Functional (FR) | Accept requests by the sandbox operator to allocate vacant synthetic tiles. | SRC-001, allocation | Must | Confirmed | High | High — allocation ownership | None — capability and requester supplied |
| BR-001 | Business rule (BR) | A tile has at most one accepted active holder, including under competing requests. | SRC-001, invariant | Could | Confirmed | High | High — correctness boundary | None — invariant supplied |
| FR-002 | Functional (FR) | Display the synthetic tile label in uppercase. | SRC-001, display | Must | Confirmed | High | Low — localized presentation | None — formatting meaning supplied |
| NFR-001 | Non-functional (NFR) | Restore usable reads of the last recoverable committed ledger within 25 minutes of the declared fixture stoppage signal. | SRC-001, recovery | Should | Confirmed | High | High — recovery sequencing and state | Q-001 for the separate, unspecified loss tolerance |

### Architecture Drivers

Evidence status Inferred describes the architectural effect, not a downgrade of the explicit source requirements.

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-001 | Single-holder allocation | FR-001, BR-001 | Inferred | High — changing the invariant or ownership boundary is costly to reverse | Keep allocation decision and accepted-holder state within an explicit correctness boundary | No implementation or contention evidence supplied | Exercise competing requests and inspect accepted-holder outcomes; later evaluate partition behavior per operation |
| DRV-002 | Uppercase label display | FR-002 | Inferred | Low — localized formatting is readily reversible | Keep presentation concerns from unnecessarily constraining state ownership | No implementation evidence supplied; source meaning sufficient for rank | Check rendering of synthetic labels without changing allocation semantics |
| DRV-003 | Recoverable ledger reads | NFR-001 | Inferred | High — recovery dependencies and durable state affect core service restoration | Identify recovery scope, sequencing, and independent restore evidence before choosing mechanisms | Loss tolerance Q-001; no recovery exercise evidence | Measure restore from the supplied stoppage signal to usable reads; establish recoverable-loss objective separately |

### Quality Scenarios

| Driver ID | Related requirement IDs | Source or evidence class | Stimulus and source | Environment and workload | Affected operation or boundary | Expected response | Measurement and supplied target | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-001 | FR-001, BR-001 | SRC-001 invariant Confirmed; scenario Proposed | Competing allocation requests from sandbox operators | Synthetic competition for the same tile; concurrency count Unresolved | Allocation decision and holder state | Preserve the single-holder invariant; do not report incompatible successful allocations | Accepted active holders per tile; at most one, as supplied in BR-001 | No execution evidence; distribution behavior not supplied | Inspect authoritative outcomes under contention and later failure scenarios |
| DRV-003 | NFR-001 | SRC-001 recovery Confirmed; scenario Proposed | Declared fixture stoppage signal | Synthetic stopped-ledger exercise; ledger size Unresolved | Restore path through usable ledger reads | Recover the last recoverable committed state and enable reads | Signal-to-usable-read interval at most 25 minutes; recoverable-loss target Unresolved, Q-001 | Restore dependencies, loss tolerance, exercise results | Record actual timing and recovered state during a future restore test; do not equate replication with backup |

### Requirement Disposition

| Requirement ID | Driver IDs | Disposition | Rationale |
| --- | --- | --- | --- |
| FR-001 | DRV-001 | Driver | Allocation establishes a correctness responsibility |
| BR-001 | DRV-001 | Driver | Invariant changes coordination regardless of business priority |
| FR-002 | DRV-002 | Low-impact driver | Explicitly retained rather than silently dropped |
| NFR-001 | DRV-003 | Driver | Recovery scope and sequence affect core state |

### Clarification Ledger

Active Batch below; Queued Questions: None in this excerpt. Answered history: None supplied. No ASM record is needed because no missing numeric objective is assumed.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Important | What recoverable ledger-loss tolerance is required? | Restoration time alone does not define acceptable data loss | Supply an objective with data scope; leave objective undecided | Unresolved — no numeric loss objective | Durability/recovery-mechanism conclusions, not qualitative recovery analysis | Open | NFR-001 |

### Handoff

- Design ID: TILE-LEDGER.
- Contract version: 1.0.0.
- Artifact: architecture-driver-analyzer — tile driver excerpt.
- Artifact status: Provisional — recoverable-loss evidence is missing and this is a bounded excerpt.
- Baseline: A; SRC-001 and supplied FR/BR/NFR records.
- Evidence summary: Confirmed requirements and time target; Inferred impact ranks/effects; Assumed none; Proposed quality exercises; Unresolved loss tolerance and exercise evidence.
- Changed IDs: Added DRV-001, DRV-002, DRV-003, Q-001; inherited source/requirement IDs preserved.
- Open questions: Q-001 Important; source owner to provide loss tolerance before durability conclusions.
- Validation: Behavioral, restore, contention, parser, and specialist checks Not run; no measured recovery or approval is claimed.
- Next handoff: architecture-style-selector receives registers, scenarios, dispositions, and gates after remaining concern coverage is supplied; no style winner is implied by this excerpt.

Why: Requirement priority orders desired outcomes; impact rank measures architectural consequence and reversibility. A supplied recovery target does not establish either a recovery mechanism or its success.

## Example 2: Leave “live” freshness impact Unknown

- User prompt: “Extend the tile analysis: the allocation eligibility check must use a ‘live’ view. Explain what cannot yet be ranked or selected.”
- Supplied evidence and safe IDs: Inherit Example 1 as TILE-LEDGER baseline B, including Q-001. SRC-002 and NFR-002 below are supplied synthetic upstream delta records, not requirements extracted by this skill. The source leaves “live” undefined and priority Unspecified; Q-002 is the reserved clarification link completed below. Allocate DRV-004 without changing source meaning.
- Expected activation: Yes.
- Expected response: Block the freshness-dependent conclusion, preserve authoritative-read and lag-tolerant interpretations, and retain DRV-004 as Unknown rather than Low. Continue the independent display and recovery analysis; return source meaning to requirement-analyzer and the source owner.
- Excerpt scope: New source, requirement, driver, and complete active ledger only; not a complete driver artifact or full HLD. No cache, event stream, protocol, or topology is selected.

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-002 | Synthetic live-view statement | Inline Example 2 | User-authorized fixture input; no definition or precedence supplied | Supplied |

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| NFR-002 | Non-functional (NFR) | Use a live allocation view for the eligibility check. | SRC-002, live-view statement | Unspecified | Unresolved | Unknown — freshness meaning missing | Unknown — read/correctness coupling depends on meaning | Q-002 |

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-004 | Eligibility-view freshness | NFR-002, BR-001 | Unresolved | Unknown — consequence cannot be ranked without operation semantics | May constrain whether an eligibility view can differ from authoritative holder state | Meaning of live; Q-002 | Obtain source-owner interpretation, then test its relation to the single-holder invariant |

Active Batch below retains Q-001; Queued Questions: None. The Critical default blocks only dependent conclusions; no surrogate numeric freshness target is introduced.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-002 | Critical | What does live mean at the allocation eligibility check? | A lagging view may not establish the state needed by the invariant | Authoritative state at decision; lag allowed only for informational display; revised operation meaning | None — blocked | Freshness/consistency driver conclusion and dependent style choice | Open | NFR-002, BR-001 |
| Q-001 | Important | What recoverable ledger-loss tolerance is required? | Restoration time alone does not define acceptable data loss | Supply an objective with data scope; leave objective undecided | Unresolved — no numeric loss objective | Durability/recovery-mechanism conclusions, not qualitative recovery analysis | Open | NFR-001 |

Why: Unknown impact, Unspecified business priority, and Critical question priority describe different things. The inherited cross-skill batch stays below seven without hiding the source-meaning blocker.

## Example 3: Route raw-source extraction rather than ranking

- User prompt: “Convert the raw tile-allocation notes into atomic requirements and actors; do not rank architecture drivers yet.”
- Supplied evidence and safe IDs: Only Example 1's authorized raw SRC-001 content is supplied for this request; its illustrated requirement/driver tables are not input. No new sources or output IDs are allocated by this routing example.
- Expected activation: No.
- Expected response: “Route the raw allocation, invariant, display, and recovery notes to requirement-analyzer. Preserve their priorities and the supplied recovery measurement definition; request the resulting source-linked baseline before driver ranking.”
- Excerpt scope: Routing only, not a driver artifact or full HLD.
- Why: Source extraction belongs upstream. An allocation topic does not by itself activate driver analysis when the explicit task is to create the requirements baseline.