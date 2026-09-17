# Requirements Baseline — Asphaleia (AI-Application Security Package)

## Handoff

- Design ID: ASPHALEIA (proposed; derived from the workspace name).
- Contract version: 1.0.0.
- Artifact: requirement-analyzer — requirements baseline for an installable Python security package for AI-enabled applications.
- Artifact status: Provisional — extraction is complete for the stated scope; requirement priorities were not supplied (Unspecified), numeric NFR targets are Unresolved, and the threat-model boundary (Q-001) is open. Driver analysis may proceed.
- Baseline: SRC-001/SRC-002 conversation turns of 2026-09-16; SRC-007 [00-concern-isolation.md](00-concern-isolation.md); no prior version.
- Evidence summary: Confirmed — CON-001, CON-004 scope framing, BR-001 caveat; Inferred — FR-001–FR-022, BR-002–BR-004, NFR-002–NFR-010, CON-002/003/005, actor and external-system roles; Assumed — ASM-001–ASM-008; Proposed — data classifications, release tiering; Unresolved — all numeric targets, priorities, Q-001–Q-014.
- Changed IDs: initial allocation of SRC-001–SRC-007, FR-001–FR-022, BR-001–BR-004, NFR-001–NFR-010, CON-001–CON-005, ACT-001–ACT-005, EXT-001–EXT-010, TB-001–TB-007, DATA-001–DATA-010, ASM-001–ASM-008, Q-001–Q-014.
- Open questions: active batch Q-001–Q-007 (Important: Q-001–Q-005; Optional: Q-006, Q-007); queued Q-008–Q-014. No Critical question; no decision is blocked.
- Validation: source disposition audit performed manually against SRC-001, SRC-002 and SRC-007; ID uniqueness checked by reading; no tooling run. Stakeholder gate: user validation of the concern classification (SRC-007) and of ASM-006/ASM-007.
- Next handoff: `architecture-driver-analyzer` receives this full baseline ([02-architecture-drivers.md](02-architecture-drivers.md)).

## Objective and Scope

- Objective: provide an installable Python package that lets developers of LLM-powered and agentic applications enforce, inside their application process, the package-addressable subset of AI-SaaS security concerns: deterministic policy on tool actions and data movement, credential isolation, tenant-aware retrieval, safe output handling, consumption limits, and auditable decisions (SRC-001; SRC-007 §4.1).
- Supplied success criteria: none numeric. Qualitative (Inferred from SRC-001): every Addressable/Partial row in SRC-007 maps to a capability; the package attaches to an existing application without rewrite ("can be installed").
- In scope: capabilities FR-001–FR-022 and rules BR-001–BR-004 under constraints CON-001–CON-005.
- Out of scope (confirmed by scope framing, CON-004): OS-level sandboxing, network egress enforcement independent of the process, vendor contracts and training opt-out, shadow-AI discovery, source-system permission hygiene, model provenance/robustness, regulatory assessment, factual correctness. Proposed deferrals (not exclusions): out-of-process strict mode (DEC-011), bundled ML detector (DEC-012).
- Source references: SRC-001, SRC-002, SRC-007.
- Unresolved boundaries: host-compromise threat model (Q-001); first-release framework coverage (Q-002); multi-process deployments of one logical application (Q-012).

## Sources

| Source ID | Description | Locator | Authority | Access status |
| --- | --- | --- | --- | --- |
| SRC-001 | User request: isolate package-addressable concerns, create a high-level design, use available skills | Conversation, 2026-09-16, second user turn | Authorized user input (requester); approval authority not stated | Available |
| SRC-002 | Concern inventory: research summary delivered in the first assistant turn and adopted by the user as the basis for SRC-001 | Conversation, 2026-09-16, first assistant turn | User-adopted brief; content synthesized from SRC-003–SRC-006 | Available |
| SRC-003 | OWASP Top 10 for LLM Applications 2025 (LLM01–LLM10) | genai.owasp.org/llm-top-10/ | Public guidance; not a business requirement | Fetched 2026-09-16 (landing page and risk titles) |
| SRC-004 | OWASP Agentic AI Threats and Mitigations (Feb 2025); OWASP Top 10 for Agentic Applications 2026 (Dec 2025) | genai.owasp.org resources | Public guidance | Partially fetched (abstracts only; full text not retrieved) |
| SRC-005 | Willison, "The lethal trifecta for AI agents" (June 2025) and linked exploit series | simonwillison.net/2025/Jun/16/the-lethal-trifecta/ | Public guidance and incident summaries | Fetched |
| SRC-006 | Hacker News discussions of incidents and agent-security tooling (Slack AI exfiltration, EchoLeak, GitLab Duo, MCP attack vectors, MCP-Shield, Agent Vault, Golf Scanner, FireClaw, Aegis.rs, nono) | hn.algolia.com search results | Public practitioner discussion; evidence of concern, not of requirement | Fetched (titles and submission text) |
| SRC-007 | Concern isolation analysis | [00-concern-isolation.md](00-concern-isolation.md) | Derived analysis (Inferred); user validation pending | Available |

