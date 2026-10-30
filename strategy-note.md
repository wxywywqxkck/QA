# Test Strategy Note 
## Priority Definition
- **P0 (Critical)**: Inventory synchronization consistency, oversell prevention, payment core flow, LLM factual accuracy. Inventory data error is classified as severe production incident.
- **P1 (High)**: End-to-end order lifecycle: place, pay, ship, sign, return. Core happy path coverage.
- **P2 (Medium)**: Boundary scenarios, non-core functions, usability, browser/terminal compatibility.
- **P3 (Low)**: Portability, maintainability optimization, full-load stress test, failure drill.

## Test Method Selection
- State transition scenarios (order, inventory): State machine model + decision table method.
- Input parameter validation (API, form): Equivalence partitioning + boundary value analysis.
- Business process scenarios: Scenario-based testing + error guessing method.
- LLM capability validation: Adversarial prompting test + manual benchmark comparison.

## Trade-offs
1. Cross-system WMS/SC2P integration adopts eventual consistency model, supported by scheduled reconciliation and compensation jobs, to balance high concurrency performance and data correctness.
2. MQ asynchronous messaging is used for external sync rather than synchronous calls, to avoid blocking the order placement path. Idempotency control, dead letter queue and scheduled compensation cover message loss, duplication and disorder.
3. When test cycle is limited, P0 and P1 cases must be fully executed first. Full stress test and fault drill can be conducted in staging environment after release.

## Core Risks & Mitigation
Top risks: Inventory oversell, inventory book-physical mismatch, stuck orders that cannot proceed.
Mitigation: Scheduled reconciliation task, real-time monitoring & alert, standardized manual data repair workflow with approval.
