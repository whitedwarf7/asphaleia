# Architecture Decision Records — Asphaleia

- Design ID: ASPHALEIA.
- Contract version: 1.0.0.
- Artifact: architecture-decision-record-generator — architecture decision records.
- Artifact status: Provisional — ten ADRs document DEC-001, DEC-002, DEC-003, DEC-005, DEC-006, DEC-007, DEC-009, DEC-010 (Proposed) and DEC-011, DEC-012 (Deferred); no decision is Accepted because no approval authority or evidence has been supplied. DEC-004, DEC-008, DEC-013 and DEC-014 are minor and recorded only in the HLD decision register.
- Baseline: [04-high-level-design.md](04-high-level-design.md) section 24, [03-architecture-style.md](03-architecture-style.md), [02-architecture-drivers.md](02-architecture-drivers.md).
- Evidence summary: Confirmed — CON-001, BR-001; Inferred — driver effects; Assumed — ASM-002, ASM-003, ASM-004, ASM-008; Proposed — all decisions and consequences; Unresolved — Q-001, Q-002, Q-004, Q-006, Q-007, Q-012.
- Changed IDs: added ADR-001, ADR-002, ADR-003, ADR-005, ADR-006, ADR-007, ADR-009, ADR-010, ADR-011, ADR-012. No DEC delta proposed.
- Open questions: active batch Q-001–Q-007 unchanged; queued Q-008–Q-015.
- Validation: ADR-to-DEC status match checked by reading; no approval events, dates or versions exist to record.
- Next handoff: stakeholder review of ADRs; `architecture-reviewer` final assessment after reliability and operations passes. Return trigger: any DEC status change or new major decision in the HLD.

## Decision-to-ADR index

| ADR ID | Decision ID | DEC Status | ADR Status | Evidence or gap | Related requirements | Related risk IDs | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ADR-001 | DEC-001 | Proposed | Proposed | CON-001 Confirmed; threat-model boundary Unresolved (Q-001) | CON-001, CON-003, FR-020 | RISK-001 | Import-graph check; conformance report |
| ADR-002 | DEC-002 | Proposed | Proposed | Framework list Unresolved (Q-002) | FR-020, NFR-006, NFR-007, CON-002 | RISK-006 | Dependency lint; port conformance suite |
| ADR-003 | DEC-003 | Proposed | Proposed | Buffer size and drop policy Unresolved (Q-008) | BR-001, NFR-003, FR-017 | RISK-010 | Fault injection; sink-unavailable tests |
| ADR-005 | DEC-005 | Proposed | Proposed | Default capability classes Proposed | FR-001, FR-003, BR-001, CON-005 | RISK-002, RISK-004 | Attack-corpus conformance |
| ADR-006 | DEC-006 | Proposed | Proposed | Host-compromise case Unresolved (Q-001) | FR-009, NFR-004, BR-004 | RISK-005 | Canary-secret tests |
| ADR-007 | DEC-007 | Proposed | Proposed | Identity/ACL contract Assumed (ASM-003) | FR-012, FR-013, FR-021, BR-002 | RISK-008, RISK-011 | Negative retrieval and concurrency tests |
| ADR-009 | DEC-009 | Proposed | Proposed | Priorities Unspecified (Q-009) | FR-002, FR-022, BR-002, NFR-008 | RISK-004 | Dry-run parity; policy lint |
| ADR-010 | DEC-010 | Proposed | Proposed | Signing authority Unresolved (Q-006) | NFR-002, BR-004 | RISK-007 | SBOM review; signature and egress tests |
| ADR-011 | DEC-011 | Deferred | Deferred | Q-001 open | CON-003, FR-009 | RISK-001, RISK-005 | Q-001 answer; contract prototype |
| ADR-012 | DEC-012 | Deferred | Deferred | Q-004, Q-007 open | FR-011, NFR-001 | RISK-003 | Benchmark; signal-quality comparison |

## ADR records

### ADR-001