## Structured Requirements

Priority is Unspecified throughout because SRC-001 supplies no prioritization (queued Q-009). Proposed release tiering is recorded in ASM-007, not in this column.

| Requirement ID | Category | Requirement statement | Source | Priority | Status | Confidence | Architecture impact | Assumption or clarification required |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | Functional | Label every segment entering a model-invocation context with its provenance (system, developer, end user, retrieved document, tool result, memory, external message) and a trust level, and propagate labels for the life of the session, including across in-process agent hops. | SRC-002 §1 row 1; SRC-005; SRC-007 rows 1, 8, 15, 17 | Unspecified | Inferred | Medium | High — foundation for policy, tenant and exfiltration controls | ASM-002 |
| FR-002 | Functional | Evaluate a declarative capability policy before every tool, function or MCP invocation and return allow, deny or require-approval; unregistered tools and undeclared capabilities are denied. | SRC-002 §2; SRC-003 LLM06; SRC-007 rows 6, 11 | Unspecified | Inferred | Medium | High — central enforcement point | BR-002 |
| FR-003 | Functional | When the session context holds content labeled untrusted, deny or route to approval any tool classed as external-communication or consequential-write, per policy (lethal-trifecta rule). | SRC-005; SRC-007 row 1 | Unspecified | Inferred | High (pattern is explicit in SRC-005) | High — defines the exfiltration boundary | Capability classes are Proposed defaults |
| FR-004 | Functional | Validate tool-call arguments against the declared input schema and policy constraints (host/URL allow-lists, path constraints, size limits, value allow-lists) before execution and reject non-conforming calls. | SRC-002 §2; SRC-003 LLM06, LLM09; SRC-007 rows 6, 9, 11 | Unspecified | Inferred | Medium | Medium — bounded interface | None |
| FR-005 | Functional | Provide a human approval gate for policy-designated actions; the approval binds to a hash of tool, arguments, principal and provenance summary, is single-use, expires, and presents the provenance of the instruction that triggered the action. | SRC-004 (human-trust exploitation); SRC-007 rows 6, 19 | Unspecified | Inferred | Medium | Medium — asynchronous human step | Q-005 |
| FR-006 | Functional | Register tools and MCP servers by pinning a hash of name, description and input schema; on later sessions detect drift and block the tool until re-approved. | SRC-002 §2 (rug-pull); SRC-006 (MCP-Shield, Golf Scanner); SRC-007 row 13 | Unspecified | Inferred | Medium | Medium — needs persistent local state | ASM-008 |
| FR-007 | Functional | Scan tool and MCP manifests (descriptions, parameter descriptions) for hidden-instruction patterns and report or deny per policy. | SRC-002 §2 (tool poisoning); SRC-006; SRC-007 row 13 | Unspecified | Inferred | Medium | Low — bounded, advisory plus policy | BR-001 |
| FR-008 | Functional | Analyze the registered tool set for combinations that jointly provide private-data access, untrusted-content exposure and external communication, and report or enforce a policy outcome. | SRC-005; SRC-007 row 13 | Unspecified | Inferred | Medium | Medium — informs default policy | None |
| FR-009 | Functional | Broker credentials: prompts and tools receive opaque handles; the broker resolves a handle and injects the credential only into the outbound call it performs; raw credentials never enter model context, tool results, audit events or logs. | SRC-002 §2 (credential exfiltration); SRC-006 (Agent Vault); SRC-007 row 12 | Unspecified | Inferred | Medium | High — separate trust boundary and egress path | ASM-002, Q-001 |
| FR-010 | Functional | Structurally sanitize untrusted content before it enters context: remove or flag hidden text, invisible and zero-width characters, mixed-script homoglyphs and encoding obfuscation; enforce size limits; preserve provenance labels. | SRC-002 §1; SRC-006 (FireClaw); SRC-007 row 1 | Unspecified | Inferred | Medium | Low — bounded transformation | None |
| FR-011 | Functional | Provide pluggable advisory detectors for injection and jailbreak patterns that emit a signal (score, reasons) consumable by policy; a detector is never the sole basis for allowing an action. | SRC-002 §1; SRC-005 (guardrail caveat); SRC-007 row 1 | Unspecified | Inferred | High for the caveat, Medium for the mechanism | Medium — extension point and latency | BR-001, Q-007 |
| FR-012 | Functional | Enforce retrieval authorization: attach principal and tenant filters to retrieval queries, post-filter results against host-supplied ACL metadata before they enter context, and fail closed when identity or ACL metadata is missing. | SRC-002 §1 LLM08, §3 oversharing; SRC-007 rows 2, 8, 25, 26 | Unspecified | Inferred | Medium | High — depends on the host identity/ACL contract | ASM-003 |
| FR-013 | Functional | Gate long-term memory and instruction writes: content derived from untrusted-labeled sources is not persisted without a policy decision; memory reads carry provenance labels. | SRC-004 (memory poisoning); SRC-007 rows 4, 15 | Unspecified | Inferred | Medium | Medium | None |
| FR-014 | Functional | Guard model output per declared sink: sanitize or encode for HTML and markdown, parameterize for SQL and shell sinks, block URLs, images and links not on an allow-list, and validate structured outputs against the declared schema. | SRC-002 §1 LLM05; SRC-005 (exfiltration channels); SRC-007 rows 1, 5 | Unspecified | Inferred | Medium | Medium | None |
| FR-015 | Functional | Redact secrets and configured sensitive patterns from model outputs, tool results and audit events, and detect verbatim echo of system-prompt canary tokens. | SRC-002 §1 LLM02, LLM07; SRC-007 rows 2, 7 | Unspecified | Inferred | Medium | Medium | None |
| FR-016 | Functional | Enforce budgets per request, session and tenant for tokens, estimated cost, wall time, tool calls and agent iterations; detect loops; terminate deterministically with an auditable reason. | SRC-002 §1 LLM10; SRC-007 row 10 | Unspecified | Inferred | Medium | Medium — cross-cutting accounting | Numeric defaults Unresolved (Q-008) |
| FR-017 | Functional | Emit structured, redacted security events (context-assembly summary, policy decisions, tool calls and result summaries, approvals, budget events, detections, drift blocks) with correlation identifiers to a pluggable sink. | SRC-004 (insufficient logging); SRC-002 §3; SRC-007 rows 18, 27 | Unspecified | Inferred | Medium | Medium | Q-008 |
| FR-018 | Functional | Guard provider egress: allow-list provider endpoints and model identifiers, pin models and alert on change, apply minimization and redaction transforms, and set provider request options that restrict retention or training where the API exposes them. | SRC-002 §3; SRC-007 rows 22, 23, 29 | Unspecified | Inferred | Medium | Medium | Provider option coverage Unresolved |
| FR-019 | Functional | Provide a command-line tool that inventories the application's tool/MCP configuration and policy files, detects hard-coded credentials and insecure server configuration, lints policies and reports findings. | SRC-002 §2; SRC-006 (Golf Scanner); SRC-007 row 14 | Unspecified | Inferred | Medium | Low — offline utility | None |
| FR-020 | Functional | Attach enforcement through adapters (wrappers, middleware, hooks) for common Python agent frameworks, provider SDKs, the MCP client and HTTP frameworks without requiring application rewrite. | SRC-001 ("can be installed"); SRC-007 §4.1 item 13 | Unspecified | Inferred | Medium | High — drives ports-and-adapters organization | Q-002 |
| FR-021 | Functional | Propagate principal and tenant context across synchronous and asynchronous execution within the process and fail closed when a guarded operation runs without it. | SRC-002 §3 (tenant isolation); SRC-007 row 26 | Unspecified | Inferred | Medium | High — correctness under concurrency | None |
| FR-022 | Functional | Provide a dry-run mode that records would-be decisions without enforcing them, for staged rollout and policy tuning. | SRC-007 §4.1 item 12 (rollout need Inferred) | Unspecified | Inferred | Low | Low | None |
| BR-001 | Business rule | An advisory detector result is never the sole basis for allowing a consequential action; a deterministic policy decision is required. | SRC-005 (detection rates are not a boundary); SRC-002 §1 | Unspecified | Confirmed (explicit in adopted brief) | High | High | None |
| BR-002 | Business rule | The default outcome is deny for unregistered tools, undeclared capabilities, unknown output sinks and guarded operations lacking identity or tenant context. | SRC-007 §1; SRC-003 LLM06 | Unspecified | Inferred | Medium | High | None |
| BR-003 | Business rule | Approvals are single-use, bound to the exact call content and expire. | SRC-004; SRC-007 row 19 | Unspecified | Inferred | Medium | Medium | Expiry duration Unresolved |
| BR-004 | Business rule | The package transmits no data outside the host process on its own initiative; every outbound path is an explicitly configured adapter. | SRC-006 (objection to third-party routing, Aegis.rs and LLMSafe threads); SRC-002 §3 | Unspecified | Inferred | Medium | High — no phone-home | None |
| NFR-001 | Non-functional | Enforcement hooks add low latency on the model-call and tool-call path. Numeric budget: Unresolved. | SRC-006 (latency emphasis); SRC-007 | Unspecified | Unresolved (target) | Low | High — constrains synchronous detector design | Q-004, ASM-004 |
| NFR-002 | Non-functional | The package minimizes mandatory dependencies, publishes signed releases with a software bill of materials, pins optional extras and builds reproducibly. | SRC-003 LLM03; SRC-007 row 3 | Unspecified | Inferred | Medium | Medium | Q-006 |
| NFR-003 | Non-functional | Internal errors on an enforcement path fail closed (deny) and are audited; fail-open is possible only by explicit per-policy configuration. | SRC-007 §1; SRC-005 | Unspecified | Inferred | Medium | High | None |
| NFR-004 | Non-functional | Credentials, personal data and raw untrusted content do not appear in package logs, exceptions or telemetry. | SRC-002 §3; SRC-003 LLM02 | Unspecified | Inferred | Medium | Medium | None |
| NFR-005 | Non-functional | Supports synchronous and asyncio execution across the supported Python version range. | SRC-001; SRC-006 (async-heavy frameworks) | Unspecified | Inferred | Medium | Medium | ASM-001, Q-003 |
| NFR-006 | Non-functional | Policies and guards are unit-testable; the package ships a conformance suite and an attack-corpus regression suite for detectors and guards. | SRC-005 (non-determinism); SRC-007 | Unspecified | Inferred | Medium | Medium | None |
| NFR-007 | Non-functional | Extension only through documented ports: detectors, secret stores, audit sinks, approval channels, retrieval adapters, sandbox runners. | SRC-007 §1 | Unspecified | Inferred | Medium | High — ports and adapters | None |
| NFR-008 | Non-functional | Policy evaluation is deterministic and reproducible for identical inputs; policies are versioned and every decision records the policy version. | SRC-005; BR-001 | Unspecified | Inferred | Medium | Medium | None |
| NFR-009 | Non-functional | Context propagation is safe under threads and asyncio tasks; no cross-request or cross-tenant leakage of principal, tenant, provenance or budgets. | SRC-002 §3 | Unspecified | Inferred | Medium | High | None |
| NFR-010 | Non-functional | Policy and event schemas evolve backward-compatibly with explicit versions. | SRC-007 §4.1; NFR-008 | Unspecified | Inferred | Low | Low | None |
| CON-001 | Constraint | The deliverable is an installable Python package distributed through a package index. | SRC-001 | Unspecified | Confirmed | High | High — in-process library topology | None |
| CON-002 | Constraint | The design is neutral to LLM provider, agent framework, vector store, secret store and log sink; no vendor is mandated. | SRC-001 ("any other similar products") | Unspecified | Inferred | Medium | High | None |
| CON-003 | Constraint | The package executes inside the host application process and cannot enforce controls against the host code itself. | Consequence of CON-001; SRC-007 §1 | Unspecified | Inferred | High | High — cooperative boundary | Q-001, ASM-002 |
| CON-004 | Constraint | Concerns whose control point lies outside the process (OS sandboxing, independent network egress enforcement, vendor contracts, shadow AI, regulatory assessment, model training) are out of scope; the package exposes integration points or evidence only. | SRC-001 (scope framing); SRC-007 §4.2 | Unspecified | Confirmed framing; boundary Inferred | Medium | High | None |
| CON-005 | Constraint | The design must not depend on model-provider behavior guarantees such as instruction-hierarchy robustness. | SRC-005 | Unspecified | Inferred | High | High | None |

