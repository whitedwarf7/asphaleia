# Architecture Style Selection — Asphaleia

## Handoff

- Design ID: ASPHALEIA.
- Contract version: 1.0.0.
- Artifact: architecture-style-selector — all-style evaluation, composition recommendation and style decisions.
- Artifact status: Ready for the stated scope — one composition passes the evidence gate; two dependent choices (DEC-011, DEC-012) are Deferred with explicit gates rather than hidden. Status is a handoff description, not approval.
- Baseline: [01-requirements-baseline.md](01-requirements-baseline.md), [02-architecture-drivers.md](02-architecture-drivers.md).
- Evidence summary: Confirmed — CON-001 (installable package), BR-001; Inferred — applicability judgments and composition effects; Assumed — ASM-001–ASM-008 inherited; Proposed — DEC-001–DEC-004; Unresolved — threat-model boundary (Q-001), latency target (Q-004), detector packaging (Q-007).
- Changed IDs: added DEC-001, DEC-002, DEC-003, DEC-004 (Proposed), DEC-011, DEC-012 (Deferred). DEC-005–DEC-010 and DEC-013–DEC-014 are reserved for the HLD owner.
- Open questions: active batch unchanged, Q-001–Q-007; queued Q-008–Q-014.
- Validation: qualitative, consequence-based comparison; no scoring rubric or weights supplied, none fabricated; no benchmark or prototype run.
- Next handoff: `high-level-design-generator` ([04-high-level-design.md](04-high-level-design.md)) receives the matrix, composition, DEC records and inherited registers.

## Decision Scope and Driver Baseline

- Decision boundaries: how the package is delivered and deployed (topology), how it is organized internally, how enforcement and telemetry interact, how offline work is processed, and whether a second deployment form is needed.
- Composable dimensions: deployment topology; internal organization; interaction; processing; runtime; data-management emphasis.
- Relevant drivers: DRV-001–DRV-013; material: DRV-001, DRV-002, DRV-003, DRV-004, DRV-005, DRV-007, DRV-008.
- Confirmed constraints: CON-001 installable Python package. Inferred constraints: CON-002 neutrality, CON-003 cooperative boundary, CON-004 scope, CON-005 no reliance on provider behavior.
- Preferences: none supplied. Existing state: none (greenfield).
- Unknowns: Q-001 (host compromise in threat model), Q-004 (latency), Q-007 (detector packaging), Q-012 (multi-process state).
- Source authority: requesting user (SRC-001); no approval authority stated.

## All-Style Evaluation

