# Architecture Drivers — Asphaleia

## Handoff

- Design ID: ASPHALEIA.
- Contract version: 1.0.0.
- Artifact: architecture-driver-analyzer — driver register, quality scenarios, tensions and gates.
- Artifact status: Provisional — all requirements dispositioned; ranks for DRV-006 are conditional on the Unresolved latency target (Q-004); no decision is blocked.
- Baseline: [01-requirements-baseline.md](01-requirements-baseline.md) (Provisional), SRC-001–SRC-007.
- Evidence summary: Confirmed — BR-001 pattern, CON-001, CON-004 framing; Inferred — all DRV impact ranks and effects; Assumed — ASM-001–ASM-008 (inherited); Proposed — quality-scenario measurements; Unresolved — numeric latency, budgets, retention, threat-model boundary.
- Changed IDs: added DRV-001–DRV-013, RISK-001–RISK-012. No requirement, ASM or Q records changed.
- Open questions: active batch unchanged, Q-001–Q-007; queued Q-008–Q-014. No Critical question.
- Validation: concern matrix examined for all nine concerns; requirement disposition audited by reading; no measurements, benchmarks or tools run.
- Next handoff: `architecture-style-selector` ([03-architecture-style.md](03-architecture-style.md)) receives DRV, scenarios, tensions, risks and the inherited registers.

## Driver Summary

- Analyzed scope: FR-001–FR-022, BR-001–BR-004, NFR-001–NFR-010, CON-001–CON-005 for an in-process Python security package.
- High: DRV-001 untrusted-content-driven actions; DRV-002 framework neutrality; DRV-003 deterministic fail-closed enforcement; DRV-004 credential isolation; DRV-005 cooperative in-process limit; DRV-007 package supply-chain posture; DRV-008 tenant/principal correctness. These fix the core model (provenance, policy, broker, ports) and are costly to reverse.
- Medium: DRV-006 hot-path latency (conditional); DRV-009 auditability; DRV-010 consumption control; DRV-011 tool-ecosystem volatility; DRV-012 staged rollout; DRV-013 human approval. Bounded to identifiable modules.
- Unknown impacts: none ranked Unknown; DRV-006 carries an Unresolved target rather than an unknown consequence.
- Material tensions: latency versus detection depth; in-process convenience versus isolation strength; audit richness versus privacy; statefulness versus simplicity; fail-closed versus adoptability.
- Decisions blocked: none. Requirement priority is Unspecified for all records; impact rank below is an architectural judgment, not a business priority.

## Architecture Drivers

