# Security Architecture Review — Asphaleia

## Handoff

- Design ID: ASPHALEIA.
- Contract version: 1.0.0.
- Artifact: security-architecture-reviewer — scenario-based security review.
- Artifact status: Provisional — all 17 areas examined against the written baseline; several areas are Unresolved because the design depends on host-supplied contracts (identity, ACL metadata, sink, secret store) that are not yet evidenced. No area is certified.
- Baseline: [04-high-level-design.md](04-high-level-design.md) sections 10–17 (CMP-001–CMP-015, TB-001–TB-007, FLW-001–FLW-007, INT-001–INT-008), [01-requirements-baseline.md](01-requirements-baseline.md).
- Evidence summary: Confirmed — BR-001, CON-001; Inferred — trust-boundary tracing and threat scenarios; Assumed — ASM-002, ASM-003; Proposed — every mitigation; Unresolved — Q-001 (host compromise), Q-005 (approval channel), Q-008 (retention), Q-013 (residency), provider option coverage.
- Changed IDs: added SEC-001–SEC-009; added queued Q-015; proposed delta D-01 to the HLD owner (below). No RISK record changed; RISK-001–RISK-012 linked.
- Open questions: active batch Q-001–Q-007 unchanged; queued Q-008–Q-015. No Critical question raised: no decision requires unauthorized access or resolves a legal conflict.
- Validation: design-level trace of principal → entry point → component → data across TB-001–TB-007 by reading; no scan, test or exploit was performed or is claimed.
- Next handoff: `reliability-scalability-reviewer` (not yet run); HLD owner merges findings into sections 18 and 26 (done) and notes section 23 implications (state integrity).

## Review scope and evidence

- Reviewed scope: the in-process package as designed (Candidate A), its seven trust boundaries, eight integration contracts and seven flows.
- Excluded scope and rationale: the Deferred strict-mode component (DEC-011) has no design to review; host application code, provider internals, tool servers and infrastructure isolation are outside CON-004.
- Baseline limitations: no implementation, no adopter environment, no supplied identity model, classification policy or regulatory applicability; every classification is Proposed.
- Supplied authority: requesting user (SRC-001); no security policy owner or risk-acceptance authority supplied.

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | User request | Conversation 2026-09-16 | Authorized user input | Available |
| SRC-002 | Concern inventory | Conversation 2026-09-16 | User-adopted brief | Available |
| SRC-003 | OWASP Top 10 for LLM Applications 2025 | genai.owasp.org/llm-top-10/ | Public guidance | Fetched |
| SRC-004 | OWASP agentic threats and Top 10 for Agentic Applications 2026 | genai.owasp.org resources | Public guidance | Partially fetched |
| SRC-005 | Willison, lethal trifecta | simonwillison.net (June 2025) | Public guidance | Fetched |
| SRC-006 | Hacker News incident and tooling threads | hn.algolia.com | Public discussion | Fetched |
| SRC-007 | Concern isolation analysis | [00-concern-isolation.md](00-concern-isolation.md) | Derived analysis | Available |

## Security coverage