## Stakeholders

| Stakeholder | Interest or responsibility | Decision authority, if supplied | Source | Related requirement IDs |
| --- | --- | --- | --- | --- |
| Requesting user | Defines scope: isolation analysis and high-level design | Requests the design; approval authority not stated | SRC-001 | All |
| Application development teams (adopters) | Integrate with minimal rewrite; predictable latency | Not supplied | SRC-001 (Inferred) | FR-020, NFR-001, NFR-005 |
| Security engineering (policy authors, reviewers) | Author and version policy; consume findings and audit | Not supplied | SRC-002 (Inferred) | FR-002, FR-017, FR-019, FR-022 |
| End users of the AI application (data subjects) | Their data and permissions respected by the assistant | Not applicable | SRC-002 (Inferred) | FR-012, FR-015, FR-021 |
| Platform, legal and compliance functions | Own the out-of-scope controls the package hands off to | Not supplied | SRC-007 §4.2 | CON-004 |

## Actors and Systems

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs |
| --- | --- | --- | --- | --- | --- |
| ACT-001 | Actor | Application developer | Integrates the package, declares tools, sinks and identity binding | SRC-001; Inferred | FR-020, FR-004, FR-014 |
| ACT-002 | Actor | Policy author | Writes, versions and tests policy; reviews scanner findings | SRC-002; Inferred | FR-002, FR-019, FR-022 |
| ACT-003 | Actor | End user (principal) | Person on whose behalf the application acts; identity and tenant supplied by the host | SRC-002; Inferred | FR-012, FR-021 |
| ACT-004 | Actor | Approver | Human who approves or rejects gated actions | SRC-004; Inferred | FR-005, BR-003 |
| ACT-005 | Actor | Operator | Runs the scanner, consumes audit events, tunes budgets | SRC-002 §3; Inferred | FR-016, FR-017, FR-019 |
| EXT-001 | External system | Host application and agent framework | The Python application that imports and runs the package in-process | SRC-001; Confirmed existence | FR-020, CON-003 |
| EXT-002 | External system | LLM provider | Model inference endpoint(s), any vendor | SRC-002; Confirmed existence, vendor Unresolved | FR-018, CON-002 |
| EXT-003 | External system | Tools and MCP servers | Local functions, remote MCP servers and external APIs invoked on the agent's behalf | SRC-002 §2; Confirmed class | FR-002–FR-009 |
| EXT-004 | External system | Retrieval and memory stores | Vector stores, search indexes, long-term memory | SRC-002 §1 LLM08; Confirmed class | FR-012, FR-013 |
| EXT-005 | External system | Secret store | Environment, vault or key service holding real credentials | SRC-002 §2; Inferred | FR-009 |
| EXT-006 | External system | Audit sink | Log pipeline or SIEM receiving security events | SRC-002 §3; Inferred | FR-017 |
| EXT-007 | External system | Package distribution registry | Index from which the package is installed | CON-001; Inferred | NFR-002 |
| EXT-008 | External system | Untrusted content sources | Web pages, emails, documents, tickets and messages that reach the model via EXT-003/EXT-004 | SRC-005; Confirmed class | FR-001, FR-003, FR-010 |
| EXT-009 | External system | Approval channel | Console, chat, ticket or UI through which approvers respond | FR-005; Inferred, mechanism Unresolved (Q-005) | FR-005 |
| EXT-010 | External system | Sandbox runner | OS or container isolation facility for code and shell tools; out of scope, integration point only | SRC-006 (nono); CON-004 | FR-002 |
| TB-001 | Trust boundary | Untrusted content to model context | Content from EXT-008 (via EXT-003/EXT-004) becomes model input | SRC-005; Inferred | FR-001, FR-010, FR-011 |
| TB-002 | Trust boundary | Model output to application sinks | Model text becomes UI, database, shell or tool input | SRC-003 LLM05; Inferred | FR-014, FR-015 |
| TB-003 | Trust boundary | Agent to tool execution | A proposed tool call becomes an executed action | SRC-003 LLM06; Inferred | FR-002–FR-005 |
| TB-004 | Trust boundary | Credential boundary | Real credentials exist only between EXT-005 and the outbound call; never in model context | SRC-002 §2; Inferred | FR-009 |
| TB-005 | Trust boundary | Principal and tenant boundary | Per-request identity governs retrieval, memory, budgets and audit attribution | SRC-002 §3; Inferred | FR-012, FR-021 |
| TB-006 | Trust boundary | Application to provider | Application data leaves the process toward EXT-002 | SRC-002 §3; Inferred | FR-018 |
| TB-007 | Trust boundary | Host to package (cooperative) | The host chooses what to route through the package; the package cannot enforce against the host | CON-003; Inferred | FR-020, CON-003 |