| Driver ID | Driver | Related requirement IDs | Evidence status | Impact rank | Architectural effect | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-001 | Untrusted content can steer the model into exfiltration or unintended actions | FR-001, FR-002, FR-003, FR-010, FR-011, FR-014, BR-001, CON-005 | Pattern Confirmed (SRC-005); impact Inferred | High — irreversible data exfiltration; changing the enforcement model later rewrites the core | Provenance labels as a first-class data model; deterministic enforcement at the tool boundary (TB-003) and output boundary (TB-002); detection decoupled from decision | None material | Attack-corpus conformance tests for the trifecta rule and output guard; negative tests for unlabeled content |
| DRV-002 | Attach to any Python agent framework, provider SDK or MCP client without rewrite | FR-020, NFR-005, NFR-007, CON-002 | Inferred | High — coupling the core to one framework forces a rewrite when frameworks change | Core has no framework dependencies; ports for detectors, secrets, sinks, approvals, retrieval, sandbox; adapters shipped as optional extras | Q-002 framework list | Adapter conformance suite run against each supported framework version |
| DRV-003 | Deterministic, fail-closed enforcement separated from advisory detection | BR-001, BR-002, NFR-003, NFR-008, FR-011, FR-022 | BR-001 Confirmed; mechanism Inferred | High — a probabilistic boundary is the failure mode the concern inventory warns about | Policy engine as a pure, versioned evaluation over labeled inputs; detectors emit signals only; every decision recorded with policy version | None | Fault injection (detector unavailable, policy load failure) shows deny plus audit |
| DRV-004 | Credentials must be unreachable from model context | FR-009, FR-015, NFR-004 | Inferred | High — credential theft enables persistent, cross-system harm | Broker owns outbound calls that need credentials; handles in prompts and tool arguments; redaction on results and events | Q-001 (host-compromise scope) | Canary-secret tests through prompts, tool results and audit events |
| DRV-005 | The package is cooperative: it cannot enforce against its own host | CON-003, ASM-002, Q-001 | Inferred | High — determines what the package can honestly promise and whether a second deployment form is needed | Explicit documented boundary; integration conformance checks that detect unguarded tool or model call sites; tamper-evident local state; out-of-process strict mode kept as a Deferred alternative | Q-001 | Coverage report of guarded versus unguarded call sites in a reference application (Proposed) |
| DRV-006 | Enforcement sits on the model- and tool-call hot path | NFR-001, FR-010, FR-011, FR-014, ASM-004 | Target Unresolved | Medium (conditional) — reversible by moving detectors asynchronous or sampled, but perceived overhead affects adoption | Pre-compiled policy; inline structural checks only; ML detectors optional and asynchronous by default; cached manifest checks | Q-004, Q-011 | Benchmark of added latency per hook at stated percentiles |
| DRV-007 | The package is itself a supply-chain item installed into many applications | NFR-002, BR-004, CON-001 | Inferred | High — a compromised or phoning-home security library is a systemic failure; trust is slow to rebuild | Minimal mandatory dependencies, optional extras, signed releases with SBOM, reproducible builds, no outbound network by default | Q-006 | SBOM review; egress test of a default install in an isolated network |
| DRV-008 | Principal and tenant context must be correct under concurrency | FR-012, FR-021, NFR-009, ASM-003, ASM-005 | Inferred | High — cross-tenant leakage is a confidentiality breach; fixing later touches the core context model | Per-request context object bound through context variables; guarded operations fail closed without it; retrieval guard depends on it | Q-010 (queued) | Concurrency tests interleaving tenants across threads and asyncio tasks |
| DRV-009 | Decisions and actions must be reconstructable without leaking sensitive data | FR-017, NFR-004, DATA-005 | Inferred | Medium — reversible schema work, but redaction mistakes leak data | Versioned event schema; redaction pipeline before emission; sink port with explicit buffering and drop semantics | Q-008, Q-013 (queued) | Redaction tests with seeded secrets and personal data; sink-unavailable behavior tests |
| DRV-010 | Consumption must be bounded per request, session and tenant | FR-016, DATA-006 | Inferred | Medium — bounded accounting concern | Accounting hooks at TB-003 and TB-006; counters in local state; deterministic termination with reason | Q-008 (numeric defaults) | Loop and budget-exhaustion tests |
| DRV-011 | Tool and MCP manifests change and may carry hidden instructions | FR-006, FR-007, FR-008, ASM-008, DATA-002 | Inferred | Medium — bounded to the registry, but requires persistence | Registry with pinned hashes; block-on-drift; manifest scanner; combination analysis feeding default policy | Q-012 (multi-process state) | Drift, poisoning-pattern and combination-report tests |
| DRV-012 | Adoption requires staged rollout and offline verification | FR-019, FR-022, NFR-006, NFR-010 | Inferred | Medium — operability and reversibility of policy changes | Dry-run mode yielding would-be decisions; CLI scanner; schema versioning; conformance suite | None | Dry-run parity test: identical decisions in dry-run and enforce modes |
| DRV-013 | Consequential actions may need a human decision bound to the exact call | FR-005, BR-003, Q-005 | Inferred | Medium — introduces asynchronous waiting and state | Approval port; approval record bound to call hash, single-use, expiring; provenance shown to approver | Q-005 | Replay and expiry negative tests |

## Quality Scenarios

