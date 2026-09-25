+++
title = "Day 04 - 24/09/2026 (REMOTE) (Warehouse Structure ERD Implementation)"
weight = 4
+++

## Tasks Completed

- Reviewed the Warehouse Structure ERD implementation document for Mini-WMS.
- Confirmed the core data model using `warehouses` and `locations` tables.
- Clarified the self-referencing location hierarchy and the role of `parent_id`.
- Identified the valid location types and operational purposes for each warehouse position.
- Defined data constraints, indexes, and integration points with inventory, stock ledger, and RBAC.

## Overview

This day focused on translating the warehouse structure design into an implementable ERD and data model. The document defines how the system will represent warehouses, physical locations, and their hierarchical relationships. The goal is to build a stable foundation for inventory control, stock allocation, stock movement, and warehouse traceability.

The design uses two main tables: `warehouses` and `locations`. This approach is simple but powerful because it supports the real warehouse hierarchy while keeping the structure flexible enough for future extension.

## Key Implementation Decisions

### 1. Core warehouse model

The `warehouses` table is the master record for each operating warehouse. It stores the warehouse code, display name, address, status, and tracking information for who created or updated the record.

Important points:

- `code` is unique and should not be changed once transactions have started.
- `status` should remain `ACTIVE` or `INACTIVE` rather than deleting the record physically.
- A warehouse may contain many operational areas such as receiving, storage, picking, staging, QC, and damaged goods zones.

### 2. Self-referencing location hierarchy

The `locations` table is self-referencing and stores both the physical structure and the operational purpose of every position in the warehouse.

A warehouse location hierarchy is modeled as:

`WAREHOUSE -> ZONE -> AISLE -> RACK -> BIN`

This allows the team to describe a realistic warehouse layout without creating a large number of separate tables. The `parent_id` field links each child location to its parent, while `warehouse_id` ensures that every location belongs to exactly one warehouse.

### 3. Storage rule: only BIN holds inventory

A key design decision is that inventory is never stored directly at `ZONE`, `AISLE`, or `RACK` level. Only `location_type = BIN` may be used as the final inventory location.

This creates clear operational logic:

- `ZONE` and `AISLE` define the physical structure.
- `RACK` organizes the storage layout.
- `BIN` is the actual carrying unit for product stock.

## Data Dictionary and ERD Notes

### `warehouses`

The `warehouses` table stores the master warehouse data. It includes:

- `id`: internal warehouse ID.
- `code`: business code such as `WH01`.
- `name`: warehouse name.
- `address`: warehouse address.
- `status`: operational status.
- `created_at` and `updated_at`: audit timestamps.
- `created_by` and `updated_by`: user references.

### `locations`

The `locations` table stores the full physical and operational location structure. It includes:

- `id`: internal location ID.
- `warehouse_id`: the owning warehouse.
- `parent_id`: parent location reference for hierarchy.
- `location_type`: `ZONE`, `AISLE`, `RACK`, or `BIN`.
- `code`: unique location code within the warehouse.
- `full_path`: complete path such as `WH01/Z-A/A01/R03/B05`.
- `purpose`: `RECEIVING`, `STORAGE`, `PICKING`, `STAGING`, `QC`, or `DAMAGED`.
- `max_weight` and `max_volume`: capacity constraints.
- `is_pickable`: whether the BIN can be used in picking operations.
- `is_active`: active/inactive flag for soft delete logic.

## Business Rules

The implementation defines the following critical rules:

- `BR-WH-01`: location code is unique within the same warehouse.
- `BR-WH-02`: parent and child locations must belong to the same warehouse.
- `BR-WH-03`: hierarchy must follow `ZONE -> AISLE -> RACK -> BIN`.
- `BR-WH-04`: only `BIN` locations can receive inventory references.
- `BR-WH-05`: QC, DAMAGED, RECEIVING, and STAGING bins default to `is_pickable = false`.
- `BR-WH-06`: a location cannot be deactivated while it still contains stock or is still in use by open transactions.
- `BR-WH-07`: warehouse and location records should not be hard-deleted after being used in business transactions.

## Recommended Constraints and Indexes

The ERD also proposes several indexes and validations:

- Unique constraint on `(warehouse_id, code)`.
- Index on `(warehouse_id, parent_id)` to support fast tree traversal.
- Index on `(warehouse_id, location_type, is_active)` to find active BINs quickly.
- Index on `full_path` for fast lookup by position path.
- Validation logic to prevent invalid parent hierarchy and circular references.

These constraints are important because warehouse location data is not just a static list; it is a structural tree that must remain consistent across all stock operations.

## Integration with Other Domains

The warehouse structure connects to other modules as follows:

- Authentication and RBAC: each user role is tied to a warehouse.
- Inventory model: every inventory record must reference a valid BIN in the same warehouse.
- Stock ledger: all stock movements must reference a valid location and remain traceable.
- Product and supplier modules: products are placed into stock via BINs, not directly to a warehouse master record.

This design keeps the system modular and traceable without creating unnecessary direct coupling between modules.

## Example Workflow

An example from the document shows a warehouse `WH01` with:

`Zone Z-A -> Aisle A01 -> Rack R03 -> Bin B05`

The `full_path` for the BIN is:

`WH01/Z-A/A01/R03/B05`

When 120 kg of goods are received, the system can first place them in a receiving BIN. After putaway, the stock is moved from the receiving location to the storage BIN. The total quantity remains the same, but the stock changes location and status according to the warehouse process.

This example demonstrates the importance of accurately modeling the warehouse tree and movement logic.

## Reflection

Day 4 showed that warehouse structure design is not just about creating tables. It is about defining the operational rules that make stock movement, traceability, and warehouse control possible. The design establishes a consistent foundation for future work in inventory management, stock ledger entry creation, and warehouse operation flows.

The most important lesson is that a clean location model reduces operational error. If location hierarchy, purpose, and stock rules are designed correctly, the rest of the warehouse system becomes more reliable and easier to implement.

## Conclusion

This day completed the implementation-level review of the warehouse structure ERD. The team aligned around the use of `warehouses` and `locations`, the self-referencing hierarchy, the strict BIN rule, and the need for validation and soft-delete rules. These decisions provide a strong and realistic foundation for the Mini-WMS inventory domain.