## Data Inventory

| Entity ID | Kind | Name | Responsibility or meaning | Source or evidence class | Related requirement IDs | Classification | Classification status | Owner, if supplied | Collection purpose | Retention/deletion state | Geographic constraints |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DATA-001 | Data entity | Context segments | Prompt text with provenance labels; may contain personal or customer data | SRC-002; Inferred | FR-001, FR-010, FR-012 | Confidential | Proposed | Host application | Model invocation | Transient in process; only redacted summaries persist in audit (Proposed) | Follows host deployment; Unresolved |
| DATA-002 | Data entity | Tool manifests and pins | Name, description, schema and their hash | SRC-002 §2; Inferred | FR-006, FR-007 | Internal | Proposed | Package local state | Drift detection | Until tool removed; Unresolved | Unresolved |
| DATA-003 | Data entity | Policy documents | Versioned declarative policy | FR-002; Inferred | FR-002, FR-022, NFR-008 | Confidential (reveals controls) | Proposed | Policy author | Enforcement | Versioned history; Unresolved | Unresolved |
| DATA-004 | Data entity | Credentials | Real secrets resolved from EXT-005; handles are non-secret identifiers | SRC-002 §2; Inferred | FR-009 | Secret | Proposed | Secret store / host | Outbound tool and provider calls | In memory only for the duration of the call; never persisted by the package | Not applicable to the package |
| DATA-005 | Data entity | Audit events | Redacted, structured security events with correlation identifiers | FR-017; Inferred | FR-017, NFR-004 | Confidential | Proposed | Host / security function | Traceability and investigation | Unresolved (Q-008) | Follows sink placement; Unresolved (Q-013) |
| DATA-006 | Data entity | Budget counters | Per request, session and tenant consumption | FR-016; Inferred | FR-016 | Internal | Proposed | Package local state | Consumption control | Transient or persisted per policy; Unresolved | Unresolved |
| DATA-007 | Data entity | Approval records | Call hash, approver identity, expiry, outcome | FR-005; Inferred | FR-005, BR-003 | Confidential (approver identity) | Proposed | Package local state / host | Single-use approval enforcement | Until expiry plus audit copy; Unresolved | Unresolved |
| DATA-008 | Data entity | Detection signals | Scores and reasons from advisory detectors | FR-011; Inferred | FR-011, BR-001 | Internal | Proposed | Package (transient) | Policy input and audit summary | Transient | Not applicable |
| DATA-009 | Data entity | Retrieval ACL and tenant metadata | Host-supplied authorization attributes on stored content | FR-012; Inferred | FR-012, FR-021 | Follows source content | Unresolved | Host / retrieval store | Retrieval filtering | Owned outside the package | Follows host |
| DATA-010 | Data entity | Memory entries | Long-term agent memory with provenance labels | FR-013; Inferred | FR-013 | Confidential | Proposed | Host | Agent memory | Host policy; write gated by package | Follows host |

