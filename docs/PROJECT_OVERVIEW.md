# Mini-WMS Project Overview

## Project Purpose

This project will build a Mini Warehouse Management System (Mini-WMS) for FreshLink Produce, a fictional fresh-produce distributor. The application will simulate the daily operations of a real produce warehouse, where product freshness, batch traceability, accurate stock quantities, and timely fulfillment are essential.

The goal is to deliver a working warehouse application that manages products from inbound receipt through storage, order fulfillment, delivery, and returns.

## Warehouse Workflow

The system will support the following end-to-end process:

**Goods Receipt → Putaway → Inventory Allocation → Picking → Packing → Delivery → Returns**

Every operation that changes inventory will be recorded in a central inventory ledger. This provides a complete audit trail and ensures that stock changes are controlled consistently across the application.

## Main Features

### User Access and Master Data

- User login and role-based access control.
- Management of warehouses, storage locations, products/SKUs, batches, and units of measure.
- Unit conversion support, such as ordering by cartons while storing inventory in kilograms.

### Inventory Management

- A single inventory ledger for recording all stock movements.
- Visibility of physical stock, reserved stock, and available stock.
- Batch-level stock tracking, including expiry dates.
- Inventory adjustments and stock counts with an approval process.
- Protection against duplicate opening-stock imports.
- Concurrency-safe inventory updates to prevent negative stock when multiple users work at the same time.

### Inbound Operations

- Create and confirm goods receipts.
- Increase inventory based on received quantities.
- Assign received goods to warehouse locations through putaway.
- Track received products by batch and expiry date.

### Outbound Fulfillment

- Allocate inventory to customer orders.
- Use the FEFO rule (First Expired, First Out), which prioritizes products with the nearest expiry date.
- Handle shortages by allowing partial delivery, product substitution, or backorder delivery.
- Support picking and packing workflows.
- Record catch weight, so final inventory and delivery quantities can use the actual measured weight rather than only the requested quantity.

### Delivery and Returns

- Confirm completed deliveries and deduct the delivered quantity from inventory.
- Process returned goods and record the related inventory movement.
- Preserve the history of every delivery and return transaction for traceability.

## Business Rules the System Will Enforce

The Mini-WMS will focus on practical warehouse rules that are especially important for fresh products:

- Available inventory is different from physical inventory because some stock may already be reserved for other orders.
- Products must be converted correctly between ordering and storage units.
- Products must be allocated using FEFO instead of FIFO when expiry dates matter.
- Actual catch weight may differ from the quantity originally ordered.
- Inventory corrections require approval and must remain traceable.
- Inventory ledger records are append-only; mistakes are corrected by creating reversing transactions rather than editing or deleting history.
- All inventory changes must pass through one inventory service to keep stock calculations consistent.

## Planned Technology Stack

- **Backend:** NestJS with TypeScript
- **Database:** PostgreSQL 16 with Prisma ORM
- **Frontend:** React with TypeScript on Next.js, designed with mobile-first operational screens
- **Local environment:** Docker Compose
- **Continuous integration:** GitHub Actions

## Expected Result

At the end of the project, the team will have a deployable Mini-WMS prototype that can demonstrate a complete warehouse scenario: receive fresh produce, store it by batch, allocate stock to an order according to expiry date, pick and pack the order, record actual weight, deliver it, and process a return when needed.

## Expected Implementation Challenges

### Technical Challenges

- **Maintaining data consistency:** Inventory is shared by many workflows. The system must ensure that receiving, allocation, picking, delivery, returns, and adjustments do not create contradictory stock data.
- **Concurrent updates:** Multiple warehouse users may allocate or pick the same stock at the same time. Database transactions, locking, or optimistic concurrency controls will be needed to prevent overselling and negative inventory.
- **Inventory ledger design:** The ledger must be append-only and reliable. A mistaken transaction cannot simply be edited or deleted; the system needs reversing entries while keeping balances accurate.
- **Accurate stock calculations:** Physical, reserved, available, picked, delivered, and returned quantities must be calculated correctly for each product, batch, and location.
- **Batch and expiry tracking:** Queries and database models must efficiently identify the correct stock batch for FEFO allocation, especially when a product is stored in several locations and batches.
- **Unit conversion and decimal precision:** Product quantities may move between cartons, pieces, kilograms, and actual catch weight. The application must avoid rounding errors by using appropriate decimal types and conversion rules.
- **API and frontend workflow coordination:** The backend must expose clear, safe workflow endpoints while the mobile-first interface must guide warehouse staff through actions in the correct order and prevent invalid operations.
- **Testing complex scenarios:** Unit, integration, and end-to-end tests need to cover stock movements, concurrent requests, approval flows, partial fulfillment, and failure recovery.
- **Local environment and CI reliability:** Docker Compose, PostgreSQL, Prisma migrations, and GitHub Actions must work consistently for every team member and on automated checks.

### System Logic and Business Challenges

- **Defining a single source of truth:** The inventory ledger must be the authoritative record of stock movement. Other screens and reports must derive their balances consistently from it rather than directly changing inventory in multiple places.
- **Separating inventory states:** Physical stock is not always available stock. Reserved stock, allocated stock, picked stock, damaged stock, and returned stock must be modeled distinctly so the system does not promise the same quantity to multiple orders.
- **Applying FEFO correctly:** FEFO is based on the earliest expiry date, not the earliest receipt date. The logic must also define how to handle missing expiry dates, expired products, and equal expiry dates.
- **Handling shortages consistently:** Partial delivery, substitutions, and backorders each affect reservations, order status, stock balances, and customer commitments differently. Clear rules are required before implementation.
- **Supporting catch weight:** The final measured weight can differ from the requested quantity. The system must decide how this affects allocated stock, delivered quantity, pricing data if applicable, and the remaining balance.
- **Approval workflow for adjustments:** Stock adjustments require separation of duties. The logic must define who can request, approve, reject, or reverse an adjustment and when inventory becomes effective.
- **Managing returns:** Returned goods may be sellable, damaged, expired, or require inspection. The return process needs rules for deciding whether stock can re-enter available inventory.
- **Keeping state transitions valid:** Documents such as receipts, orders, pick lists, packages, deliveries, and returns must move through controlled statuses. The system must block invalid transitions, such as delivering an order before it is picked.
- **Traceability and auditability:** Every stock-affecting action must identify the time, user, reason, source document, product, batch, location, and quantity so warehouse discrepancies can be investigated.
