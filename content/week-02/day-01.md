+++
title = "Day 01 - 21/09/2026 (ON-SITE) Project Overview"
weight = 1
+++

## Tasks Completed

- Reviewed the project overview for the Mini-WMS system.
- Studied the warehouse workflow from receipt to return.
- Identified the key business rules, operational requirements, and technical stack.
- Discussed how the project will manage inventory, batches, FEFO allocation, and product traceability.

## Project Overview

This project focuses on building a Mini Warehouse Management System for FreshLink Produce, a fictional fresh-produce distributor. The main goal is to simulate the real operating flow of a warehouse that handles fresh goods, including stock accuracy, product freshness, batch traceability, and timely order fulfillment.

The system is expected to support a complete warehouse cycle: Goods Receipt → Putaway → Inventory Allocation → Picking → Packing → Delivery → Returns.

## Key Learning Points

### 1. Warehouse workflow

The business process begins with receiving goods, then moving them into storage locations, allocating stock for customer orders, and finally preparing and delivering the order. Returned items are also processed back into the inventory flow with full traceability.

### 2. Inventory control requirements

This project emphasizes the difference between physical stock, reserved stock, and available stock. Inventory is not only about quantity, but also about how stock can be used safely and correctly in real warehouse operations.

### 3. Batch and expiry management

Fresh products require strict handling of batch numbers and expiry dates. The project will apply FEFO (First Expired, First Out) logic instead of simple FIFO to ensure the nearest expiry date is prioritized during allocation and picking.

### 4. Operational traceability

Every inventory movement must be recorded in a central ledger so that the system can trace the origin, movement, and final usage of each batch. This also supports auditability and helps prevent incorrect stock correction.

### 5. Business rules and controls

The system will enforce several practical rules:

- Available stock is different from physical stock.
- Product units must be converted properly between order and storage units.
- Catch weight may differ from the ordered quantity.
- Stock corrections require approval and remain traceable.
- Inventory history is append-only; changes are recorded through reversing transactions instead of direct modification.
- All inventory changes must go through a central inventory service.

### 6. Expected implementation challenges

The project also introduces several important technical and business challenges that must be considered during implementation:

#### Technical challenges

- Maintaining data consistency across receiving, allocation, picking, delivery, returns, and inventory adjustments.
- Preventing concurrent stock updates that could cause overselling or negative inventory.
- Designing an append-only inventory ledger that records reversing entries instead of modifying the original transaction history.
- Calculating physical, reserved, available, picked, delivered, and returned stock correctly for each product, batch, and location.
- Supporting FEFO queries efficiently when stock is stored across multiple locations and batches.
- Handling unit conversion and decimal precision correctly, especially when moving between cartons, kilograms, and catch weight.
- Coordinating backend APIs and mobile-first frontend flows to guide staff through valid warehouse procedures.
- Testing complex scenarios such as partial fulfillment, approval workflows, and failure recovery.
- Ensuring the local Docker environment and GitHub Actions pipeline remain stable for all team members.

#### System logic and business challenges

- Defining a single source of truth for inventory by ensuring the ledger remains the authoritative record of stock movement.
- Separating inventory states clearly so reserved, picked, damaged, and returned stock do not overlap with available stock.
- Applying FEFO correctly based on earliest expiry date rather than receipt order.
- Handling shortages consistently through partial delivery, substitution, or backorder procedures.
- Managing catch-weight differences between ordered quantity and actual measured weight.
- Implementing an approval workflow for inventory corrections and stock adjustments.
- Defining rules for returned goods so they can re-enter inventory only when valid.
- Enforcing correct state transitions between receipts, orders, pick lists, deliveries, and returns.
- Preserving traceability with complete audit information such as time, user, reason, source document, batch, location, and quantity.

## Technical Stack

The project is planned to use:

- Backend: NestJS with TypeScript
- Database: PostgreSQL 16 with Prisma ORM
- Frontend: React + TypeScript with Next.js
- Local environment: Docker Compose
- CI/CD: GitHub Actions

## Reflection

Week 2 Day 1 helped me understand that this project is not just a simple CRUD application. It is a warehouse operational system that requires careful business logic, data consistency, and accounting-style traceability. The biggest takeaway is that inventory management for fresh goods must be planned around real warehouse constraints, not just basic stock counts.

## Conclusion

This day focused on understanding the real purpose of the project and the operational logic behind it. The Mini-WMS project aims to provide a functional and realistic warehouse simulation, where stock movement, expiry control, fulfillment flow, and traceability are all handled systematically.
