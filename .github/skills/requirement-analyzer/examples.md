# Requirement Analyzer — Worked Examples

Original synthetic fixtures, not historical skill runs. All roles, sources, data, and IDs below are illustrative; no real people, confidential material, endpoints, policy authority, or approval is represented. Quantities belong only to the explicitly supplied fixture.
Use [./SKILL.md](./SKILL.md), [./references/common/requirement-schema.md](./references/common/requirement-schema.md), and [./references/common/architecture-principles.md](./references/common/architecture-principles.md). Evidence classes belong outside the exact nine-field requirement table. Routing follows [./references/orchestration.md](./references/orchestration.md).
Each example is a narrowly scoped worked excerpt or routing response, not a full HLD. Expected responses are specifications, not evidence that checks or skills executed.

## Example 1: Extract a synthetic prop-status brief

- User prompt: “Extract requirements, actors, data, upstream interactions, and the remaining availability question for this prop-status board. Do not design its components.”
- Supplied evidence and safe IDs: Design ID PROP-BOARD; baseline A. The authorized inline brief is SRC-001. Newly allocated illustrative records are defined in the tables below; no other registers are inherited.
- Source content, scope: Help a sandbox viewer inspect synthetic prop availability. The viewer may read the board, not modify the feed. External fixture feed supplies complete replacement snapshots and is the data authority. Booking, personal records, and other external systems are excluded.
- Source content, capabilities: Must display the current supplied snapshot; Must receive replacement snapshots from the fixture feed. No transport or technology is specified.
- Source content, latency: Should finish lookup at the 95th percentile within 450 ms at 15 lookups/s during a 12-minute synthetic exercise, measured by the mock viewer from request send to fully displayed response. These are supplied targets, not measurements.
- Source content, lifecycle: Must discard a superseded snapshot immediately on replacement. Classification is explicitly synthetic non-personal data; geographic restrictions and an availability objective are not supplied.
- Expected activation: Yes.
- Expected response: Provisional extraction; preserve the supplied quantities, separate the feed from the human reader, and leave availability unresolved. Independent qualitative driver analysis can proceed; availability-dependent choices cannot rely on a guessed target.
- Excerpt scope: Sources, requirements, entity/data/integration records, the active question, and handoff only. A full extraction also supplies the remaining ordered sections from the skill contract; this is not a full HLD.

### Sources

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | Synthetic prop-status brief | Inline Example 1: scope, capabilities, latency, lifecycle | User-authorized fixture input; no approval authority supplied | Supplied |

### Structured Requirements

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | Functional (FR) | Display the current supplied prop snapshot to the sandbox viewer. | SRC-001, capabilities | Must | Confirmed | High | Medium — read-path responsibility | None — capability and reader are explicit |
| FR-002 | Functional (FR) | Receive complete replacement snapshots from the fixture feed. | SRC-001, capabilities | Must | Confirmed | High | Medium — upstream data dependency | None — transport selection is outside extraction |
| NFR-001 | Non-functional (NFR) | Lookup completes at the 95th percentile within 450 ms at 15 lookups/s over a 12-minute synthetic exercise, measured from mock-viewer send to fully displayed response. | SRC-001, latency | Should | Confirmed | High | Medium — user-visible critical path | None — target definition supplied; achievement unverified |
| CON-001 | Constraint (CON) | Discard the superseded snapshot immediately when its replacement is received. | SRC-001, lifecycle | Must | Confirmed | High | High — retained-state lifecycle | None — lifecycle instruction is explicit |

### Actors and Systems

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs |
| --- | --- | --- | --- | --- | --- |
| ACT-001 | Actor | Sandbox viewer | Read the supplied snapshot; no feed-modification entitlement | SRC-001, scope; Confirmed | FR-001 |
| EXT-001 | External system | Fixture feed | Supply complete snapshots; external data authority | SRC-001, scope; Confirmed | FR-002 |

### Data Inventory

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs | Classification | Classification status | Owner, if supplied | Collection purpose | Retention/deletion state | Geographic constraints |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DATA-001 | Data entity | Prop snapshot | Current synthetic availability values | SRC-001, scope/lifecycle; Confirmed | FR-001, FR-002, CON-001 | Synthetic non-personal | Confirmed | EXT-001, supplied data authority | Display current prop availability | Discard superseded snapshot on replacement | Unresolved — no geographic restriction supplied |

### Integration Inventory

| External system ID | Direction relative to subject | Business purpose | Data exchanged | Source or evidence class | Related requirement IDs | Open questions |
| --- | --- | --- | --- | --- | --- | --- |
| EXT-001 | Upstream — feed supplies the board | Refresh visible availability | DATA-001 | SRC-001, scope/capabilities; Confirmed | FR-002 | None for business direction; protocol intentionally unspecified |

### Clarification Ledger

Active Batch; Queued Questions: None — no other clarification is requested in this bounded excerpt. Answered history: None supplied.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Important | What user-visible availability objective applies to snapshot lookup? | Defines acceptable interruption and its measurement boundary | Supply an objective with its measurement definition; leave objective undecided | Unresolved — no numeric availability objective | Availability-dependent driver conclusions, not extraction | Open | FR-001 |