| Style | Dimension | Applicability | Driver and requirement IDs | Evidence and assumptions | Advantages | Limitations | Operational complexity | Major risks | Select conditions | Avoid conditions | Validation gates |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Modular monolith | Deployment and modular boundaries | Applicable — the deliverable is one installable package whose guards are modules | DRV-002, DRV-005, DRV-007; CON-001, FR-020 | CON-001 Confirmed; module map Proposed | One release unit; enforceable module ownership through package structure; synchronous local decisions; no distributed contracts | Shares the host's failure and latency unit; module erosion can couple guards to adapters | Low — library release and semantic versioning | RISK-001 partial integration; RISK-006 adapter drift | CON-001 holds | Host-compromise defense is required (Q-001), which needs a second form (Hybrid) | Import-graph check that the core imports no adapter or framework module |
| Layered | Internal organization | Applicable — as dependency direction inside the package: adapters, then application hooks, then domain | DRV-002, DRV-003; NFR-007 | Inferred organization | Predictable dependency direction; domain isolated from framework types | Pass-through layers add little alone; overlaps with hexagonal | None at runtime | Framework types leaking into the domain | Used together with hexagonal ports | Used as the only organizing principle | Dependency lint in the build |
| Microservices | Deployment and service ownership | Not applicable — no independent release or scaling units; a network hop per decision contradicts NFR-001 and "install and attach" | DRV-002, DRV-006; CON-001 | CON-001 Confirmed | None for this scope | Distributed failure and latency on every decision | High | Not evaluated further | Never for the library scope | CON-001 | None |
| Event-driven | Interaction | Conditional — only for in-process audit emission and detector signals; enforcement decisions stay synchronous | DRV-009, DRV-003; FR-017, BR-001 | FR-017 emission need Inferred; BR-001 Confirmed | Sinks decoupled from the hot path; non-blocking audit | Asynchronous emission risks event loss and ordering issues; must never carry decisions | Low to medium — bounded buffer, drop or deny semantics | RISK-010; event loss when sink unavailable | Audit and signal emission only | Any enforcement decision | Sink-unavailable and buffer-overflow tests |
| SOA | Service integration and capability boundaries | Not applicable — no governed set of heterogeneous enterprise services; integration is through code-level ports | DRV-002; CON-001, CON-002 | Inferred | None | Contract governance overhead without a service estate | High | Not evaluated further | Never for this scope | CON-001 | None |
| Serverless | Runtime and execution model | Not applicable — the runtime belongs to the host; the library must run wherever the host runs and selects no runtime itself | NFR-005; ASM-008 | Inferred | None | Serverless hosts are stateless, which affects local state (ASM-008) | Not owned | Local state loss on stateless hosts | Never as a package choice | Always for the package; note host effect | State-store port supports host-supplied stores |
| Hexagonal/clean | Internal dependency organization | Applicable — domain (provenance, policy evaluation, broker) independent of frameworks; ports for detectors, secrets, sinks, approvals, retrieval, sandbox | DRV-002, DRV-003, DRV-004; FR-020, NFR-006, NFR-007, CON-002 | FR-020 and NFR-007 Inferred | Testable domain without frameworks; adapters swappable; vendor neutrality | Indirection; adapter fidelity must be tested per framework version | Low runtime; more contracts to test | RISK-006 | CON-002 and FR-020 hold | Only if a single framework were mandated, which is not evidenced | Conformance suite per port and adapter |
| Data-centric | Data-management emphasis | Conditional — provenance-labeled context and decision records are the central domain model, but the package owns no shared database and no host data authority | DRV-001, DRV-008; FR-001, FR-012 | FR-001 Inferred; ACL truth external (DEC-007) | Explicit label model and lineage of content through the session | Package must not become the authority for host entitlements | Low | RISK-008 | As an emphasis for the domain model only | Any shared-store authority over host data | Label propagation and negative retrieval tests |
| Batch | Processing | Conditional — scanner and dry-run replay run offline over configuration and recorded sessions | DRV-012; FR-019, FR-022 | Inferred | No hot-path cost; reproducible reports | Findings stale between runs | Low | None material | For scanner and replay | For enforcement | Dry-run parity test |
| Streaming | Processing | Not applicable — audit produces discrete events; continuous analytics belong to the host's sink (EXT-006) | DRV-009 | Inferred | None inside the library | Unbounded state and lag management inside a library | Medium | Not evaluated further | Never inside the package | Always | None |
| Hybrid | Explicit composition across dimensions/boundaries | Conditional — library plus an optional out-of-process strict-mode component (credential broker or gateway) if host compromise enters the threat model | DRV-004, DRV-005; CON-003, FR-009 | SRC-006 shows both forms in practice; Q-001 Unresolved | Defense in depth; credentials and policy leave the host process | Two deployment forms; duplicated policy evaluation; divergence between modes | Medium to high | Mode divergence; operational burden | Q-001 answered "included" | Without threat-model evidence | Q-001 answer; prototype of the strict-mode contract |

## Candidate Compositions

| Candidate | Composed dimensions | Boundary and ownership implications | Related driver IDs | Evidence status | Trade-offs | Validation gates |
| --- | --- | --- | --- | --- | --- | --- |
| A — In-process guarded library | Modular monolith package; hexagonal core with layered dependency direction; synchronous inline enforcement; in-process event emission for audit; batch CLI | One distributable; domain modules own provenance, policy, broker; adapters own framework binding; local state through a store port; host owns identity and ACL truth | DRV-001–DRV-013 | Proposed; supported by CON-001, DRV-002, DRV-003 | Simplest install; cooperative boundary remains (CON-003); cannot defend against host compromise | Import-graph, conformance and fault-injection tests |
| B — A plus optional strict-mode sidecar | Candidate A plus a Hybrid out-of-process component holding credentials and optionally evaluating policy for egress | Credentials never present in the host process; policy evaluated twice or delegated; new network contract and failure mode | DRV-004, DRV-005 | Deferred pending Q-001 | Stronger isolation; higher operational cost; mode divergence risk | Q-001; strict-mode contract prototype |
| C — Gateway only (no library) | Out-of-process proxy between host and providers/tools; no in-process code | Sees only network traffic; cannot label provenance inside the host, guard retrieval, gate memory writes or bind approvals to in-process context | DRV-001, DRV-008 | Rejected for this scope | Contradicts CON-001 and loses the in-process control points that make rows 2, 8, 12, 15 addressable | Revisit only if CON-001 is withdrawn |

## Recommendation or Deferred Choice