| Area | Review status | Evidence and affected IDs | Threat scenario or gap | Finding or risk IDs | Validation required |
| --- | --- | --- | --- | --- | --- |
| Identity | Unresolved | Principal and tenant asserted by host via INT-001 (ASM-003); approver identity asserted by channel adapter (INT-007); no agent identity for cross-process hops | Host binds the wrong principal or none; cross-process agent chain loses attribution | RISK-008; Q-015 | Adopter identity contract review; negative tests for missing binding |
| Authentication | Not applicable (package) / Unresolved (adapters) | Package authenticates nobody; adapters carry provider, tool, store and sink authentication through CMP-005 handles | Adapter misconfiguration sends credentials outside the broker path | SEC-009 | Adapter conformance: no credential outside CMP-005 |
| Authorization | Covered (Proposed) | CMP-002 default deny (BR-002); capability classes; trifecta rule FR-003; retrieval double filter FLW-004; memory write gating | Injected instruction triggers an external-communication tool after untrusted ingestion | RISK-004; SEC-006 | Attack-corpus tests; policy lint |
| Least privilege | Covered (Proposed) | Capability classes narrow tool rights; handles instead of secrets; code-execution class can require INT-008 | Over-broad capability declaration by developer | RISK-004 | Combination report (FR-008); review of default classes |
| Network security and trust boundaries | Gap (by constraint) | TB-001–TB-006 enforced in-process; TB-007 not enforceable; no independent egress control (CON-004) | Host bypasses adapters or performs its own outbound calls | SEC-001; RISK-001 | Conformance check for unguarded call sites; Q-001 |
| Data classification | Unresolved | DATA-001–DATA-010 classifications Proposed; ACL metadata classification follows source | Misclassified content treated as trusted; ACL metadata absent | RISK-008 | Host classification confirmation; fail-closed tests |
| Encryption in transit and at rest | Unresolved | Owned by host and external systems; package adds no transport; CMP-015 file confidentiality is host's, integrity via hash | State file readable by other processes on host | SEC-005 | Host confirmation of file permissions and transport settings in adapters |
| Secrets management | Covered (Proposed) | CMP-005 broker; handles only; scrub responses and exceptions; no header logging | Secret in traceback or echoed response field | SEC-009; RISK-005 | Canary-secret tests through prompts, results, exceptions, events |
| Audit logging | Covered (Proposed) | FLW-005 events; redaction before emission; correlation; bounded buffer | Redaction misses; sink outage drops events silently | SEC-004; RISK-010 | Redaction tests; sink-unavailable tests with drop counter |
| Input validation | Covered (Proposed) | FR-004 argument validation; FR-010 sanitization; detectors advisory (BR-001) | Argument smuggling of data into allowed tools | SEC-006 | Argument redaction and size-limit tests |
| API security | Covered (Proposed) | Provider endpoint and model allow-list (FR-018); URL allow-list on output (FR-014); budgets (FR-016) | Markdown image exfiltration; denial-of-wallet | RISK-004 | Output-guard and budget tests |
| Tenant isolation | Covered (Proposed) | CMP-001 context variables; fail closed; double filtering | Context lost across executors; store filter bypass | SEC-003; RISK-011 | Concurrency tests; negative retrieval tests |
| Threat detection | Covered (Proposed) | Deny rates, drift blocks, detector signals, approval expiry emitted for the host's sink | No consumer configured; alerts not defined | — | Host sink and alert configuration (section 21) |
| Backup security | Covered (Proposed) | Package state reconstructible; integrity hash; policy in host VCS; audit durability is sink's | Restored stale state hides drift | SEC-005 | Restore test with drift after snapshot |
| Supply chain | Covered (Proposed) | DEC-010 for the package; FR-006–FR-008 for tools; no runtime downloads | Malicious optional extra; compromised release | SEC-008; RISK-007 | SBOM review; signature verification; egress test |
| Privacy | Unresolved | Minimization and redaction Proposed; retention and residency Unresolved (Q-008, Q-013); no personal data in package logs | Prompt content persisted in audit beyond need | RISK-010 | Retention decision; redaction tests |
| Regulatory concerns | Not applicable (package) / Unresolved (host) | CON-004: assessment is the host's; package supplies evidence (FLW-005) | Adopter assumes package delivers compliance | — | Documentation stating evidence-only role; stakeholder validation |

## Security findings