- ADR ID: ADR-001
- Title: Deliver Asphaleia as a single in-process installable Python package
- Status: Proposed
- Context: The user requires an installable Python package (CON-001, SRC-001). The concern isolation (SRC-007 §1) shows that the addressable concerns are addressable precisely because their control points are inside the application process. A library cannot enforce against its own host (CON-003); whether host compromise is in the threat model is Unresolved (Q-001).
- Decision drivers: DRV-002 (neutrality), DRV-005 (cooperative limit), DRV-007 (supply chain); CON-001, CON-003, FR-020.
- Considered options: (1) In-process library — matches CON-001; simplest install; cooperative boundary. (2) Library plus strict-mode sidecar — stronger credential isolation; two deployment forms (analyzed in ADR-011). (3) Gateway or proxy only — resists host compromise but sees only network traffic and loses provenance, retrieval, memory and approval control points; contradicts CON-001. Options 2 and 3 are Proposed analytical alternatives, not supplied history.
- Decision: Option 1, with option 2 retained as a Deferred extension.
- Rationale: CON-001 is the only Confirmed constraint; the library form places controls where the incident history (SRC-005, SRC-006) shows the failures occur. Option 3 would make rows 2, 8, 12 and 15 of the concern matrix unaddressable.
- Positive consequences: single distributable; synchronous local decisions; no distributed contract; adapters can wrap frameworks at import time. Affected IDs: all CMP; DEP-001.
- Negative consequences: TB-007 cannot be enforced; the package's promises are conditional on host integration (SEC-001); per-process state (Q-012).
- Risks: RISK-001 (partial integration) — mitigated by conformance check (D-01); residual host defects. Acceptance evidence: none.
- Validation conditions: import-graph check that the core is framework-free; conformance report on a reference application; reconsider if CON-001 is withdrawn or Q-001 includes host compromise with no acceptable library-only mitigation. Status: planned.
- Related requirements: CON-001, CON-003, FR-020.
- Decision ID: DEC-001 (Proposed; no approval evidence).

### ADR-002

- ADR ID: ADR-002
- Title: Hexagonal core with ports and adapters and a layered dependency direction
- Status: Proposed
- Context: The package must attach to any provider, framework, store and sink without mandating one (CON-002) and remain testable without frameworks (NFR-006). The first-release framework list is Unresolved (Q-002).
- Decision drivers: DRV-002, DRV-003; FR-020, NFR-006, NFR-007.
- Considered options: (1) Hexagonal core with ports for detectors, secrets, sinks, approvals, retrieval, sandbox and adapters as optional extras. (2) Framework-specific core (plugin for one agent framework) — deepest integration, vendor lock, violates CON-002. (3) Plugin-only architecture with no stable core contracts — flexible but untestable boundaries.
- Decision: Option 1.
- Rationale: Neutrality and determinism require a domain that imports no framework types; ports make each boundary independently testable and swappable.
- Positive consequences: framework-free domain; per-port conformance suites; adapters versioned independently. Affected IDs: CMP-001–CMP-011, CMP-012.
- Negative consequences: indirection; adapter fidelity must be tested for each supported framework version; adapter count grows with Q-002.
- Risks: RISK-006 (adapter drift) — mitigated by separately versioned extras and a conformance suite.
- Validation conditions: dependency lint (domain imports no adapter); port conformance suite passes for each adapter; reconsider only if a single framework is mandated (simplifies, does not reverse). Status: planned.
- Related requirements: FR-020, NFR-005, NFR-006, NFR-007, CON-002.
- Decision ID: DEC-002 (Proposed).

### ADR-003

- ADR ID: ADR-003
- Title: Synchronous inline enforcement decisions with in-process event emission for audit only
- Status: Proposed
- Context: BR-001 requires a deterministic decision before any consequential action; NFR-003 requires fail-closed behavior; FR-017 requires audit that should not block the hot path. Buffer size and overflow policy are Unresolved (Q-008).
- Decision drivers: DRV-003, DRV-009; BR-001, NFR-003, FR-017.
- Considered options: (1) Synchronous decisions; asynchronous bounded-buffer audit emission that never carries decisions. (2) Fully synchronous audit — guaranteed ordering, but a slow sink blocks every action. (3) Asynchronous decisions — incompatible with BR-001.
- Decision: Option 1.
- Rationale: decisions must be deterministic and immediate; audit durability is the sink's responsibility, so bounded buffering with explicit drop-or-deny semantics is the honest contract.
- Positive consequences: hot path independent of sink latency; explicit failure semantics. Affected IDs: CMP-002, CMP-010, INT-005.
- Negative consequences: events can be dropped under sustained sink outage unless policy selects deny; ordering across processes not guaranteed.
- Risks: RISK-010 (redaction gaps) — mitigated by redaction before buffering; event loss — surfaced by a drop counter.
- Validation conditions: fault injection (policy load failure, detector failure) yields deny plus audit; sink-unavailable test shows buffering then the configured semantics; reconsider if a requirement for synchronous guaranteed audit delivery appears. Status: planned.
- Related requirements: BR-001, NFR-003, FR-017, NFR-010.
- Decision ID: DEC-003 (Proposed).

