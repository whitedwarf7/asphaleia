# High-Level System Design

Use every numbered section below in order. Replace instructional text with evidence-backed content, or state Unresolved / Not applicable with rationale. These instructions are a template, not requirements for the subject system.

Scale the depth to the system. A small system may answer a section in one or two sentences; a large or regulated one needs the full register treatment. Keep every heading so the design stays comparable and reviewable, but do not pad a section to look complete. Precede a full-pipeline design with the shared handoff envelope and keep one proposal caveat.

## 1. Executive Summary

Describe the proposal, business value, key alternatives, confidence, and material blockers. State that stakeholder validation is required and no quality or compliance guarantee is made.

## 2. Business Objective

Describe the source-backed outcome, success criteria if supplied, and scope owner if supplied.

## 3. Scope

### In Scope

List attributable capabilities and boundaries with requirement IDs.

### Out of Scope

List confirmed exclusions separately from proposed deferrals; include related decision IDs.

## 4. Stakeholders and Actors

Use the entity register for actors and external systems. Do not invent departments, approvers, or integrations.

## 5. Requirements Summary

### Functional Requirements

Use the exact structured-requirement columns for FR and BR records, or link the full baseline and summarize all relevant IDs.

### Non-Functional Requirements

Use the same contract for NFR records, retaining source values, status, confidence, and missing measurement definitions.

### Constraints

Include CON records and business rules affecting scope, data, technology, organizational operation, or deployment.

## 6. Assumptions

Use the assumption register, including consequences if false and validation conditions.

## 7. Open Questions

Use the question contract, grouped Critical, Important, Optional. Distinguish the active batch of at most seven from queued questions.

## 8. Architecture Drivers

Use the driver contract. Link requirements and explain architectural impact, including missing targets.

## 9. Recommended Architecture Style

Reference the style comparison and DEC records. Recommend only with sufficient evidence; otherwise retain alternatives and decision gates. Explain composable style dimensions.

## 10. System Context

Describe one subject system, external dependencies, ownership, human actors, scope, and trust boundaries.

## 11. System Context Diagram

Include a Mermaid flowchart showing the subject as one box, no internal implementation, labeled interactions, and a textual explanation. If blocked, give the reason and a truthful textual view.

## 12. Logical Component Architecture

Describe responsibility groupings, modularity, data ownership, synchronous versus asynchronous interaction, and the difference between logical and deployment boundaries.

## 13. Container Diagram

Include a Mermaid flowchart with component IDs, applicable applications/workers/stores/external dependencies, relevant trust boundaries, directional labels, and a textual explanation. Do not add infrastructure merely to fill the view.

## 14. Component Responsibilities

Use every field in the component contract. Include major operational components if shown in the diagram. State Proposed unless already approved with evidence.

## 15. Critical Data Flows

Use the flow contract. Include selected Mermaid sequence diagrams with producer/consumer IDs, data sensitivity, durable acknowledgement, authorization, validation, transformations, timeouts, duplicates, retries, and terminal failure paths. Explain each diagram immediately afterward.

## 16. Integration Design

Use the integration contract for conceptual APIs, messages, batch or file transfers where applicable. Cover schemas, auth boundaries, retries, idempotency, limits, dead letters, and reconciliation. Do not generate detailed API specifications without an explicit request.

## 17. Data Architecture

Record entities, classification evidence, owners, authoritative stores, consistency and transaction boundaries, lifecycle, retention/deletion, lineage, backups, privacy, and residency. Do not equate a datastore category with a consistency guarantee.

## 18. Security Architecture

Describe proposed identity, authorization, least privilege, trust boundaries, tenant isolation if applicable, encryption, keys/secrets, validation, audit, detection, API protection, privacy, supply chain, and protected backup controls. Include review findings and validation gaps; no certification claim.

## 19. Scalability and Performance

Consider workload shape, horizontal/vertical scaling, load balancing, statelessness, caching, hot spots, queue load leveling, and backpressure. Keep unspecified objectives Unresolved; make test plans and conditional capacity trade-offs explicit.

## 20. Availability and Resilience

Identify single points of failure, dependency failures, bounded retries, idempotency, circuit breaking, failure isolation, graceful degradation, zone/region alternatives, and recovery prerequisites. Distinguish durability from availability.

## 21. Observability and Operations

Define logs, metrics, traces, safe correlation, health checks, dashboards, alerts, SLIs, supplied SLOs or open questions, deployment monitoring, incident response, runbooks, audit events, capacity and cost monitoring. Include ownership gaps without inventing teams.

## 22. Deployment Architecture

Describe technology-neutral execution units, environment isolation, placement constraints, promotion, rollback, change compatibility, and trust zones. Map existing CMP IDs to DEP records; leave provider, regions, replica counts, and products unspecified unless supplied or explicitly proposed.

## 23. Backup and Disaster Recovery

Cover backup scope, protected access, restore validation, dependencies, retention/deletion and legal-hold conflicts, RTO/RPO questions, failover/failback, and recovery drills. Replication does not replace independent backups.

## 24. Architecture Decisions

Use the complete decision contract and link associated ADRs. Distinguish Proposed, Deferred, and evidence-backed Accepted decisions.

## 25. Alternatives Considered

Compare meaningful alternatives, advantages, limitations, operational complexity, risks, rejection reasons, and conditions that would reverse the selection.

## 26. Risks and Mitigations

Use the risk contract with affected IDs, likelihood/impact evidence, severity, mitigation, residual risk, owner if supplied, and validation state.

## 27. Requirements Traceability

Use the traceability contract with every requirement ID, components/decisions, flows, validation, and coverage. Include justified unresolved and excluded rows; never equate coverage with implementation verification.

## 28. Recommended Next Steps

Prioritize critical clarifications, design changes, prototypes, security/privacy/legal review, performance/failure/restore tests, cost estimation, accessibility checks where applicable, ADR approval, and final quality review. Include readiness recommendation and remaining stakeholder gates.