## Integration Inventory

| External system ID | Direction relative to subject | Business purpose | Data exchanged | Source or evidence class | Related requirement IDs | Open questions |
| --- | --- | --- | --- | --- | --- | --- |
| EXT-001 | Bidirectional | Host invokes package hooks; package calls host-registered adapters and callbacks | DATA-001, DATA-003, DATA-006, DATA-009 | SRC-001; Inferred | FR-020, FR-021, CON-003 | Q-002, Q-003 |
| EXT-002 | Downstream | Guarded model invocations | DATA-001 (minimized and redacted) | SRC-002; Confirmed class | FR-018 | Provider option coverage |
| EXT-003 | Downstream (calls), Upstream (results) | Guarded tool execution; results return as untrusted content | Tool arguments, DATA-004 injected at egress only, results as DATA-001 | SRC-002 §2; Confirmed class | FR-002–FR-009 | None |
| EXT-004 | Downstream (queries), Upstream (results) | Authorized retrieval and gated memory writes | DATA-001, DATA-009, DATA-010 | SRC-002 §1; Confirmed class | FR-012, FR-013 | ASM-003 |
| EXT-005 | Upstream | Credential resolution for the broker | DATA-004 | SRC-002 §2; Inferred | FR-009 | Q-001 |
| EXT-006 | Downstream | Audit event delivery | DATA-005 | SRC-002 §3; Inferred | FR-017 | Q-008, Q-013 |
| EXT-007 | Upstream | Package installation and updates | Package artifact and signatures | CON-001; Inferred | NFR-002 | Q-006 |
| EXT-008 | Upstream (indirect) | Untrusted content entering via tools and retrieval | DATA-001 | SRC-005; Confirmed class | FR-001, FR-003, FR-010 | None |
| EXT-009 | Bidirectional | Approval request and response | DATA-007 | FR-005; Inferred | FR-005 | Q-005 |
| EXT-010 | Downstream | Sandboxed execution of code/shell tools when policy requires it | Tool arguments and results | CON-004; Inferred | FR-002 | None (out of scope beyond the port) |

