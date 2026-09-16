# Component and Flow Register

Use alongside the common 28-section design template. These empty tables are authoring forms, not completed output. Populate them from the supplied baseline; use explained Unresolved or Not applicable fields when evidence is absent.

## Component register

State the evidence status and logical-versus-deployable interpretation before the table.

| Component ID | Component name | Responsibility | Inputs | Outputs | Dependencies | Data owned | Scaling considerations | Security considerations | Failure considerations | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Trust-boundary register

| Boundary ID | Boundary meaning | Inside IDs | Outside IDs | Crossing flow IDs | Proposed controls | Source or assumption | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Critical-flow register

| Flow ID | Trigger | Producer | Consumers | Data and classification | Stores | Interaction mode | Validation and transformation | Success and failure paths | Retention and deletion | Related requirement IDs |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Deployment mapping

| Deployment ID | Logical component IDs | Execution or storage responsibility | Isolation and placement | Scaling unit | Failure and recovery boundary | Status | Validation required |
| --- | --- | --- | --- | --- | --- | --- | --- |

Do not create deployment elements that contradict the component register. A proposed process grouping may contain multiple logical modules; state this rather than implying independently deployed microservices.

## Reconciliation checks

- Every diagram alias maps to a registered entity or component.
- Each required interaction has consistent sender, receiver, meaning, and direction.
- Important components appear in the relevant view; justified omissions are recorded.
- Every requirement maps to a component/decision or a visible gap.
- Security, reliability, operations, and integration findings are merged into the design without silently changing approved records.