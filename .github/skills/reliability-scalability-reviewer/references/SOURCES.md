# Sources and Provenance

Research date: 2026-09-15. This collection is original, task-specific guidance informed by the sources below. External prose, code, images, diagrams, checklists, and interview solutions have not been vendored. General concepts are synthesized, not treated as the subject system's requirements.

## Source register

| Reference | Inspected resource | Contribution | License observation and limits |
| --- | --- | --- | --- |
| REF-001 | [Donne Martin, System Design Primer](https://github.com/donnemartin/system-design-primer/blob/ae9bbd7b02d90b9866215de185217d33f39ab733/README.md) | Requirements-first design, alternatives, caching, replication, partitioning, load balancing, asynchronous work, consistency/performance distinctions | README explicitly states CC BY 4.0; [license](https://github.com/donnemartin/system-design-primer/blob/ae9bbd7b02d90b9866215de185217d33f39ab733/LICENSE.txt). No copied diagrams or solutions. |
| REF-002 | [Ashish Pratap Singh, Awesome System Design Resources](https://github.com/ashishps1/awesome-system-design-resources/blob/25724090f7dd7746129b7194b55504f9d06f86ed/README.md) | Topic discovery: APIs, idempotency, rate limits, database/cache trade-offs, messaging, distributed systems, batch/stream processing | [Inspected license](https://github.com/ashishps1/awesome-system-design-resources/blob/25724090f7dd7746129b7194b55504f9d06f86ed/LICENSE) is GPL-3.0. Used as an index, not copied or embedded. External linked resources have their own terms. |
| REF-003 | [Agent Skills specification](https://agentskills.io/specification) | Directory/name matching, YAML fields, activation descriptions, progressive loading, resource references | Format reference; its site content is not reproduced. |
| REF-004 | [VS Code Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills) | Supported discovery locations, relative references, slash invocation, discovery limitations | Official product behavior, checked on research date; functionality can change by client/version. |
| REF-005 | [OWASP ASVS repository](https://github.com/OWASP/ASVS) and [project](https://owasp.org/www-project-application-security-verification-standard/) | Security verification considerations and need for explicit validation evidence | Repository states CC BY-SA 4.0; observed stable version 5.0.0. Do not cite unverified individual control IDs or imply certification. No control text copied. |
| REF-006 | [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) | System decomposition, trust boundaries, threat scenarios, mitigation and review lifecycle | Page states CC BY-SA 4.0. Used for conceptual orientation; not copied. |
| REF-007 | [ADR community](https://adr.github.io/) and [MADR](https://github.com/adr/madr/blob/develop/template/adr-template.md) | Context, drivers, options, consequences, confirmation and decision history | MADR is MIT OR CC0-1.0. The project's ADR fields are independently authored to the user's contract. |
| REF-008 | [OpenTelemetry primer](https://opentelemetry.io/docs/concepts/observability-primer/) and [signals](https://opentelemetry.io/docs/concepts/signals/) | Logs, metrics, traces, correlation, measurement boundaries | Page license not independently verified; no copied content. OpenTelemetry is a reference, not a mandatory product selection. |
| REF-009 | [OpenTelemetry logs model](https://opentelemetry.io/docs/specs/otel/logs/data-model/), [metrics model](https://opentelemetry.io/docs/specs/otel/metrics/data-model/), and [trace API](https://opentelemetry.io/docs/specs/otel/trace/api/) | Signal semantics, context propagation, cardinality, aggregation and status-aware interpretation | Consult current document stability and version when implementation mapping is requested. No license inference or reproduced specification text. |

## How references shape the skills

| Skill area | References | Application |
| --- | --- | --- |
| Requirements, drivers, HLD | REF-001, REF-002 | Gather workload and constraints before selecting architecture; preserve uncertainties and trade-offs. |
| Style selection, data, integration, reliability | REF-001, REF-002 | Compare topology and interaction alternatives; evaluate failure, data consistency, and operational cost explicitly. |
| Context and container diagrams | REF-001, REF-006 | Use clear boundaries and appropriate abstraction; all Mermaid examples in this project are original. |
| Security review | REF-005, REF-006 | Review controls through credible scenarios and validation requirements, not a certification checklist. |
| Observability and operations | REF-008, REF-009 | Separate signals from SLOs and from backend products; protect telemetry and control cardinality. |
| ADRs and final review | REF-007 plus shared project contracts | Document decisions, alternatives, consequences, approval evidence, and traceability. |
| All skill packaging | REF-003, REF-004 | Precise frontmatter descriptions, focused bodies, referenced assets, supported installation locations. |

## Corrections and interpretation limits

- Educational repositories are not organizational policy, legal advice, current service documentation, or proof that a design fits a workload.
- CAP concerns the consistency/availability conflict during partitions, not a free choice of any two independent attributes.
- NoSQL and relational labels do not determine transaction scope, isolation, durability, or consistency guarantees. Check the actual candidate and operation.
- Historical latency tables, advertised service limits, and example architecture sizes are not project SLOs or measured performance.
- Service prices, quotas, availability promises, supported regions, compliance claims, and product lifecycles need current official documentation and stakeholder validation before a platform mapping is accepted.
- Verify each external link's content and license before any future copying. An index's license does not relicense all linked materials. Record changes and attribution for any permitted adaptation.
- The cited GitHub commit IDs identify the README snapshots observed during research; they do not assert that the repositories will remain at those revisions.

## Publication boundary

No license for redistribution of this new project has been selected on the user's behalf. The development package is private and UNLICENSED until its owner chooses terms. This does not alter third-party rights or dependency licenses. Legal/license review is a publication gate, not a claim about the subject system's compliance.