## Assumptions and Constraints

Constraints CON-001–CON-005 and rules BR-001–BR-004 are listed in Structured Requirements.

| Assumption ID | Assumption | Reason | Consequence if false | Validation required | Status | Related IDs |
| --- | --- | --- | --- | --- | --- | --- |
| ASM-001 | Supported Python floor is 3.10 (planning value); commitment Unresolved | Context variables and typing features used by the design | Adapter and typing changes; fallback is to raise the floor or add compatibility shims | Q-003 answer | Proposed | NFR-005, Q-003 |
| ASM-002 | The host process is cooperative and not compromised; the attacker controls untrusted content and possibly tool servers | An in-process library cannot defend against its own host | In-process controls become bypassable; fallback is out-of-process strict mode (DEC-011) | Q-001 answer; threat-model review | Proposed | CON-003, FR-009, Q-001 |
| ASM-003 | The host can supply principal and tenant identity and retrieval ACL metadata | Retrieval authorization needs an authority for entitlements | Retrieval guard fails closed, reducing utility; fallback is host-side pre-filtering with the package verifying presence only | Adopter interviews; conformance tests | Proposed | FR-012, FR-021, DATA-009 |
| ASM-004 | Low single-digit millisecond overhead per hook, excluding optional ML detectors, is an acceptable planning envelope | No latency target supplied | Detector placement and caching strategy change; fallback is asynchronous or sampled detection | Q-004 answer; benchmark | Proposed | NFR-001, Q-004 |
| ASM-005 | Multi-tenant host applications are in scope | Tenant isolation is a listed concern in SRC-002 | Tenant features become optional simplifications | Q-010 (queued) | Proposed | FR-021, FR-012 |
| ASM-006 | SRC-002 is an authorized concern inventory for this design | The user built SRC-001 on it | Requirement set changes where the user disputes rows | User review of SRC-007 | Proposed | SRC-002, SRC-007 |
| ASM-007 | Proposed release tiering: SRC-007 §4.1 items 1, 2, 3, 5, 8, 9, 10, 13 first; items 4, 6, 7, 11, 12 later | No priorities supplied; enforcement core before advisory and tooling | Scope reorders; no architectural change expected | Q-009 (queued) | Proposed | All FR |
| ASM-008 | A small pluggable local state (pins, approvals, counters) with a file or embedded default is acceptable | Drift detection and single-use approvals need persistence | Pinning must be host-provided or stateless; fallback is a host-supplied store adapter | Adopter feedback | Proposed | FR-005, FR-006, FR-016, DATA-002, DATA-006, DATA-007 |

