# Shared Artifact Contracts

Contract version: 1.0.0. Use the exact field names below. Markdown tables or labeled records are valid; never put source code inside tables. Extra fields may extend a record but must not rename or remove required fields. Use stable IDs, explicit empty-state explanations, and safe source references.

## Handoff envelope

- Design ID: user-supplied identifier, or explicitly proposed identifier.
- Contract version: 1.0.0.
- Artifact: producing skill and artifact title.
- Artifact status: Ready, Provisional, Blocked, or Not applicable, with reason.
- Baseline: input artifact versions or safe source locators; Unspecified if absent.
- Evidence summary: Confirmed, Inferred, Assumed, Proposed, and Unresolved items.
- Changed IDs: additions, updates, supersessions; None when unchanged.
- Open questions: Q IDs, priority, blocked decisions, and outstanding answers.
- Validation: performed checks and tools, failures, checks not run, stakeholder gates.
- Next handoff: consumer, records passed, and any required upstream rework.

## Source register

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |

Authority records what the user supplied, not a guessed document hierarchy. A source may be public guidance, authorized user input, or a synthetic test fixture. Guidance is never automatically a business requirement. Safe locators should avoid confidential paths and identifiers.

## Structured requirements

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

- Category: Functional (FR), Non-functional (NFR), Business rule (BR), or Constraint (CON).
- Requirement statement: one attributable capability or constraint; preserve supplied quantities and units. Split composite statements and preserve source links.
- Source: SRC ID plus section/paragraph/record locator; no fabricated quotes.
- Priority: Must, Should, Could, or Unspecified. Use Unspecified if not supplied.
- Status: Confirmed, Inferred, Proposed, Conflicted, Unresolved, Rejected, or Superseded.
- Confidence: High, Medium, Low, or Unknown, with a short evidence rationale when not obvious.
- Architecture impact: High, Medium, Low, or Unknown plus the affected architectural concern. This is an analysis, not source priority.
- Assumption or clarification required: ASM/Q IDs or None with reason.

Inferences and assumptions must not become Confirmed through handoff. For an assumed detail, link its ASM record rather than silently extending a source-backed requirement. Preserve rejection/supersession and traceability history.

## Actors, systems, and data

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs |
| --- | --- | --- | --- | --- | --- |

Kind is Actor, External system, Data entity, Trust boundary, or Deployment element. For data entities add classification (Confirmed or Proposed), owner if supplied, collection purpose, retention/deletion state, and geographic constraints. A conservative handling recommendation is not an invented organizational classification.

## Architecture components

| Component ID | Component name | Responsibility | Inputs | Outputs | Dependencies | Data owned | Scaling considerations | Security considerations | Failure considerations | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

Identify proposed components as Proposed in a table caption or individual field. Use IDs for dependencies and owned data. Distinguish ownership from a copy, cache, or external source of truth. Record logical modules versus independently deployable containers explicitly.

## Architecture drivers

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |

Rank High, Medium, Low, or Unknown using consequence and reversibility, with rationale. Separate confirmed target values from unresolved measurement definitions. Do not create a numeric weighted score without supplied weights.

## Architecture decisions

| Decision ID | Decision | Status | Rationale | Alternatives | Trade-offs | Related requirements | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |

Status is Proposed, Accepted, Deferred, Rejected, or Superseded. Accepted requires named authority or supplied approval evidence recorded alongside the decision; do not invent an approver. ADRs reference these IDs and mirror their status. Dates and versions appear only when supplied.

## Risks

| Risk ID | Description | Likelihood | Impact | Severity | Mitigation | Owner, if supplied | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |

Likelihood: Unlikely, Possible, Likely, or Unknown. Impact: Low, Medium, High, Critical, or Unknown. Severity: Critical, High, Medium, Low, or Unassessed for a risk lacking assessable evidence. Use the severity model; do not invent numerical probabilities. Status: Open, Mitigation proposed, Mitigated pending validation, Accepted, or Closed. Acceptance/closure require supplied evidence. Owner defaults to Unassigned. Add affected IDs and related requirements to make each risk traceable.

## Assumptions and questions

| Assumption ID | Assumption | Reason | Consequence if false | Validation required | Status | Related IDs |
| --- | --- | --- | --- | --- | --- | --- |

Assumption status: Proposed, Validated, Invalidated, or Superseded. Only a supplied answer or evidence can validate an assumption.

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Question priority: Critical, Important, or Optional. Status: Open, Answered, or Superseded. Critical defaults to None — blocked and is reserved for the delivery-posture blockers; every other question carries the working assumption used in the meantime, or explicitly leaves a nonessential value Unresolved. Ask at most seven at a time.

## Flows and integrations

| Flow ID | Trigger | Producer | Consumers | Data and classification | Stores | Interaction mode | Validation and transformation | Success and failure paths | Retention and deletion | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| Integration ID | Participants | Purpose | Style | Contract and schema evolution | Authentication and authorization | Timeout and retry ownership | Idempotency and ordering | Rate limits and backpressure | Failure, dead-letter, and reconciliation | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

Interaction mode is Synchronous, Asynchronous, Batch, or Mixed; state proposed versus confirmed. Fields that do not apply need a reason, not a fabricated queue or protocol. Numeric limits and policy-dependent settings remain Unresolved unless supplied.

## Findings

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Every finding uses Critical, High, Medium, or Low, with a scenario and severity rationale; mark provisional severity when evidence is incomplete. Distinguish a confirmed defect from missing evidence. Reference a shared RISK entry when needed rather than duplicating the same risk across reviews.

## Requirements traceability

| Requirement ID | Component IDs | Decision IDs | Flow or integration IDs | Validation | Coverage status |
| --- | --- | --- | --- | --- | --- |

Coverage status: Covered, Partial, Unaddressed, or Out of scope. Covered means covered by the proposal, not verified in production. Every requirement must have a row; map to components/decisions or explicitly explain the gap and link a Q/DEC record. Trace backward too: every important component and decision needs a requirement, assumption, or justified risk. No orphan IDs or references to deleted components.