| Driver ID | Related requirement IDs | Source or evidence class | Stimulus and source | Environment and workload | Affected operation or boundary | Expected response | Measurement and supplied target | Missing evidence | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DRV-001 | FR-001, FR-003 | SRC-005; Proposed scenario | A fetched web page (EXT-008 via EXT-003) instructs the agent to send a private file to an external address | Agent session with a file-read tool and an email-send tool registered | TB-003 tool decision | Email-send is denied or routed to approval because context carries an untrusted label and the tool is classed external-communication; event emitted | Deterministic: 100% of labeled paths deny or gate; no probabilistic target | None | Conformance test corpus |
| DRV-001 | FR-014 | SRC-005 | Model output contains a markdown image whose URL encodes context data | Chat UI sink declared as markdown | TB-002 | Non-allow-listed URL removed or neutralized; event emitted | Deterministic on declared sinks | Allow-list policy defaults Unresolved | Output-guard tests |
| DRV-003 | BR-001, NFR-003 | Inferred | Advisory detector raises an exception or times out | Any guarded call | Policy evaluation | Deterministic rules unaffected; rules that require a signal evaluate to deny; audit records the failure | Deterministic | None | Fault injection |
| DRV-004 | FR-009, FR-015 | Inferred | Model asks a tool to reveal its credential or embeds a handle in output | Tool with brokered credential | TB-004 | Tool sees only a handle; output and events contain no credential material | Canary detection: 0 leaks in test corpus | Q-001 | Canary-secret tests |
| DRV-005 | CON-003 | Inferred | Host code invokes a tool directly, bypassing the gateway | Reference application | TB-007 | Package cannot prevent it; conformance check reports the unguarded call site | Coverage of guarded call sites: Proposed metric, target Unresolved | Q-001 | Static/dynamic coverage report |
| DRV-006 | NFR-001 | Unresolved target | One model call and three tool calls per request | Steady state, structural checks only | All hooks | Added latency within the planning envelope (ASM-004) | Numeric target Unresolved (Q-004) | Q-004, Q-011 | Benchmark |
| DRV-007 | NFR-002, BR-004 | Inferred | Adopter installs the default distribution | Isolated network | Installation and first run | Signature verifiable; no outbound connection attempted by the package | 0 outbound connections from package code | Q-006 | Egress test, SBOM review |
| DRV-008 | FR-012, FR-021 | Inferred | Interleaved requests for two tenants on one asyncio loop | Shared retrieval store with tenant metadata | TB-005 | Results for one tenant never enter the other's context; missing tenant yields deny | Deterministic | ASM-003 | Concurrency tests |
| DRV-011 | FR-006 | Inferred | A registered MCP server changes a tool description | Second session | Registry check | Tool blocked, drift event emitted, re-approval required | Deterministic | Q-012 | Drift test |
| DRV-013 | FR-005, BR-003 | Inferred | An approver replays an earlier approval for a new call | Gated action | Approval verification | Rejected as not bound to the current call hash or expired | Deterministic | Q-005 | Replay/expiry tests |

## Requirement Disposition