### Handoff

- Design ID: PROP-BOARD.
- Contract version: 1.0.0.
- Artifact: requirement-analyzer — prop-status extraction excerpt.
- Artifact status: Provisional — availability remains unresolved; this excerpt does not claim full artifact completion.
- Baseline: A; SRC-001 inline synthetic brief.
- Evidence summary: Confirmed source statements and classification; Inferred architectural impacts; Assumed none; Proposed none; Unresolved availability and geographic restrictions.
- Changed IDs: Added SRC-001, FR-001, FR-002, NFR-001, CON-001, ACT-001, EXT-001, DATA-001, Q-001.
- Open questions: Q-001 Important; availability-dependent conclusions await the source owner's objective.
- Validation: Behavioral, runtime, parser, and specialist checks Not run; no measured target achievement or stakeholder approval supplied.
- Next handoff: architecture-driver-analyzer receives the full relevant registers and Q-001; obtain omitted extraction sections before representing this excerpt as a complete baseline.

Why: The brief contains attributable capabilities and an authorized reader. Extraction preserves source values without turning the feed into an invented protocol, vendor, or component.

## Example 2: Keep incompatible lifecycle instructions visible

- User prompt: “Reconcile the replacement rule with this retention addendum. Also clarify whether the output should call the view a board or a list; do not silently choose either lifecycle instruction.”
- Supplied evidence and safe IDs: Reuse all Example 1 records in PROP-BOARD, baseline B. SRC-002 adds “Must keep every superseded snapshot for 36 hours after replacement.” No source precedence is supplied. The board/list output label is explicitly undecided. Allocate CON-002, Q-002, Q-003; retain Q-001.
- Expected activation: Yes.
- Expected response: Blocked for lifecycle interpretation; retain both attributable requirements as Conflicted. Continue the unaffected actor and latency inventory. Request an authorized resolution, not a compromise duration.
- Excerpt scope: Changed source, requirement, data, and clarification records only, not a complete extraction or full HLD. Preserve DATA-001 classification and ownership while updating lifecycle evidence and requirement links.

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-002 | Synthetic retention addendum and terminology request | Inline Example 2 | User-authorized fixture input; precedence over SRC-001 not supplied | Supplied |

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| CON-001 | Constraint (CON) | Discard the superseded snapshot immediately when its replacement is received. | SRC-001, lifecycle | Must | Conflicted | High — explicit statement, unresolved authority | High — retained-state lifecycle | Q-002 |
| CON-002 | Constraint (CON) | Keep every superseded snapshot for 36 hours after replacement. | SRC-002, retention | Must | Conflicted | High — explicit competing statement | High — retained-state lifecycle | Q-002 |

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs | Classification | Classification status | Owner, if supplied | Collection purpose | Retention/deletion state | Geographic constraints |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DATA-001 | Data entity | Prop snapshot | Current synthetic availability values | SRC-001 scope/classification; SRC-001/SRC-002 competing lifecycle instructions | FR-001, FR-002, CON-001, CON-002 | Synthetic non-personal | Confirmed | EXT-001, supplied data authority | Display current prop availability | Conflicted — CON-001 versus CON-002; Q-002 | Unresolved — no geographic restriction supplied |

Active Batch below includes the inherited question; Queued Questions: None. Questions are ordered by priority, not ID. No assumption bypasses Q-002.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-002 | Critical | Which lifecycle instruction applies to superseded snapshots? | Immediate discard and continued retention cannot both govern the same snapshot | Authorized precedence decision; revised scope separating the instructions | None — blocked | Retention interpretation and dependent storage design | Open | CON-001, CON-002 |
| Q-001 | Important | What user-visible availability objective applies to snapshot lookup? | Defines acceptable interruption and its measurement boundary | Supply an objective with its measurement definition; leave objective undecided | Unresolved — no numeric availability objective | Availability-dependent driver conclusions, not extraction | Open | FR-001 |
| Q-003 | Optional | Should the output label this view board or list? | Settles the expressly requested presentation terminology only | Board; list; leave undecided | Unresolved — retain the neutral term view | None — wording only | Open | FR-001 |

Why: Recency is not authority. The active cross-skill batch stays below seven, keeps Critical first, and distinguishes source confidence from resolution status and question priority.

## Example 3: Route a diagram-only request

- User prompt: “Only draw the system context for the already-defined prop board, viewer, and fixture feed; do not re-extract requirements.”
- Supplied evidence and safe IDs: Reuse Example 1 SRC-001, ACT-001, EXT-001, DATA-001, FR-001, and FR-002 with their complete records; no new IDs, authority, or source access is implied.
- Expected activation: No.
- Expected response: “This is a system-context-diagram-generator request. Pass the registered viewer, feed, subject scope, and snapshot interaction to that owner. Keep the board as one subject box with no internals; report parser/render checks only if performed.”
- Excerpt scope: Routing response only, not an extraction artifact or full HLD.
- Why: The user requests a standalone view from established evidence. Re-extraction or component design would cross ownership; view rules come from [./references/common/diagram-guidelines.md](./references/common/diagram-guidelines.md).