### ADR-005

- ADR ID: ADR-005
- Title: Provenance labels as the core data model and the lethal-trifecta rule in the policy engine
- Status: Proposed
- Context: Indirect prompt injection is the most frequently reported production failure (SRC-005, SRC-006). LLMs cannot reliably distinguish instructions by origin (CON-005). Detection is probabilistic and not a boundary (BR-001).
- Decision drivers: DRV-001, DRV-003; FR-001, FR-003, BR-001, CON-005.
- Considered options: (1) Label every context segment with provenance and trust; the policy engine denies or gates external-communication and consequential-write tools once untrusted content is present. (2) Detection-first guardrail as the boundary — rejected by BR-001 and SRC-005. (3) Dual-model quarantine pattern only — reduces exposure but does not by itself constrain tool capability; can compose with option 1 later (Proposed).
- Decision: Option 1, with detection retained as an advisory signal and option 3 left open as a future composition.
- Rationale: the rule is deterministic, testable and independent of provider behavior; it converts an unsolvable text-classification problem into a capability decision.
- Positive consequences: deterministic exfiltration boundary; provenance available to approvals, memory gating and audit. Affected IDs: CMP-001, CMP-002, CMP-003, CMP-006, CMP-007.
- Negative consequences: every ingress path must be labeled (unlabeled paths are treated as untrusted or denied); agents lose external-communication capability after reading untrusted content unless a human approves, which changes user experience.
- Risks: RISK-002 (detection treated as boundary), RISK-004 (permissive policy) — mitigated by default classes and lint (SEC-006).
- Validation conditions: attack-corpus conformance for labeled paths (deterministic pass); negative tests for unlabeled content; reconsider only if a provider-independent mechanism proves the instruction hierarchy, which is not evidenced. Status: planned.
- Related requirements: FR-001, FR-003, FR-013, BR-001, CON-005.
- Decision ID: DEC-005 (Proposed).

### ADR-006

- ADR ID: ADR-006
- Title: Credential brokering with opaque handles and broker-performed egress
- Status: Proposed
- Context: Manipulated agents leak the credentials they can read (SRC-002 §2, SRC-006 Agent Vault thread). Whether host compromise is in scope is Unresolved (Q-001).
- Decision drivers: DRV-004; FR-009, NFR-004, BR-004.
- Considered options: (1) In-process broker: handles in prompts and tool arguments; CMP-005 resolves and injects only inside the outbound call it performs; scrubs responses and exceptions. (2) Tools read secrets from environment variables — zero effort, but any code path or prompt-driven behavior can read them. (3) Out-of-process broker or egress proxy — credentials never in the host process; new deployment form (ADR-011).
- Decision: Option 1, with option 3 Deferred.
- Rationale: option 1 removes credentials from model context and tool code under ASM-002; option 2 fails FR-009 outright; option 3 depends on Q-001.
- Positive consequences: no credential in DATA-001, tool results or events; single scrubbing point. Affected IDs: CMP-005, CMP-003, CMP-011, INT-004, TB-004.
- Negative consequences: tools must call through the broker for credentialed requests (integration effort); credentials still exist in process memory during a call (host compromise not defended).
- Risks: RISK-005 (leak via exceptions or echoed fields) — mitigated by scrubbing (SEC-009).
- Validation conditions: canary-secret tests through prompts, tool results, exception paths and events; reconsider (promote option 3) if Q-001 includes host compromise. Status: planned.
- Related requirements: FR-009, FR-015, NFR-004, BR-004.
- Decision ID: DEC-006 (Proposed).

### ADR-007

