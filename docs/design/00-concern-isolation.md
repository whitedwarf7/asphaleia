# Concern Isolation: What an Installable Python Package Can Address

Design ID: ASPHALEIA. Contract version: 1.0.0. Artifact: pre-stage analysis feeding `requirement-analyzer` (scope input). Artifact status: Provisional — classification is analytical (Inferred) against the concern inventory in SRC-002; stakeholder validation of the threat-model boundary (Q-001) may move rows between columns.

## Scope

Classify every concern from the AI-SaaS security inventory (SRC-002, grounded in SRC-003–SRC-006) by whether an in-process Python library installed into an AI-enabled application can address it, and map each addressable concern to a package capability (FR ID in [01-requirements-baseline.md](01-requirements-baseline.md)). This file does not design components; that is [04-high-level-design.md](04-high-level-design.md).

## Assumptions

- ASM-002: the host application process is cooperative and not itself compromised; the attacker controls untrusted content and possibly tool servers. If false, in-process controls can be bypassed and an out-of-process strict mode (DEC-011) becomes necessary.
- ASM-003: the host can supply principal/tenant identity and retrieval ACL metadata; if not, retrieval and sensitive-tool controls fail closed rather than silently degrading.
- ASM-006: SRC-002 (the research summary the user built this request on) is an authorized concern inventory, not a requirements source of record for numeric targets.

## Open questions

- Q-001 (Important): Is host-process compromise inside the threat model? Default: no (ASM-002).
- Q-002 (Important): Which frameworks/SDKs must be supported first? Default: framework-neutral core, MCP client and provider HTTP adapters first.

## 1. Control-point model

An installable library executes inside the application that imports it. It can observe and intercept only what passes through code paths the application routes to it:

| Control point the package can own | Control point outside the package |
| --- | --- |
| Context assembly for model calls (what text enters the prompt, with what provenance) | Model weights, alignment, training data, provider-side behavior |
| Tool / function / MCP invocation before execution and its results after | Operating-system isolation of tool processes, network egress enforcement |
| Retrieval queries and results before they enter context | Source-system permission hygiene, vector-store infrastructure isolation |
| Model output before it reaches a sink (UI, DB, shell, HTTP) | Rendering behavior of third-party UIs the application does not control |
| Credentials handed to tools (brokered handles vs. raw secrets) | Secret-store operation, IdP scope grants, OAuth consent |
| Token/cost/time/iteration accounting for a request or session | Provider billing, quota, and abuse response |
| Structured security events emitted from the process | SIEM retention, incident response, legal hold |
| Its own configuration and the app's tool/MCP config files (offline scan) | Fleet-wide discovery of shadow agents on endpoints (MDM/EDR) |
| Its own supply-chain posture (deps, signing, SBOM) | Vendor contracts, training opt-out, sub-processor lists, regulatory obligations |

Consequence (Inferred, SRC-005): the only deterministic security boundary a library can provide is **policy enforcement on actions and data movement** (what tools may run, what may enter/leave context, what credentials are reachable). Detection of malicious text is probabilistic and can only be an **advisory signal** — never the sole control (BR-001).

## 2. Classification legend

| Class | Meaning |
| --- | --- |
| Addressable | The package can implement a deterministic control at a point it owns; residual risk is bounded and stated. |
| Partial | The package provides a deterministic enforcement pattern or an advisory signal, but a material part of the concern remains with model, infrastructure, vendor, or organization. |
| Enabler | The package cannot address the concern but produces evidence or configuration that helps the owning control (for example audit trails for compliance). |
| Not addressable | The control point is outside any in-process library; listed with the owner that must handle it. |

## 3. Concern matrix

Taxonomy IDs reference OWASP Top 10 for LLM Applications 2025 (LLMxx, SRC-003) and OWASP agentic threats (SRC-004). "Enforcement" states whether the package control is a deterministic boundary (D), an advisory signal (S), or evidence (E).

### 3.1 Model / prompt layer