| Finding ID | Severity | Finding | Affected component | Reason | Recommended mitigation | Residual risk | Validation required | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SEC-001 | High | Cooperative boundary: any model, tool, retrieval or output path the host does not route through the adapters bypasses every control | CMP-012, CMP-003, CMP-011, TB-007, INT-001 | Finding type: Proposed design risk. Safe threat scenario: a developer registers a tool directly with the framework; an injected instruction invokes it without policy evaluation and exfiltrates data. Evidence class and source: Inferred from CON-003 and SRC-005. Exposure and known controls: full bypass; documented limitation only. Severity rationale: credible serious confidentiality harm with no viable in-package control. Severity status: Provisional pending Q-001 and conformance-check design. | Framework-level wrapping that intercepts all call paths; conformance check reporting unguarded call sites; explicit documentation; strict mode Deferred (DEC-011) | Host defects and deliberate bypass | Conformance report on a reference application; Q-001 | CON-003, FR-020 |
| SEC-002 | High | Approval replay or substitution if approval is not bound to the exact call and consumed once | CMP-003, CMP-015, DATA-007, INT-007 | Finding type: Proposed design risk. Safe threat scenario: an approval granted for one call is reused for a different, harmful call, or a delayed response approves a superseded request. Evidence class and source: Inferred from FR-005 and SRC-004. Exposure and known controls: consequential actions; design specifies hash binding, single use, expiry. Severity rationale: integrity of the human control is the last line for gated actions. Severity status: Provisional until binding fields and state integrity are prototyped. | Bind to hash of tool, arguments, principal, provenance digest and policy version; persist pending record before sending; consume exactly once; expire; integrity-hashed state | Approver deceived by plausible request (RISK-009) | Replay, late-response and expiry negative tests | FR-005, BR-003 |
| SEC-003 | Medium | Tenant or principal context lost or leaked across thread pools and asyncio tasks | CMP-001, CMP-007, TB-005 | Finding type: Proposed design risk. Safe threat scenario: host offloads retrieval to an executor without copying context; the guard sees no tenant and either denies (safe) or, if misimplemented, inherits another request's tenant. Evidence class and source: Inferred from NFR-009. Exposure and known controls: cross-tenant disclosure; design mandates fail closed. Severity rationale: bounded by fail-closed design; leakage requires implementation error. Severity status: Provisional. | Explicit context capture and copy helpers; deny when absent; concurrency tests | Host-created tasks outside adapters | Interleaved-tenant concurrency tests | FR-021, NFR-009 |
| SEC-004 | Medium | Pattern-based redaction misses unknown secret or personal-data shapes | CMP-008, CMP-010, DATA-005 | Finding type: Proposed design risk. Safe threat scenario: a provider-specific token format not in the pattern set appears in a tool result and is echoed into an audit event. Evidence class and source: Inferred. Exposure and known controls: sink readers; summaries-by-default reduce exposure. Severity rationale: bounded, recoverable, credible workaround. Severity status: Provisional. | Summaries by default; configurable patterns; canary tests; length limits | Novel formats until patterns updated | Redaction test corpus | FR-015, FR-017, NFR-004 |
| SEC-005 | Medium | Local state tampering hides drift or enables approval replay | CMP-015, DATA-002, DATA-007 | Finding type: Proposed design risk. Safe threat scenario: a process with host file access rewrites a pin to match a changed manifest. Evidence class and source: Inferred from ASM-008. Exposure and known controls: drift detection defeated; integrity hash proposed. Severity rationale: requires host-level access, which equals host compromise (Q-001). Severity status: Provisional. | Integrity hash keyed via INT-004; read-only deployment option; audit state mutations | Host compromise | Tamper tests; restore-with-drift test | FR-005, FR-006 |
| SEC-006 | Medium | Covert exfiltration through arguments of allowed external-communication tools | CMP-003, CMP-008, TB-003 | Finding type: Proposed design risk. Safe threat scenario: with untrusted content present, a search tool remains allowed by a permissive policy and the query carries private data. Evidence class and source: Inferred from SRC-005. Exposure and known controls: low-bandwidth channel; trifecta rule gates such tools by default. Severity rationale: bounded when default policy is kept; policy relaxation is the risk. Severity status: Provisional. | Default classes treat any tool that leaves the process as external-communication; argument redaction and size limits; lint warns on relaxations | Covert channels via legitimately allowed tools | Policy lint and argument-redaction tests | FR-003, FR-004 |
| SEC-007 | Low | Policy loaded from a writable or ambiguous path can be altered locally | CMP-013, DATA-003 | Finding type: Proposed design risk. Safe threat scenario: policy path resolved from the working directory picks up an attacker-written file. Evidence class and source: Inferred. Exposure and known controls: host compromise prerequisite. Severity rationale: hygiene. Severity status: Provisional. | Explicit path; integrity hash; optional signature | Host compromise | Load-path tests | FR-002, NFR-008 |
| SEC-008 | Medium | Optional ML detector extra could download models or dependencies at runtime, violating BR-004 | CMP-006, CMP-012 | Finding type: Proposed design risk. Safe threat scenario: an extra fetches a model at first run, creating an unaudited egress and supply-chain path. Evidence class and source: Inferred from DEC-012. Exposure and known controls: no default egress policy. Severity rationale: bounded to adopters installing the extra. Severity status: Provisional. | No runtime downloads; models bundled or supplied by explicit path; egress test | Untrusted extra installed by adopter | Egress test on extra install | BR-004, NFR-002 |
| SEC-009 | Medium | Credential material surfaces in exceptions, tracebacks or echoed headers | CMP-005, INT-002, INT-003 | Finding type: Proposed design risk. Safe threat scenario: an HTTP client exception includes request headers with the injected credential and propagates to host logs. Evidence class and source: Inferred. Exposure and known controls: broker scrubbing proposed. Severity rationale: credible but bounded by scrubbing and NFR-004. Severity status: Provisional. | Wrap and scrub exceptions; never log headers; scrub response fields; canary tests | Provider echoing secrets in unusual fields | Canary-secret tests including exception paths | FR-009, NFR-004 |