- ADR ID: ADR-007
- Title: Retrieval authorization fails closed on host-supplied ACL metadata; the package never owns entitlement truth
- Status: Proposed
- Context: Copilots surface over-shared content and shared vector stores leak across tenants (SRC-002 §1 LLM08, §3). The host owns entitlements; the package can only enforce metadata it is given (ASM-003).
- Decision drivers: DRV-008; FR-012, FR-013, FR-021, BR-002.
- Considered options: (1) Fail closed: add tenant and principal filters to queries, post-filter results on ACL metadata, drop chunks lacking metadata, deny when identity is unbound. (2) Verify-presence-only mode: check that the host applied a filter, pass results through — weaker, easier adoption. (3) Package-owned ACL store — makes the package an entitlement authority it cannot keep correct.
- Decision: Option 1; option 2 may be offered as an explicitly audited opt-in, not a default (Proposed).
- Rationale: correctness of tenant isolation outweighs adoption friction; an entitlement copy inside the package would drift from the source of truth.
- Positive consequences: deterministic tenant boundary inside the application; defense in depth against store misconfiguration. Affected IDs: CMP-007, CMP-001, INT-006, TB-005.
- Negative consequences: hosts without ACL metadata cannot use retrieval through the guard; requires a stable identity binding (INT-001).
- Risks: RISK-008 (missing or wrong metadata), RISK-011 (context leakage) — mitigated by fail-closed behavior and concurrency tests (SEC-003).
- Validation conditions: negative retrieval tests; interleaved-tenant concurrency tests; adopter confirmation of ASM-003; reconsider default if adopters cannot supply metadata and accept audited opt-in. Status: planned.
- Related requirements: FR-012, FR-013, FR-021, NFR-009, BR-002.
- Decision ID: DEC-007 (Proposed).

### ADR-009

- ADR ID: ADR-009
- Title: Default-deny, versioned declarative policy with dry-run mode
- Status: Proposed
- Context: Excessive agency and over-broad tools are core concerns (SRC-003 LLM06). Requirement priorities are Unspecified (Q-009); adoption needs a safe rollout path (DRV-012).
- Decision drivers: DRV-003, DRV-012; FR-002, FR-022, BR-002, NFR-008.
- Considered options: (1) Default deny for unregistered tools, undeclared capabilities, unknown sinks and missing identity; versioned declarative policy; dry-run recording would-be decisions. (2) Allow-by-default with blocklists — lower friction, silently permissive (RISK-004). (3) Imperative policy code only — flexible, hard to lint and version.
- Decision: Option 1, allowing imperative extensions only through the detector and constraint ports.
- Rationale: deny-by-default is the only posture consistent with BR-002 and the failure history; dry-run converts friction into a tuning loop rather than a bypass.
- Positive consequences: deterministic, reproducible decisions with policy version in every event; lintable policy; staged rollout. Affected IDs: CMP-002, CMP-013, CMP-014.
- Negative consequences: initial integration friction; policy authoring becomes a required skill (ACT-002).
- Risks: RISK-004 — mitigated by lint, dry-run parity and the combination report.
- Validation conditions: dry-run parity test; policy lint; default-deny tests; reconsider only with evidence that deny-by-default blocks adoption without an acceptable opt-in path. Status: planned.
- Related requirements: FR-002, FR-004, FR-022, BR-002, NFR-008, NFR-010.
- Decision ID: DEC-009 (Proposed).

### ADR-010

- ADR ID: ADR-010
- Title: Package supply-chain posture — minimal dependencies, signed releases, SBOM, no default egress
- Status: Proposed
- Context: A security library installed into many applications is a high-value supply-chain target (SRC-003 LLM03). Practitioners object to security tooling that routes data to third parties (SRC-006). Signing authority is Unresolved (Q-006).
- Decision drivers: DRV-007; NFR-002, BR-004.
- Considered options: (1) Minimal mandatory dependencies, optional extras for adapters and detectors, signed releases with SBOM, reproducible builds, no outbound network by default. (2) Bundled full-featured distribution — convenient, large attack surface. (3) Hosted control plane with telemetry — operationally rich, violates BR-004.
- Decision: Option 1.
- Rationale: the package must not become the exfiltration path it is meant to prevent.
- Positive consequences: small trusted base; verifiable artifacts; no phone-home. Affected IDs: build pipeline (not a CMP), CMP-012 extras, CMP-006 detector extra.
- Negative consequences: features are opt-in installs; publishing requires key management and a signing owner.
- Risks: RISK-007 — mitigated by option 1; residual: adopter-installed untrusted extras (SEC-008).
- Validation conditions: SBOM review; signature verification in a clean environment; egress test of a default install; reconsider none. Status: planned; signing authority pending Q-006.
- Related requirements: NFR-002, BR-004, CON-001.
- Decision ID: DEC-010 (Proposed).

