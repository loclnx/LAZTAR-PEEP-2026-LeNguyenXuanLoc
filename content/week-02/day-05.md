+++
title = "Day 05 - Friday - 25/09/2026 (REMOTE) (Sprint 0 Integration & Final Review)"
weight = 5
+++

## Tasks Completed

- Reviewed the final Sprint 0 requirements for the Mini-WMS design and confirmed the integration scope across the five core domains.
- Verified the critical links between Authentication & RBAC, Warehouse Structure, Product & Supplier, Inventory Model, and Stock Ledger & Movement.
- Identified and resolved mismatches in foreign keys, naming conventions, data types, statuses, and business rules.
- Tested the required operational scenarios to ensure the design works in practice, not only on paper.
- Consolidated the domain ERDs into a unified v1 ERD and checked that the PlantUML diagram could render successfully.

## Overview

Day 5 marked the integration and final review point of Sprint 0. The key objective was not only to finalize the design of individual modules, but to prove that all five domains could work together as one consistent WMS architecture.

The report states that Friday is the milestone for cross-domain validation. At this stage, the team must demonstrate that the model can handle eight business scenarios, maintain traceability, and support warehouse operations without breaking structural consistency. In short, the design must be tested against operational reality rather than just reviewed as static schema diagrams.

## Key Integration Focus

### 1. Cross-domain alignment

The design review confirmed the need to align the following relationships:

- Authentication and RBAC: user roles and authorizations must be linked to warehouse-level access.
- Warehouse Structure: each warehouse and location tree must support both physical layout and operational flow.
- Product and Supplier: product definitions and supplier references must integrate with stock handling correctly.
- Inventory Model: inventory records must be stored by location, SKU, and lot state with valid constraints.
- Stock Ledger and Movement: stock changes must be traceable, consistent, and aligned with inventory updates.

This alignment was essential because the system would fail not from one isolated table, but from mismatches between domain contracts.

### 2. Shared business rules and data conventions

The validation checklist highlighted several mandatory points that needed to be agreed on across all domains:

- `user_roles.warehouse_id` must reference `warehouses`.
- `stock_ledger.created_by` must reference `users`, and the role allowed to view or approve ledger actions must be explicit.
- Only `location_type = BIN` may hold inventory, while QC and DAMAGED states must be clearly defined in availability logic.
- UoM and base quantity rules must be standardized across inventory and ledger entries.
- Inventory and ledger must share the same business key and transaction logic for reconciliation.
- Deactivation rules must prevent removing active SKUs or warehouse locations still in use.

These decisions reduce the risk of inconsistent states during receiving, storage, shipping, and adjustment.

## Required Validation Scenarios

The document specifies eight business scenarios that must be checked in order to validate the design.

### Scenario 1: Receipt of goods

Receive 10 BOX of SKU-001 into `RECV-01`, where 1 BOX = 12 EA.

Expected result:

- Receipt quantity is recorded correctly.
- Inventory increases by 120 EA.
- On-hand quantity at the receiving location becomes 120 EA.

### Scenario 2: Putaway from receiving to storage

Move 120 EA from `RECV-01` to `BIN A01-R03-B05`.

Expected result:

- Two ledger entries are recorded: -120 and +120.
- Total stock remains unchanged.
- Inventory moves between positions without affecting total quantity.

### Scenario 3: Reservation for outbound order

Create an outbound order for 30 EA and reserve stock.

Expected result:

- `reserved = 30`.
- `available = 90`.
- Ledger does not change because the reserve is a commitment, not a physical movement.

### Scenario 4: Pick and ship

Pick and ship 30 EA.

Expected result:

- On-hand stock reduces to 90.
- Reserved quantity becomes 0.
- Ledger reflects PICK and SHIP transactions.

### Scenario 5: Stock count variance

Perform a stocktake and identify a shortage of 2 EA.

Expected result:

- Adjustment is recorded as `ADJUST OUT -2`.
- Approval and audit steps are enforced if required by policy.

### Scenario 6: Wrong receipt SKU

A receipt is created with the wrong SKU.

Expected result:

- The incorrect receipt is reversed.
- Correct SKU receipt is posted.
- No historical movement is overwritten incorrectly.

### Scenario 7: Unauthorized picker adjustment

A picker attempts to adjust stock in a warehouse area without the required permission.

Expected result:

- The system denies the action.
- RBAC enforcement blocks unauthorized access.

### Scenario 8: Concurrent reservation conflict

Two users attempt to reserve the last 90 EA at the same time.

Expected result:

- Only one transaction succeeds.
- The other is rejected using optimistic locking, versioning, or row lock mechanisms.

These scenarios are important because they validate the logic behind real warehouse operations rather than only the schema structure.

## Mandatory Completion Criteria

The final day requires the design to satisfy the following criteria:

- The unified ERD v1 must render successfully.
- There must be no orphan tables in the overall design.
- All cross-domain foreign keys and data types must be consistent with the data dictionary.
- All eight operational scenarios must pass through review or implementation checks.
- Any non-passing case must be logged as a known issue with an owner and resolution plan.
- The PlantUML source must be updated in Git and ready for submission.

## Risks to Control

The document highlights several high-priority risks:

- Difference between inventory state and ledger interpretation of reserve logic.
- Inconsistent keys or data types across modules.
- Failure of the overall ERD to render successfully.

These issues were considered critical because they directly affect both operational correctness and delivery reliability. The project team therefore needed to turn them into explicit decisions during the integration meeting rather than leaving them unresolved until the final submission date.

## Reflection

This day showed that Sprint 0 is not only about drawing a database model, but about proving that the model can support real warehouse behavior. The team had to validate the domain boundaries, check whether the tables correctly connect, and ensure that the business rules remain consistent during transactions and adjustments.

The most important lesson from Day 5 is that integration is where system quality is truly tested. A design that looks correct in isolation may still fail in practice if inventory, ledger, RBAC, location, and product rules are not aligned. This review helped close that gap and create a stronger foundation for the upcoming implementation phases.

## Conclusion

Day 5 completed the Sprint 0 integration review for the Mini-WMS project. The team confirmed the connection between the five core domains, verified the required business scenarios, and consolidated the ERD into a unified v1 design. The result is a more realistic and operationally grounded design, ready to move forward with implementation and final reporting.
