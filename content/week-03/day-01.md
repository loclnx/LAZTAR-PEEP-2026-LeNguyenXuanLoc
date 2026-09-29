+++
title = "Day 01 - Monday - 28/09/2026 (REMOTE) Sprint 0 Planning & Domain Alignment"
weight = 1
+++

## Tasks Completed

- Reviewed the Sprint 0 plan and confirmed the overall scope of the Mini-WMS design phase.
- Discussed the five core domains: Authentication & RBAC, Warehouse Structure, Product & Supplier, Inventory Model, and Stock Ledger & Movement.
- Agreed on the common naming conventions, data types, audit columns, and soft-delete policy for all modules.
- Defined the rules for primary keys, foreign keys, status values, and required metadata across the system.
- Started drafting the business flow for inbound, putaway, pick, ship, transfer, and adjustment scenarios.
- Identified the key dependencies between domains so the design can be integrated instead of developed in isolation.

## Overview

Day 1 of Sprint 0 focused on the foundation of the system design. The key idea was not to design each domain separately, but to create a unified WMS architecture in which all modules match each other in naming, logic, and business rules.

This is the point where the project shifts from high-level understanding to actual structural planning. The team had to decide the standards that would later control the ERD, data dictionary, and business rules. Without this alignment, the design would be inconsistent and difficult to implement.

## Key Focus Areas

### 1. Sprint 0 goals

The project plan states that Sprint 0 must produce one integrated design package for the warehouse management system. The final output is not just a list of tables, but a working design structure that can support real inventory operations.

The major deliverables include:

- Business flow diagrams for business scenarios.
- ERD v1 for the full system.
- Data dictionary per domain.
- Business rule list with identifiers.

### 2. Domain division

The team split the system into five domains, each with a clear scope and dependency chain:

- Domain A: Authentication & RBAC
- Domain B: Warehouse Structure
- Domain C: Product & Supplier
- Domain D: Inventory Model
- Domain E: Stock Ledger & Movement

This division is important because the design is not independent by module. For example, warehouse structure and product information are required before inventory and ledger rules can be modeled correctly.

### 3. Common conventions agreed on day 1

The team aligned several conventions that would later be enforced across the database design. These included:

- Table names in snake_case and plural form.
- Column names in snake_case.
- Primary keys using `id` and foreign keys using `<table>_id`.
- Status values stored as uppercase enum-like codes such as ACTIVE, INACTIVE, RECEIPT, and SHIP.
- Quantity fields using `DECIMAL(18,4)` rather than floating-point values.
- Timestamps stored in UTC with user timezone conversion on display.
- Audit columns required for master tables: `created_at`, `created_by`, `updated_at`, `updated_by`.
- Inventory ledger entries kept append-only, with no direct update/delete of historical records.

These rules are critical for warehouse accuracy because inventory data must be traceable, consistent, and auditable.

### 4. Dependency and integration logic

The plan also emphasized that B and C are the foundation domains. They must be locked early because D and E depend on them. In other words, the warehouse structure and product definitions must be known before inventory and movement logic can be finalized.

This lesson helped the team understand that design quality depends on how domains connect, not only on how each table is defined internally.

### 5. Business flow and validation scenarios

The team started sketching the main warehouse lifecycle, including:

- Inbound receipt
- Putaway to storage
- Reservation for outbound order
- Pick and ship
- Internal transfer
- Adjustment and cycle count

The Sprint 0 checklist also mentions eight core business scenarios that must later be tested on paper to validate the design. These scenarios include correct receipt recording, successful putaway, stock reservation, stock adjustment, wrong-SKU reversal, RBAC blocked actions, and concurrent reservation conflict.

## Lessons Learned

### 1. Team alignment matters more than individual modeling

A domain can be perfectly designed in isolation, but still fail once connected to other modules. The most important lesson from Day 1 was that the project requires a shared contract across all five domains.

### 2. Inventory rules are the real complexity

The design discussion showed that stock movements are not simple CRUD operations. Inventory must track physical quantity, reserved quantity, and available quantity while keeping ledger and business state consistent.

### 3. Traceability is mandatory

This project is not only about storing data but also about preserving a full audit trail. Every movement, correction, approval, or reversal must leave a traceable record.

### 4. Standardized conventions reduce risk

The decisions on naming, status values, UoM, and audit fields are small at first glance, but they significantly reduce integration issues later in the project.

## Challenges Discussed

### 1. Cross-domain mismatch risk

One major risk is that different team members may define the same field or concept differently, which causes broken foreign keys or mismatched business logic.

The solution is to define the common conventions early and review dependencies during the integration meeting.

### 2. Reserve behavior ambiguity

The Sprint 0 notes also highlight a major design decision: whether to record reservation actions in the stock ledger or keep them only in the inventory model. This decision affects how traceability and auditability are implemented.

### 3. Timing and dependency pressure

Because B and C must be completed early, the team must keep a tight schedule. If warehouse structure or product model is delayed, the inventory and ledger work cannot proceed smoothly.

## Conclusion

Day 1 of Week 3 laid the foundation for the entire Sprint 0 design work. The team agreed on the system architecture, clarified domain ownership, and set the rules that would guide the data model and inventory logic.

This was a strong start because the project focused on integration and alignment from the beginning, rather than rushing into separate module designs that might later conflict. The result is a more realistic and robust design process for the Mini-WMS project.