## Ambiguities and Conflicts

| Question ID | Kind | Related IDs | Source alternatives | Interpretations or conflict | Affected decision | Resolution evidence or next owner |
| --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Ambiguity | CON-003, FR-009, ASM-002 | SRC-001 silent; SRC-006 tooling threads split between in-process and proxy designs | (a) host compromise excluded — in-process library suffices; (b) included — out-of-process broker or gateway required | DEC-001 topology, DEC-011 strict mode | Requesting user |
| Q-002 | Ambiguity | FR-020, CON-002 | SRC-001 "can be installed"; SRC-006 names many frameworks | Which frameworks constitute "installed without rewrite" for the first release | Adapter scope (CMP-012) | Requesting user |
| — | Conflict | None | — | No source-backed conflict identified; the latency-versus-detection tension is recorded as a driver tension, not a conflict | — | — |

## Architecture-Driving Gaps

| Concern | Related requirement IDs | Missing attribute | Architectural consequence | Question ID |
| --- | --- | --- | --- | --- |
| Workload and latency | NFR-001, FR-011 | Calls per second, context sizes, latency budget and percentile | Determines synchronous versus asynchronous detector placement and caching | Q-004, Q-011 (queued) |
| Availability | NFR-003 | Behavior when adapters' external dependencies (secret store, audit sink, approval channel) are unavailable | Fail-closed semantics and buffering decisions | Q-008 (queued) |
| Recovery | FR-006, FR-005 | Recovery expectations for local state (pins, approvals) | Whether loss of local state must block or re-pin | None new; covered by ASM-008 |
| Consistency | FR-005, FR-016 | Single-use approvals and budgets across multiple processes of one application | Shared store or per-process scope | Q-012 (queued) |
| Security and privacy | FR-012, FR-015 | Identity and ACL contract; sensitive-pattern policy | Fail-closed behavior and redaction defaults | ASM-003, Q-008 (queued) |
| Compliance and residency | FR-017 | Audit sink placement and retention | Sink adapter behavior and buffering | Q-013 (queued) |
| Integration | FR-020, FR-018 | Framework list; provider request-option coverage | Adapter count and provider-specific transforms | Q-002 |
| Deployment and operations | NFR-002, FR-019 | Publishing and signing authority; support ownership | Release pipeline evidence; ownership gaps | Q-006, Q-014 (queued) |