| # | Concern | Class | Enforcement | Package capability (FR) | Remains outside the package (owner) |
| --- | --- | --- | --- | --- | --- |
| 1 | Indirect and direct prompt injection leading to data exfiltration or unintended actions (LLM01; SRC-005 lethal trifecta) | Partial | D on actions, S on text | FR-001 provenance labels, FR-002 capability policy, FR-003 trifecta rule, FR-010 structural sanitization, FR-011 advisory detection, FR-014 URL/exfil blocking | Model still follows injected text in pure-text answers (user manipulation, misinformation); model-level robustness (provider) |
| 2 | Sensitive information disclosure in outputs or via over-broad retrieval (LLM02) | Addressable | D | FR-012 retrieval authorization, FR-014 output guard, FR-015 redaction, FR-018 minimization before provider egress | Memorized training data (provider); correctness of host-supplied ACLs (host) |
| 3 | Supply chain: plugins, tools, MCP servers, skills, models (LLM03) | Partial | D on tool manifests, E on configs | FR-006 manifest pinning and drift detection, FR-007 poisoning scan, FR-019 config scan; NFR-002 own posture | Model provenance, organization dependency governance, build pipeline integrity (org/platform) |
| 4 | Data and model poisoning (LLM04) | Partial | D on RAG/memory writes only | FR-013 memory write policy, FR-001 provenance on ingested documents | Pre-training/fine-tuning data pipelines (provider/ML team) |
| 5 | Improper output handling: model output into HTML, SQL, shell, URLs (LLM05) | Addressable | D | FR-014 sink-aware sanitization and schema validation, FR-015 redaction | Third-party renderers the app does not control (host) |
| 6 | Excessive agency: over-broad tools, autonomous consequential actions (LLM06) | Addressable | D | FR-002 default-deny capability policy, FR-003, FR-004 argument validation, FR-005 approval gate, FR-016 budgets | Scope of credentials granted by IdP/OAuth (host/IdP) |
| 7 | System prompt leakage (LLM07) | Partial | D on secrets, S on echo | FR-009 secrets never in prompts, FR-015 canary echo detection, FR-019 prompt lint | Paraphrased disclosure; design guidance that system prompts are not secrets (host) |
| 8 | Vector and embedding weaknesses: cross-tenant leakage, missing access control at retrieval (LLM08) | Partial | D | FR-012 pre-context ACL filtering, FR-021 tenant context propagation, FR-001 provenance on retrieved chunks | Embedding inversion, store-level isolation, ACL source of truth (infra/host) |
| 9 | Misinformation and hallucination, including hallucinated package names (LLM09) | Not addressable | E (narrow) | FR-004 can validate tool arguments such as install targets against allow-lists | Factual correctness, citations, human review (product/UX) |
| 10 | Unbounded consumption: denial-of-wallet, runaway loops (LLM10) | Addressable | D | FR-016 token/cost/time/tool-call/iteration budgets, loop detection | Provider quotas and billing alerts (platform) |

### 3.2 Agentic layer

| # | Concern | Class | Enforcement | Package capability (FR) | Remains outside the package (owner) |
| --- | --- | --- | --- | --- | --- |
| 11 | Tool misuse and over-broad write permissions | Addressable | D | FR-002, FR-004, FR-005, FR-016 | Granting of underlying scopes (host/IdP) |
| 12 | Credential exfiltration by manipulated agents | Addressable | D | FR-009 credential brokering (opaque handles, injection at egress), FR-015 redaction, BR-004 | Host-process compromise (Q-001); network egress enforcement (infra) |
| 13 | MCP tool poisoning, tool shadowing, rug-pull, toxic tool combinations | Addressable | D | FR-006 pinning and drift block, FR-007 description scan, FR-008 combination analysis | Server-side behavior changes invisible in the manifest (runtime) |
| 14 | Hard-coded credentials in agent/MCP configs; no inventory of connected servers | Partial | E | FR-019 CLI scanner for the application's own configs and policies | Fleet/endpoint discovery of shadow agents (MDM/EDR/org) |
| 15 | Memory poisoning of long-term agent memory | Partial | D on writes | FR-013 provenance-gated memory writes, FR-001 | Memory stores the app does not route through the package (host) |
| 16 | Agent goal hijack | Partial | D on actions | Same as row 1 | Model-level robustness (provider) |
| 17 | Cascading failures across multi-agent chains | Partial | D within process | FR-001 provenance, FR-016 budgets and FR-021 identity propagated across in-process agent hops | Cross-process agents not integrated with the package (host/infra) |
| 18 | Insufficient logging and traceability of agent decisions | Addressable | D/E | FR-017 structured, redacted security events with correlation IDs | Retention, SIEM, incident response (org) |
| 19 | Human-trust exploitation, approval fatigue | Partial | D on binding, S on UX | FR-005 approvals bound to exact call hash, expiring, showing instruction provenance | Approver training and UX design (host/org) |
| 20 | Agent sandboxing at OS level (filesystem, syscalls) | Enabler | D on requirement only | FR-002 capability class can require code/shell tools to run through a configured sandbox adapter (INT-008) | The sandbox itself: OS isolation, containers, kernel controls (platform) |
| 21 | Browser-as-sandbox / remote browsing isolation | Not addressable | — | None | Browser/runtime isolation (platform) |

### 3.3 Governance / procurement layer

