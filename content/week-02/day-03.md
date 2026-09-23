+++
title = "Day 03 - 23/09/2026 (Warehouse Structure Design)"
weight = 3
+++

## Tasks Completed

- Reviewed the warehouse structure implementation document for Mini-WMS.
- Studied the warehouse and location design as the foundation of inventory control.
- Clarified the self-referencing location hierarchy and business rules.
- Identified key warehouse constraints for pickability, stock validity, and traceability.
- Drew and reviewed the business flow for warehouse structure and related operations.

## Overview

This day focused on the warehouse structure domain, which is the foundation for stock visibility, location-based inventory, and operational traceability in the Mini-WMS. The document defines how warehouses and storage locations are modeled so that inventory, picking, receiving, QC, and damaged goods can be managed consistently.

## Key Design Decisions

### 1. Warehouse and location model

The system uses two main tables: `warehouses` and `locations`. The `locations` table is self-referencing, which allows the system to represent the real warehouse hierarchy without creating separate tables for zone, aisle, rack, and bin.

### 2. Standard physical hierarchy

The standard structure is:

`WAREHOUSE -> ZONE -> AISLE -> RACK -> BIN`

This structure is easy to understand for warehouse operations and is flexible enough for future expansion. Only bin-level locations are used to hold inventory.

### 3. Picking and status control

Not every location is allowed to be used for picking. Only `BIN` locations with the correct purpose and `is_pickable = true` can be selected for order picking. Areas such as QC, damaged, receiving, and staging are used for control and quarantine rather than normal stock allocation.

## Business Rules

- `BR-B-01`: location code must be unique within the same warehouse.
- `BR-B-02`: child and parent locations must belong to the same warehouse.
- `BR-B-03`: hierarchy must follow the correct sequence: `ZONE -> AISLE -> RACK -> BIN`.
- `BR-B-04`: inventory can only be stored at the `BIN` level.
- `BR-B-05`: QC, damaged, receiving, and staging bins must not be pickable.
- `BR-B-06`: a location cannot be disabled while stock remains in it.
- `BR-B-07`: warehouse and location records should not be hard-deleted after being used in business transactions.

## Integration with Other Domains

The warehouse structure is tightly connected to other system areas:

- Authentication & RBAC: access is restricted by warehouse.
- Inventory model: every stock record must map to a valid BIN in the same warehouse.
- Stock ledger: every putaway, transfer, and adjustment must remain consistent with the warehouse location tree.
- Product & supplier modules: the warehouse structure supports traceability without creating direct coupling.

## Business Flow Diagram

The business flow for the warehouse structure and stock movement was drawn and reviewed during this session.

- Diagram link: [Business Flow Diagram](https://app.diagrams.net/#G1Vc7Tz9084zgDJpeSOipud6UmZ0-48kWH#%7B%22pageId%22%3A%223coEni-nVhuQpga9hQnN%22%7D)

## Reflection

Day 3 showed that the warehouse structure is not a simple database table. It is the operational backbone of the inventory system. If the location model is designed properly, the team can improve stock accuracy, prevent wrong allocation, and make warehouse activities easier to control and trace.

## Conclusion

This day concluded the warehouse structure design for the Mini-WMS. The team agreed on a clear hierarchy, strict stock rules, and important integration points needed for future implementation in inventory, stock ledger, and warehouse operations.