- Outcome: Recommend Candidate A.
- Evidence sufficiency: material drivers (DRV-001, DRV-002, DRV-003, DRV-004, DRV-007, DRV-008) are understood; CON-001 is Confirmed; no Critical question exists; alternatives B and C have documented trade-offs.
- Preferred composition: modular monolith package, hexagonal core with layered dependency direction, synchronous inline enforcement, in-process event emission for audit only, batch CLI for scanning and dry-run replay.
- Alternatives retained: B (Deferred, DEC-011); C (Rejected with revival condition).
- Reasons to select: matches CON-001 and CON-002; keeps decisions synchronous and deterministic (BR-001, NFR-003); places every control at an in-process point identified in the concern isolation; lowest operational cost.
- Reasons to avoid: if Q-001 includes host compromise, A alone under-protects credentials; B becomes necessary.
- Blocked decisions: none. Deferred: DEC-011 (strict mode), DEC-012 (detector placement and packaging).
- Conditions that reverse the outcome: CON-001 withdrawn (revives C); Q-001 answered "included" (promotes B); a mandated single framework (simplifies hexagonal ports but does not change topology).

## Architecture Decisions

| Decision ID | Decision | Status | Rationale | Alternatives | Trade-offs | Related requirements | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DEC-001 | Deliver Asphaleia as a single installable in-process Python package (modular monolith library) | Proposed | CON-001 is Confirmed; in-process placement is what makes the addressable concerns addressable (SRC-007 §1) | B sidecar hybrid; C gateway only | Cooperative boundary (CON-003) is accepted as a documented limitation, not solved | CON-001, CON-003, FR-020 | Import-graph check; conformance check for unguarded call sites |
| DEC-002 | Organize internally as a hexagonal core (provenance, policy, broker, guards) with ports and adapters, and a layered dependency direction from adapters to hooks to domain | Proposed | DRV-002 neutrality and DRV-003 testability; NFR-007 names the ports | Framework-specific core; plugin-only architecture | Indirection and per-adapter conformance testing | FR-020, NFR-006, NFR-007, CON-002 | Dependency lint; per-port conformance suite |
| DEC-003 | Enforcement decisions are synchronous and inline; audit events and detector signals are emitted through in-process hooks that never carry decisions | Proposed | BR-001 and NFR-003 require deterministic synchronous decisions; audit should not block the hot path | Fully synchronous audit; asynchronous decisions | Bounded buffer with explicit drop or deny semantics for events | BR-001, NFR-003, FR-017 | Fault injection; sink-unavailable tests |
| DEC-004 | Scanner and dry-run replay run as offline batch commands | Proposed | DRV-012; no hot-path cost | Continuous in-process scanning | Findings stale between runs | FR-019, FR-022 | Dry-run parity test |
| DEC-011 | Add an out-of-process strict-mode component (credential broker or gateway) | Deferred | Depends on Q-001; SRC-006 shows both forms in practice | None (library only); full gateway | Two forms, divergence risk, operational cost | CON-003, FR-009 | Q-001 answer; contract prototype |
| DEC-012 | Placement and packaging of injection detectors (inline structural only; asynchronous ML; optional extra) | Deferred | Depends on latency target (Q-004) and dependency tolerance (Q-007) | Bundle ML detector; no ML detector | Signal quality versus latency and footprint | FR-011, NFR-001 | Benchmark and signal-quality comparison |

## Assumptions

Inherited unchanged: ASM-001–ASM-008 ([01-requirements-baseline.md](01-requirements-baseline.md)). Reversible fallback for the composition: if ASM-002 is invalidated by Q-001, Candidate B is adopted without changing the core modules of A.

## Clarification Ledger

Inherited unchanged: active batch Q-001–Q-007; queued Q-008–Q-014. No new question.

## Validation and Reversal Gates

| Decision ID | Evidence or experiment required | Expected observation | Reversal condition | Responsible authority, if supplied | Check status | Next owner |
| --- | --- | --- | --- | --- | --- | --- |
| DEC-001 | Import-graph check; reference-application conformance report | Core imports no adapters; unguarded call sites are reported | CON-001 withdrawn or Q-001 includes host compromise with no acceptable library-only mitigation | Requesting user | Not run | high-level-design-generator |
| DEC-002 | Dependency lint; port conformance suite | Adapters depend on domain, never the reverse | A mandated single framework makes ports unnecessary (does not reverse, simplifies) | Unassigned | Not run | high-level-design-generator |
| DEC-003 | Fault injection on detectors, sinks and policy loading | Deny plus audit on failure; events buffered or dropped per policy without blocking decisions | Requirement for synchronous guaranteed audit delivery appears | Unassigned | Not run | high-level-design-generator |
| DEC-004 | Dry-run parity test | Identical decisions in dry-run and enforce modes | Need for continuous in-process scanning | Unassigned | Not run | high-level-design-generator |
| DEC-011 | Q-001 answer; strict-mode contract prototype | Threat-model decision recorded | Q-001 answered "excluded" closes it as Rejected; "included" promotes to Proposed | Requesting user | Open | high-level-design-generator |
| DEC-012 | Benchmark at stated percentile; signal-quality comparison | Overhead within the agreed target | Latency target allows inline ML, or footprint tolerance forbids optional extras | Unassigned | Open | high-level-design-generator |