| Requirement ID | Driver IDs | Disposition | Rationale |
| --- | --- | --- | --- |
| FR-001 | DRV-001, DRV-008 | Driver | Provenance is the data model every enforcement decision reads |
| FR-002 | DRV-001, DRV-003 | Driver | Central enforcement point |
| FR-003 | DRV-001 | Driver | Defines the exfiltration boundary |
| FR-004 | DRV-001 | Contributes | Bounded validation at TB-003 |
| FR-005 | DRV-013 | Driver | Introduces asynchronous human step and state |
| FR-006 | DRV-011 | Driver | Requires persistent pins |
| FR-007 | DRV-011 | Contributes | Advisory scan inside the registry |
| FR-008 | DRV-011, DRV-001 | Contributes | Informs default policy |
| FR-009 | DRV-004 | Driver | Separate egress path |
| FR-010 | DRV-001, DRV-006 | Contributes | Inline structural transformation |
| FR-011 | DRV-003, DRV-006 | Driver | Extension point whose placement is latency-sensitive |
| FR-012 | DRV-008 | Driver | Depends on identity contract |
| FR-013 | DRV-001 | Contributes | Provenance-gated writes |
| FR-014 | DRV-001 | Driver | Output boundary |
| FR-015 | DRV-004, DRV-009 | Contributes | Redaction shared by output and audit paths |
| FR-016 | DRV-010 | Driver | Accounting across boundaries |
| FR-017 | DRV-009 | Driver | Event schema and sink semantics |
| FR-018 | DRV-001, DRV-009 | Contributes | Provider boundary controls |
| FR-019 | DRV-012 | Contributes | Offline utility |
| FR-020 | DRV-002 | Driver | Determines internal organization |
| FR-021 | DRV-008 | Driver | Concurrency-correct context |
| FR-022 | DRV-012, DRV-003 | Contributes | Rollout safety |
| BR-001 | DRV-003 | Driver | Detection is not a boundary |
| BR-002 | DRV-003 | Driver | Default deny |
| BR-003 | DRV-013 | Contributes | Approval semantics |
| BR-004 | DRV-007 | Driver | No phone-home |
| NFR-001 | DRV-006 | Driver (target Unresolved) | Hot path |
| NFR-002 | DRV-007 | Driver | Supply-chain posture |
| NFR-003 | DRV-003 | Driver | Fail closed |
| NFR-004 | DRV-004, DRV-009 | Contributes | Sensitive data out of logs |
| NFR-005 | DRV-002 | Contributes | Runtime compatibility |
| NFR-006 | DRV-012 | Contributes | Test suites |
| NFR-007 | DRV-002 | Driver | Ports |
| NFR-008 | DRV-003 | Contributes | Deterministic evaluation |
| NFR-009 | DRV-008 | Driver | Concurrency safety |
| NFR-010 | DRV-012 | Non-driver (low impact) | Versioning hygiene; no boundary effect |
| CON-001 | DRV-005, DRV-007 | Driver | Library topology |
| CON-002 | DRV-002 | Driver | Neutrality |
| CON-003 | DRV-005 | Driver | Cooperative boundary |
| CON-004 | DRV-005 | Contributes | Scope boundary and integration points |
| CON-005 | DRV-001, DRV-003 | Contributes | No reliance on provider behavior |

## Driver Tensions

| Related driver IDs | Evidence and competing effects | Affected decision | Alternatives or missing evidence | Question IDs | Validation required |
| --- | --- | --- | --- | --- | --- |
| DRV-006 vs DRV-001, DRV-003 | Deeper inline detection improves signal but adds latency on every call | Detector placement (DEC-012) | Inline structural only; asynchronous ML; sampled detection; latency target missing | Q-004, Q-007 | Benchmark and signal-quality comparison |
| DRV-005 vs DRV-002 | Out-of-process isolation resists host compromise but breaks install-and-attach simplicity | Deployment form (DEC-001, DEC-011) | Library only; library plus optional sidecar; gateway only | Q-001 | Threat-model decision |
| DRV-009 vs NFR-004 | Richer audit aids investigation but increases leakage surface | Event schema and redaction defaults (DEC-008) | Summaries only; full payload with redaction; host-configurable | Q-008 | Redaction tests |
| DRV-011, DRV-013 vs ASM-008 | Pins, approvals and counters need state; state adds operational burden and tamper surface | State store form and scope | Per-process file; host-supplied store; stateless re-pin each session | Q-012 | Tamper and recovery tests |
| DRV-008 vs adoption (ASM-003) | Fail-closed retrieval protects tenants but blocks hosts lacking an identity contract | Retrieval guard behavior (DEC-007) | Fail closed; verify-presence-only mode; opt-out with audit | Q-010 | Adopter validation |

## Risks

Owner is Unassigned for every entry; no acceptance evidence exists.