## Risks and unresolved evidence

Shared risks RISK-001–RISK-012 are inherited from [02-architecture-drivers.md](02-architecture-drivers.md) without duplication; links: SEC-001→RISK-001; SEC-002→RISK-009, RISK-012; SEC-003→RISK-011; SEC-004→RISK-010; SEC-005→RISK-012; SEC-006→RISK-004; SEC-008→RISK-007; SEC-009→RISK-005. No new RISK record is needed; owner Unassigned throughout.

Unresolved evidence: host identity contract (ASM-003), host-compromise scope (Q-001), approval channel and approver role (Q-005), retention and residency (Q-008, Q-013), provider request-option coverage, cross-process agent identity (Q-015).

## Validation plan

| Finding or risk ID | Validation scenario | Evidence required | Expected observable outcome | Owner, if supplied | Status | Residual uncertainty |
| --- | --- | --- | --- | --- | --- | --- |
| SEC-001 | Reference application with one unwrapped tool | Conformance report | Unwrapped call site listed; documentation states limitation | Unassigned | Planned | Cannot detect dynamic bypass at runtime |
| SEC-002 | Replay, late response, modified arguments | Test results | All rejected; only exact-hash single-use approval executes | Unassigned | Planned | Approver judgment |
| SEC-003 | Interleaved tenants across executor and asyncio tasks | Test results | No cross-tenant chunk; missing context denies | Unassigned | Planned | Host tasks outside adapters |
| SEC-004 | Seeded secrets and personal data in results and outputs | Redaction report | Zero leaks for seeded corpus | Unassigned | Planned | Unknown formats |
| SEC-005 | Modify state file; restore stale snapshot | Test results | Integrity failure denies; drift after restore detected | Unassigned | Planned | Host access |
| SEC-006 | Permissive policy with untrusted content and a search tool | Lint output and test | Lint warning; default policy gates the call | Unassigned | Planned | Relaxed policies |
| SEC-007 | Ambiguous policy path | Test results | Load refused without explicit path | Unassigned | Planned | — |
| SEC-008 | Install extra in isolated network | Egress log | No outbound connection | Unassigned | Planned | Third-party extras |
| SEC-009 | Force HTTP exceptions with injected credential | Log capture | No credential in exception text or logs | Unassigned | Planned | Provider fields |

## Questions and proposed deltas

- Assumptions: ASM-001–ASM-008 inherited unchanged.
- Questions: active batch unchanged (Q-001–Q-007). New queued question Q-015 (Optional): "Do multi-agent deployments span processes, requiring agent identity and provenance propagation across process boundaries?" Why it matters: attribution and trifecta state across hops. Answer options: single process; multi-process with shared context; unknown. Default assumption: single process (design scope). Blocks: none in first release. Status: Open. Related: FR-001, FR-021.
- Decisions: no DEC change proposed; DEC-011 remains Deferred on Q-001.
- Structured requirements and traceability: inherited unchanged; every FR/NFR/BR/CON retains its row in HLD section 27.
- Proposed delta D-01 (to `high-level-design-generator`): make the conformance check for unguarded call sites an explicit responsibility of CMP-014 (offline) and CMP-012 (startup warning) rather than an implied capability. Affected IDs: CMP-012, CMP-014, SEC-001, RISK-001. Rationale: SEC-001 mitigation must be a designed feature to be testable. Validation: conformance report test. Status: merged — HLD section 14 now lists the conformance check under CMP-014 (offline report) and CMP-012 (startup warning), and section 27 traces CON-003 to both.

## Review assessment and next steps

- Scope assessment: the design places deterministic controls at every in-process boundary identified in SRC-007 and keeps detection advisory (BR-001). Its principal weakness is structural and acknowledged: TB-007 cannot be enforced by a library.
- Critical blockers: none.
- High-priority mitigation gates: SEC-001 (conformance check designed and tested; Q-001 answered), SEC-002 (approval binding prototyped with negative tests) before detailed-design readiness.
- Strengths supported by evidence: default deny; provenance-based trifecta rule; broker-owned egress for credentials; double filtering for retrieval; fail-closed dependency behavior; no default egress.
- Unresolved decisions: DEC-011, DEC-012.
- Validation not performed: all tests above are planned; no scan, exploit or benchmark executed; diagrams reviewed for boundary consistency only.
- Next consumer and upstream rework: `reliability-scalability-reviewer` (not run); HLD owner to apply D-01 on next revision; no upstream requirement change required.