| # | Concern | Class | Enforcement | Package capability (FR) | Remains outside the package (owner) |
| --- | --- | --- | --- | --- | --- |
| 22 | Vendor training on customer data, opt-out defaults | Enabler | E | FR-018 can set provider request options that disable training/retention where an API exposes them (later tier, ASM-007) | Contracts, DPA terms, opt-out administration (legal/procurement) |
| 23 | Fourth-party data flows: sub-processors, prompt retention, residency of inference | Partial | D on egress | FR-018 provider endpoint allow-list, minimization and redaction before egress | Sub-processor lists, residency contracts, retention terms (legal/vendor) |
| 24 | Shadow AI: employees pasting data into consumer chatbots | Not addressable | — | None | DLP, network controls, policy (security org) |
| 25 | Oversharing amplification: copilots surfacing already-overshared files | Partial | D at retrieval | FR-012 enforces host-supplied ACLs before context assembly | Permission hygiene in source systems (data owners) |
| 26 | Tenant isolation in shared inference/RAG infrastructure | Partial | D in-app | FR-021 tenant propagation, FR-012 filtering, fail-closed on missing tenant | Infrastructure-level isolation, provider multi-tenancy (platform/vendor) |
| 27 | Auditability and incident investigation of AI-mediated actions | Addressable | D/E | FR-017 | SIEM, retention, IR process (org) |
| 28 | Regulatory: GDPR, EU AI Act, ISO/IEC 42001, SOC 2 scope, HIPAA/BAA | Not addressable | E | FR-017 audit evidence and data-flow records support assessments | Legal applicability, assessments, certification (compliance/legal) |
| 29 | Model change risk: vendor swaps model or safety behavior without notice | Partial | D on pin, S on change | FR-018 model identifier pinning and change alert | Behavioral regression evaluation harness (product/QA); vendor notice terms |
| 30 | IP and indemnity for generated output | Not addressable | — | None | Contracts (legal) |

## 4. Result

| Class | Rows | Count |
| --- | --- | --- |
| Addressable | 2, 5, 6, 10, 11, 12, 13, 18, 27 | 9 |
| Partial | 1, 3, 4, 7, 8, 14, 15, 16, 17, 19, 23, 25, 26, 29 | 14 |
| Enabler | 20, 22, 28 | 3 |
| Not addressable | 9, 21, 24, 30 | 4 |

### 4.1 Capabilities the package should provide (in scope)

1. Provenance and trust labeling of every context segment, propagated across the session and across in-process agent hops (FR-001, FR-021).
2. Declarative, default-deny capability policy on every tool/MCP call, including the lethal-trifecta rule and argument validation (FR-002, FR-003, FR-004).
3. Human approval gate bound to the exact call, single-use, expiring (FR-005).
4. Tool/MCP trust management: manifest pinning, drift (rug-pull) blocking, poisoning scan, toxic-combination analysis (FR-006, FR-007, FR-008).
5. Credential brokering so secrets never enter model context, tool results, or logs (FR-009).
6. Ingress content guard: structural sanitization plus pluggable advisory detection (FR-010, FR-011).
7. Retrieval and memory guard: ACL/tenant filtering before context, provenance-gated memory writes (FR-012, FR-013).
8. Output guard: sink-aware sanitization, URL/exfil-channel blocking, schema validation, redaction, canary echo detection (FR-014, FR-015).
9. Consumption governor: budgets and loop termination (FR-016).
10. Audit emitter: structured, redacted, correlated security events through a sink port (FR-017).
11. Provider egress guard: endpoint/model allow-list and pinning, minimization/redaction (FR-018).
12. CLI scanner and policy dry-run for rollout (FR-019, FR-022).
13. Framework adapters so the above attach without rewriting the application (FR-020).

### 4.2 Explicitly out of scope (CON-004) and where handled instead

| Concern | Handled by |
| --- | --- |
| OS-level sandboxing of tool execution, browser isolation | Platform/infra; package exposes a sandbox-adapter port only |
| Network egress enforcement independent of the host process | Infra (egress proxy/firewall); optional out-of-process strict mode is DEC-011 (Deferred) |
| Vendor training on data, sub-processor terms, IP/indemnity | Legal/procurement |
| Shadow AI and fleet discovery | Security org tooling (DLP, MDM/EDR) |
| Permission hygiene of source systems, IdP scope grants | Data owners, IAM |
| Model provenance, training-data poisoning, model robustness | Provider / ML platform |
| Regulatory assessments and certification | Compliance/legal; package supplies evidence only |
| Factual correctness / hallucination | Product and UX controls |

These exclusions are constraints on the package (CON-004), not a claim that the concerns are unimportant. The HLD records each as an integration point or a documented residual risk.

## 5. Handoff

Next: `requirement-analyzer` consumes SRC-001/SRC-002 and this classification to produce the FR/NFR/BR/CON baseline ([01-requirements-baseline.md](01-requirements-baseline.md)). Changed IDs: none (initial). Validation: analytical classification only; no runtime evidence, no product evaluated.