| Risk ID | Description | Likelihood | Impact | Severity | Mitigation | Owner, if supplied | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| RISK-001 | Host integrates the package partially, leaving unguarded tool or model call sites (affects CON-003, FR-020; DRV-005) | Likely | High | High | Conformance check for unguarded call sites; adapters that wrap at framework level; documentation of the cooperative boundary | Unassigned | Mitigation proposed |
| RISK-002 | Adopters treat advisory detection as the security boundary (BR-001; DRV-003) | Possible | High | High | Detectors expose only signals; policy API requires deterministic rules for consequential actions; naming and docs | Unassigned | Mitigation proposed |
| RISK-003 | Latency overhead hampers adoption (NFR-001; DRV-006) | Possible | Medium | Medium | Inline structural checks only; asynchronous detectors; benchmarks published | Unassigned | Mitigation proposed |
| RISK-004 | Over-permissive policy misconfiguration (FR-002, BR-002) | Likely | High | High | Default deny; policy lint; dry-run parity; combination report highlighting trifecta exposure | Unassigned | Mitigation proposed |
| RISK-005 | Credential material leaks through tool results, exceptions or events despite brokering (FR-009, NFR-004) | Possible | High | High | Redaction of known credential shapes; canary secrets in tests; broker performs the outbound call itself | Unassigned | Mitigation proposed |
| RISK-006 | Framework adapter drift when upstream libraries change (FR-020) | Likely | Medium | Medium | Adapters as separately versioned extras; conformance suite per framework version | Unassigned | Mitigation proposed |
| RISK-007 | The package becomes a supply-chain target (NFR-002; DRV-007) | Possible | Critical | High | Signed releases, SBOM, minimal dependencies, reproducible builds, no default network egress | Unassigned | Mitigation proposed |
| RISK-008 | Host-supplied ACL or tenant metadata is missing or wrong, so retrieval authorization is ineffective (FR-012; ASM-003) | Possible | High | High | Fail closed on missing metadata; conformance tests; document that ACL truth is the host's | Unassigned | Mitigation proposed |
| RISK-009 | Approval fatigue leads approvers to accept everything (FR-005; DRV-013) | Likely | Medium | Medium | Show provenance and diff; rate of approvals surfaced in audit; policy tuning via dry-run | Unassigned | Mitigation proposed |
| RISK-010 | Incomplete redaction places sensitive data in audit events (FR-017, NFR-004) | Possible | Medium | Medium | Summaries by default; configurable patterns; redaction tests | Unassigned | Mitigation proposed |
| RISK-011 | Context leaks across asyncio tasks or thread pools (FR-021, NFR-009) | Possible | High | High | Context bound per request via context variables; explicit copy into executors; fail closed when absent | Unassigned | Mitigation proposed |
| RISK-012 | Local state (pins, approvals, counters) is tampered with, hiding drift or replaying approvals (FR-005, FR-006) | Possible | Medium | Medium | Integrity hash on state; read-only deployment option; audit of state changes | Unassigned | Mitigation proposed |

## Assumptions

Inherited unchanged from the baseline: ASM-001–ASM-008 (see [01-requirements-baseline.md](01-requirements-baseline.md)). No assumption was validated or added at this stage.

## Clarification Ledger

Inherited unchanged: active batch Q-001–Q-007; queued Q-008–Q-014 (see [01-requirements-baseline.md](01-requirements-baseline.md)). No new question; existing questions cover every decision-changing gap identified above.

## Validation and Handoff Gates

| Affected IDs | Required evidence or check | Decision gated | Responsible owner, if supplied | Current result | Next consumer |
| --- | --- | --- | --- | --- | --- |
| DRV-005, DRV-004, ASM-002 | Answer to Q-001 (threat-model boundary) | DEC-011 status | Requesting user | Not run — Open | architecture-style-selector |
| DRV-006, FR-011 | Latency target (Q-004) and benchmark | DEC-012 detector placement | Unassigned | Not run — Unresolved | architecture-style-selector |
| DRV-002, FR-020 | Framework list (Q-002) | Adapter scope | Requesting user | Not run — Open | high-level-design-generator |
| DRV-008, FR-012 | Adopter confirmation of identity/ACL contract (ASM-003) | DEC-007 retrieval behavior | Unassigned | Not run | high-level-design-generator |
| DRV-007, NFR-002 | Signing authority (Q-006), SBOM and egress test | DEC-010 evidence | Unassigned | Not run | high-level-design-generator |
| All DRV | Style comparison against the eleven styles | Composition | architecture-style-selector | Pending | architecture-style-selector |