## Clarification Ledger

### Active Batch

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-001 | Important | Is compromise of the host application process inside the threat model the package must defend against? | Decides whether in-process enforcement is sufficient or an out-of-process broker/gateway is mandatory | Excluded; included; included for credentials only | Excluded (ASM-002); strict mode stays a Deferred alternative | DEC-011 final status | Open | CON-003, FR-009 |
| Q-002 | Important | Which frameworks and SDKs must the first release attach to without application rewrite? | Sizes the adapter layer and conformance suite | MCP client and provider HTTP clients only; plus LangChain/LlamaIndex; plus OpenAI Agents/Anthropic SDKs; other | Framework-neutral core plus MCP client, provider HTTP and generic function-tool adapters | CMP-012 scope | Open | FR-020 |
| Q-003 | Important | What is the minimum Python version and must both sync and asyncio be supported? | Affects context propagation and typing design | 3.10+; 3.11+; 3.12+; sync only; both | 3.10+, both (ASM-001) | NFR-005 detail | Open | NFR-005 |
| Q-004 | Important | What latency overhead per model and tool call is acceptable, at which percentile? | Decides whether detectors run inline, asynchronously or sampled | Under 1 ms; under 5 ms; under 20 ms; any with asynchronous detectors | Unresolved numeric; ASM-004 planning envelope | DEC-012 detector placement | Open | NFR-001, FR-011 |
| Q-005 | Important | Through which channel do approvers receive and answer approval requests, and who may approve? | Defines the approval port and record binding | Console; chat integration; HTTP callback; host UI | Port with callback and console adapters; approver role supplied by host | INT-007 detail | Open | FR-005 |
| Q-006 | Optional | Who publishes releases and holds signing keys? | Evidence for the supply-chain posture | Named team; automated pipeline identity; Unresolved | Unresolved; DEC-010 proposes signed releases | NFR-002 evidence | Open | NFR-002 |
| Q-007 | Optional | Is a bundled ML injection detector acceptable, or must it be an optional extra? | Dependency footprint and latency | Bundled; optional extra; none | Optional extra (DEC-012 Deferred) | Dependency footprint | Open | FR-011 |

### Queued Questions

| Question ID | Priority | Question | Why it matters | Answer options | Default assumption | Blocks | Status | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q-008 | Important | What audit retention and default budget values apply? | Numeric policy defaults | Supplied values; host-defined; none | Unresolved; no defaults committed | FR-016, FR-017 numeric fields | Open | FR-016, FR-017 |
| Q-009 | Important | What priority (Must/Should/Could) does each requirement carry? | Release scoping | MoSCoW per ID | ASM-007 tiering | Release plan | Open | All |
| Q-010 | Optional | Is multi-tenancy required in first release? | Tenant context complexity | Yes; later; no | Yes (ASM-005) | FR-021 scope | Open | FR-021 |
| Q-011 | Optional | What is the expected workload envelope (calls/s, context size)? | Capacity reasoning | Supplied numbers | Unresolved | NFR-001 sizing | Open | NFR-001 |
| Q-012 | Important | Do single-use approvals and budgets need consistency across multiple processes of one application? | Shared versus local state | Per process; shared store; both | Per process, with a store port | DEC on state store | Open | FR-005, FR-016 |
| Q-013 | Optional | Where must audit events reside and for how long? | Residency of DATA-005 | Host region; specified jurisdiction | Unresolved | Sink adapter behavior | Open | FR-017 |
| Q-014 | Optional | Who owns support, maintenance and licensing of the package? | Operability and adoption | Named owner; Unresolved | Unresolved | Ownership fields | Open | NFR-002 |