### ADR-011

- ADR ID: ADR-011
- Title: Out-of-process strict-mode component (credential broker or gateway)
- Status: Deferred
- Context: If host-process compromise is in the threat model, an in-process broker cannot keep credentials out of reach and in-process policy can be bypassed. SRC-006 shows both library and proxy forms in practice. Q-001 is open.
- Decision drivers: DRV-004, DRV-005; CON-003, FR-009.
- Considered options: (1) Library only (current baseline). (2) Library plus optional sidecar holding credentials and optionally evaluating egress policy — defense in depth, two forms, divergence risk. (3) Gateway only — rejected in ADR-001.
- Decision: Unresolved — selection deferred until Q-001 is answered.
- Rationale: choosing without the threat-model answer would either over-build (two forms without need) or under-protect (library alone against host compromise).
- Positive consequences: none until decided; keeping the option open preserves the core modules of option 1 unchanged.
- Negative consequences: adopters needing strict isolation have no supported path yet; design of the strict-mode contract is not started.
- Risks: RISK-001, RISK-005 remain at residual levels described in ADR-001 and ADR-006.
- Validation conditions: Q-001 answer recorded; if "included", prototype the strict-mode contract and rerun the security review; if "excluded", mark Rejected with the recorded rationale. Status: open.
- Related requirements: CON-003, FR-009.
- Decision ID: DEC-011 (Deferred).

### ADR-012

- ADR ID: ADR-012
- Title: Placement and packaging of advisory injection detectors
- Status: Deferred
- Context: Detectors are advisory signals only (BR-001) but affect hot-path latency (NFR-001, target Unresolved, Q-004) and dependency footprint (Q-007). Runtime downloads would violate BR-004 (SEC-008).
- Decision drivers: DRV-006, DRV-003; FR-011, NFR-001, BR-004.
- Considered options: (1) Inline structural heuristics only in the core; ML detectors as an optional extra, asynchronous by default with no runtime downloads. (2) Bundled ML detector inline — best signal, largest footprint and latency. (3) No ML detector — smallest footprint, weaker signal.
- Decision: Unresolved — selection deferred; the working assumption for the baseline is option 1 (ASM-004), reversible.
- Rationale: the latency target and footprint tolerance decide between options; committing now would fabricate a target.
- Positive consequences: baseline proceeds with structural checks; ports allow either outcome.
- Negative consequences: signal quality of the first release is limited to heuristics until decided.
- Risks: RISK-003 (latency), RISK-002 (detection treated as boundary) — the latter is independent of placement and mitigated by ADR-005.
- Validation conditions: benchmark added latency at the stated percentile with and without detectors; signal-quality comparison on the attack corpus; Q-004 and Q-007 answered. Status: open.
- Related requirements: FR-011, NFR-001, NFR-002, BR-001, BR-004.
- Decision ID: DEC-012 (Deferred).

## Blocked record ledger

None. No ADR candidate is blocked: every major decision has either a Proposed record with complete fields or a Deferred record whose missing evidence and resume condition are explicit (ADR-011: Q-001; ADR-012: Q-004, Q-007). Minor decisions DEC-004, DEC-008, DEC-013 and DEC-014 are intentionally documented only in the HLD decision register; they can receive ADRs if a stakeholder requests them.

## Register deltas and validation

- DEC deltas proposed: none. ADR statuses mirror DEC statuses exactly (Proposed or Deferred); no Accepted status exists because no authority or approval evidence was supplied.
- RISK, ASM, Q and requirement registers: inherited unchanged from [01-requirements-baseline.md](01-requirements-baseline.md), [02-architecture-drivers.md](02-architecture-drivers.md) and [05-security-review.md](05-security-review.md) (Q-015 queued).
- Traceability: every ADR lists its related requirement IDs; HLD section 27 remains the authoritative trace.
- Validation performed: status match and field completeness by reading. Not performed: stakeholder review, approval, any test named in the validation conditions.
