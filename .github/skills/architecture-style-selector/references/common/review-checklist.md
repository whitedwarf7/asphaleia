# Review Checklist

Use this checklist for every complete design. For each dimension record Covered, Gap, Unresolved, or Not applicable with evidence, requirement/component/decision IDs, risks, and validation required. A checkmark means reviewed, not implemented or certified.

| Dimension | Evidence and questions to examine |
| --- | --- |
| Requirements coverage | Every FR/NFR/BR/CON has a trace row; priorities and supplied values survive; exclusions and unresolved requirements are explicit. |
| Internal consistency | Sources, status, names, IDs, diagrams, stores, flows, decisions, and versions agree; no accidental promotion of assumptions. |
| Responsibility clarity | Component boundaries, ownership, inputs/outputs, dependencies, and logical/deployment distinctions are explicit. |
| Security | Identity, authentication, authorization, least privilege, API/input protection, network/trust boundaries, encryption, secrets, audit, detection, supply chain, protected backups. |
| Privacy | Data purpose, minimization, access, tenant boundaries, retention/deletion, telemetry leakage, rights and legal-hold questions. |
| Availability | User-visible failure modes, dependency availability, health/routing, deployment interruption, and supplied SLO measurement boundaries. |
| Reliability | Correctness invariants, duplicates, ordering, idempotency, reconciliation, and durable acknowledgement behavior. |
| Resilience | Fault isolation, circuit breaking, bounded retry ownership, backpressure, graceful degradation, recovery, failback, correlated failures. |
| Scalability | Workload growth, horizontal/vertical options, state and partitioning, load leveling, bottlenecks and capacity uncertainty. |
| Performance | Latency distributions, throughput, critical-path dependencies, cache correctness, measurement scope, test data, and missing targets. |
| Maintainability | Module cohesion, coupling, schema/API evolution, operational complexity, migrations, and decision reversibility. |
| Extensibility | Change scenarios and extension points justified by requirements, not speculative features. |
| Interoperability | Conceptual contracts, compatibility, external system authority, protocol status, ownership, and failure responsibilities. |
| Data management | Authoritative ownership, transaction/consistency boundaries, lineage, copies, lifecycle, corruption and recovery. |
| Integration | APIs/events/batch/files when applicable; auth, timeout, retry, idempotency, rate limits, schema evolution, dead letters, reconciliation. |
| Observability | Logs/metrics/traces, safe correlation, SLIs, dashboards, alert actionability, cardinality, sensitive-data handling. |
| Operability | Health, deployment monitoring, rollback, incident response, runbooks, ownership gaps, support and maintenance constraints. |
| Testability | Contract, authorization, failure, restore, load, migration, accessibility, and deletion tests with observable acceptance evidence. |
| Disaster recovery | RTO/RPO source, backup protection, independent copies, restore drills, recovery dependencies, residency and retention. |
| Cost awareness | Cost drivers, budgets if supplied, utilization, data transfer, idle redundancy, recovery, telemetry, staffing and alternatives; no invented prices. |
| Accessibility | User-facing journeys, assistive-technology/interaction considerations, acceptance checks and applicable standard questions; justify backend-only exclusions. |
| Compliance and data residency | Supplied jurisdiction/policy, applicability questions, data and backup placement, lawful purpose, stakeholder validation; no invented regulations or certification. |
| Diagram consistency | Appropriate view detail, stable IDs, complete relevant components, trust boundaries, direction, readable labels, explanation, actual parser/render status. |
| Assumption visibility | Every material assumption has an ASM record, consequence if false, validation and links; Critical questions are not bypassed. |
| Decision traceability | DEC/ADR status agrees, alternatives and trade-offs are recorded, approvals are evidenced, no orphan components or invented owners. |

## Final assessment format

Return Overall assessment; Critical gaps; High-priority improvements; Medium-priority improvements; Strengths; Unresolved decisions; Readiness recommendation. Also include the full dimension matrix, low-severity follow-ups, risk links, and validation not performed.

## Readiness rules

- Not ready: unresolved Critical decision, Critical finding, contradictory core scope, unauthorized inputs, or missing baseline prevents responsible review. Identify the specific blocker and allow safe independent work.
- Ready for stakeholder review with conditions: a coherent provisional proposal has explicit gaps and an actionable validation plan; no assumption is represented as approval.
- Ready for detailed design: material choices have sufficient evidence and explicit stakeholder acceptance where required; record any remaining bounded issues. This is not permission to deploy.
- High findings need an agreed mitigation/validation plan before recommending detailed design. Unknown evidence cannot be treated as a passed control.
- Final review never grants production approval, accepts organizational risks on behalf of owners, or certifies security/compliance.