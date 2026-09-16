# Terminology

Contract version: 1.0.0. These definitions apply to every skill and handoff.

## Evidence classes

| Class | Meaning | Required treatment |
| --- | --- | --- |
| Confirmed | Explicitly stated in an authorized source | Cite the source; this is not proof that implementation satisfies it. |
| Inferred | A reasoned interpretation of supplied evidence | Explain the inference and request validation when material. |
| Assumed | A temporary premise used to continue | Assign an ASM ID, consequences, validation condition, and fallback. |
| Proposed | A recommendation, control, component, or product not yet approved | Link drivers, alternatives, and approval conditions. |
| Unresolved | Evidence is missing, contradictory, or insufficient | Assign a Q ID; never fill with an invented fact. |

Confidence measures evidence quality, not business priority or approval. An accepted decision needs explicit approval evidence; repetition and silence are not approval.

## Architectural vocabulary

| Term | Definition |
| --- | --- |
| Requirement | An attributable desired capability, quality, business rule, or constraint. |
| Architecture driver | A requirement or constraint that materially changes boundaries, deployment, data, or operation. |
| Logical component | A responsibility boundary; it need not be independently deployed. |
| Container | A major application, process, or data store in the C4 sense; not necessarily an OS container. |
| System context | The subject system as one box, its actors, and external dependencies. |
| Trust boundary | A change in identity, authorization, ownership, privilege, or data-handling assumptions; not necessarily a subnet. |
| Integration | A conceptual contract across a component or system boundary. |
| Flow | An ordered business interaction, including outcomes and failure paths. |
| Availability | Whether the required service is usable, from a stated observer and measurement window. |
| Reliability | Correct service over the relevant operating conditions and time. |
| Resilience | Ability to contain, recover from, or adapt to failure without unacceptable harm. |
| Scalability | Ability to accommodate workload growth with acceptable resource and operational cost. |
| Performance | Observed latency, throughput, resource use, and workload behavior. |
| SLI / SLO | A measured service indicator / its explicitly agreed objective and window. |
| RTO / RPO | Agreed recovery-time / recoverable-data-loss objectives, not measured guarantees. |
| Readiness | A review recommendation, never production approval or certification. |

## Stable identifiers

Use uppercase prefixes and at least three digits, allocated within one design. Do not recycle deleted IDs or renumber unchanged records. Record supersession and redirects when merging. Different designs have separate namespaces; carry the design identifier at handoffs.

| Prefix | Entity |
| --- | --- |
| SRC | Source |
| FR / NFR / BR / CON | Functional / non-functional / business-rule / constraint requirement |
| ACT / EXT | Human actor / external system |
| DATA / TB / DEP | Data entity / trust boundary / deployment element |
| ASM / Q | Assumption / clarification question |
| DRV | Architecture driver |
| CMP | Architecture component, including persistent and operational stores |
| FLW / INT | Data flow / integration |
| DEC / ADR | Decision / decision record; link the two explicitly |
| RISK | Shared risk register entry |
| SEC / REL / OPS / REV | Security / reliability / operations / final-review finding or recommendation |

Use `CMP_001` in Mermaid for component `CMP-001`; apply the same hyphen-to-underscore mapping to other identifiers. These are aliases, not newly allocated entities.

## Distinctions that must survive handoffs

- Requirement priority is Must, Should, Could, or Unspecified; do not infer it from tone.
- Clarification priority is Critical, Important, or Optional.
- Finding severity is Critical, High, Medium, or Low, with an evidence-based rationale.
- Proposed architectural coverage is not verified implementation coverage.
- Unknown, None, and Not applicable are different: explain each use.
- Security, privacy, compliance, and data residency are related but not interchangeable.
- Accessibility concerns user-facing interaction; justify non-applicability for backend-only scopes.