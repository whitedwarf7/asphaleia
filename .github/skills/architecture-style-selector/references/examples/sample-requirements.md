# Sample Requirements — Employee Document Service

This is an original, synthetic business brief for exercising the skills. It contains no real employee information, credentials, organizational policy, internal endpoint, or confidential source text. Do not treat it as requirements for an actual organization.

Design ID supplied for this fixture: EMPLOYEE-DOCS. Source ID: SRC-001. Baseline version: 1.0. The paragraphs below are authorized synthetic input; all requirement statements using "must" have supplied priority Must. No design decision, product, risk acceptance, or production deployment has been approved.

## S01 — Business objective and users

Provide a browser-based service so employees can store and retrieve work documents within a single organization. Employees must be able to upload a document, list documents they are authorized to access, and retrieve an authorized document. The business objective is reliable, controlled document access, not public content sharing. Business success metrics have not been supplied.

## S02 — Access administration

Records administrators must be able to create and revoke explicit document access grants. Their administrative role must not automatically grant permission to read every document. Every list and retrieval must evaluate the current document grants; if a required grant decision cannot be made, access must be denied rather than guessed. Identity and grant changes can invalidate prior access.

## S03 — Identity boundary

Employees, records administrators, and operations personnel must sign in through the existing corporate identity system. Its protocol, claim contract, token lifetime, account-disable propagation, and outage behavior have not been supplied. The document service must not build a new identity directory in this scope. Authentication does not itself establish document access rights.

## S04 — Upload correctness

The service must report upload success only after both the content and the corresponding document metadata have been durably recorded as a retrievable version. A repeated request for the same upload intent must not publish a second version. Invalid or incomplete content must not be exposed as an available document. Supported file types, file-size limits, upload rate, and acceptable validation duration remain unspecified.

## S05 — Document lifecycle

Records administrators must be able to record a document's retention disposition and legal-hold state. A legal hold must prevent deletion. No retention schedule, automatic deletion rule, legal-hold release authority, or applicable jurisdiction has been provided. Do not infer any of these from general industry practice; the affected irreversible-deletion decision needs clarification.

## S06 — Sensitive data and audit

Document contents, document titles, entitlement links, and identity information are sensitive in this fixture. They must be protected against unauthorized access. Application diagnostic logs, metrics, and traces must not contain document contents, document titles, personal information, or credentials. Upload results, document access, grant changes, and lifecycle actions must have attributable audit records available for authorized investigation. Audit records themselves need restricted access; no audit-retention or audit-outage policy is supplied.

## S07 — Recovery and operations

Operations personnel must be able to inspect service health and initiate recovery through authorized procedures. The design must consider protected backups of authoritative content, metadata, grants, lifecycle state, and audit evidence, and must propose restore validation. Operational role membership is not permission to browse document content. Availability, restore time, tolerable data loss, region failure requirements, maintenance windows, incident routing, operating owners, and support hours are not supplied.

## S08 — User-facing quality

Employee and records-administrator browser journeys must be accessible. No particular accessibility conformance level, jurisdiction, or acceptance standard is supplied. The service must have clear component responsibilities and a compatible change approach so changes to the browser and data representation can be tested without obscuring access rules.

## S09 — Scope exclusions

Public sharing and external-tenant hosting are excluded. Content editing, full-text search, optical character recognition, automatic classification, and AI extraction are also excluded from this initial design. Do not introduce an external scanning, notification, analytics, or other integration without evidence or an explicitly labeled, separately gated recommendation.

## S10 — Technology and scale

Generate the technology-neutral design first. No cloud, product stack, deployment environment, region, organizational technology standard, budget, throughput, storage volume, latency target, availability percentage, RTO, or RPO is supplied. A platform-specific mapping may be requested later only after the neutral baseline has been explicitly approved for that purpose.

## Expected use

Extract the [structured requirements](./sample-structured-requirements.md), then compare drivers and styles before assembling the [sample high-level design](./sample-high-level-design.md). Those files are illustrative outputs with Proposed decisions and unresolved questions, not evidence that an implementation or behavioral evaluation passed.