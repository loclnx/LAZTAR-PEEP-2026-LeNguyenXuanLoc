+++
title = "Day 02 - 22/09/2026 (Database and ORM Study)"
weight = 2
+++

## Tasks Completed

- Studied the database technology used in the project: PostgreSQL 16.
- Reviewed Prisma ORM and its role in the NestJS backend.
- Identified how these two technologies work together in the Mini-WMS system.
- Analyzed their importance for inventory data, transactions, and auditability.

## Overview

The Mini-WMS will use PostgreSQL 16 as its relational database and Prisma ORM as the data-access layer for the backend. This combination is suitable for a warehouse system because the project involves complex relationships, stock transactions, audit history, and real-time inventory consistency.

The database is the system's foundation. It stores all warehouse records such as users, products, batches, stock balances, orders, deliveries, and ledger entries. Prisma then helps the NestJS backend interact with PostgreSQL in a safer and cleaner way using TypeScript.

## PostgreSQL 16

PostgreSQL is a powerful open-source relational database system. In this project, it will be the persistent source of truth for all operational data.

### What PostgreSQL will store

PostgreSQL will store important business records, including:

- Users, roles, and permissions.
- Warehouses and storage locations.
- Products, SKUs, units of measure, and conversions.
- Inventory batches and expiry dates.
- Inventory ledger entries for every movement.
- Goods receipts, putaway records, stock counts, and adjustments.
- Orders, allocations, pick lists, packing, deliveries, and returns.
- Approval logs and audit data.

### Why PostgreSQL is suitable

#### 1. Relational data model

Warehouse data usually contains many relationships. For example, a product can have multiple batches, a warehouse can contain many storage locations, and an order can have several order lines. PostgreSQL supports these structures well and maintains data integrity.

#### 2. Data integrity

PostgreSQL provides primary keys, foreign keys, unique constraints, check constraints, and transactions. This helps prevent invalid or inconsistent records from being saved.

#### 3. Transaction support

A warehouse operation often updates several tables together. For example, confirming a goods receipt may create receipt records, batch records, stock movements, and ledger entries in one transaction. PostgreSQL can commit them together or roll them back if an error occurs.

#### 4. Concurrency control

The system may have multiple users working at the same time. PostgreSQL offers row-level locking and transaction isolation, which helps reduce conflicts when inventory is being allocated or reserved.

#### 5. Numeric precision

This project works with product quantities, weights, and conversion factors. PostgreSQL's NUMERIC type is more reliable than floating-point numbers because it keeps precision and avoids rounding errors in stock calculations.

#### 6. Query capability

Warehouse reports and FEFO queries need to filter stock by product, batch, location, expiry date, and status. SQL is well suited for these queries and supports fast aggregation and summarization.

## Prisma ORM

Prisma ORM is a TypeScript-friendly database toolkit that connects the NestJS backend to PostgreSQL. It allows developers to work with database records in a strongly typed way.

### What Prisma will do

- Define database models, fields, enums, indexes, and relationships in a Prisma schema.
- Generate TypeScript types and a Prisma Client.
- Read and write database records through type-safe queries.
- Create and apply database migrations.
- Support transactional database operations.
- Provide a clean data-access layer for the application.

### Why Prisma is suitable

#### 1. Type safety

Prisma generates TypeScript types from the schema, which helps prevent mistakes when selecting fields or creating records. This reduces many common development errors early.

#### 2. Clear data access

Prisma queries are easier to read than manually written SQL in many standard CRUD operations. This improves team productivity and makes code easier to maintain.

#### 3. Migration management

Prisma Migrate stores schema changes as versioned migrations. This helps developers and CI pipelines keep the PostgreSQL structure aligned across environments.

#### 4. Relationship handling

Prisma makes it easier to work with related data, such as retrieving an order with its lines, allocations, and inventory batches in a single flow.

#### 5. Developer productivity

Prisma reduces repetitive code and allows backend developers to focus more on business logic instead of low-level SQL details.

## How PostgreSQL and Prisma Work Together

The Prisma schema describes the database model. When the schema changes, Prisma Migrate generates migration files that update the PostgreSQL database. Then the NestJS backend uses Prisma Client to read and write records.

```text
NestJS Service
      |
      v
Prisma Client / Prisma ORM
      |
      v
PostgreSQL 16 Database
```

For example, when a warehouse user confirms a goods receipt, a Prisma transaction can:

1. Create the goods receipt record.
2. Add receipt lines.
3. Create batch records when needed.
4. Create ledger entries for the received quantity.
5. Update current stock balances.

If any step fails, PostgreSQL can roll back all changes inside the same transaction. This helps keep the database consistent.

## Importance for This Project

This project depends heavily on accurate inventory logic, so database design matters a lot. PostgreSQL and Prisma provide the right foundation for handling:

- Stock calculations
- Batch tracking and expiry dates
- Inventory ledger integrity
- Order allocation and reservation logic
- Approval and audit workflows
- Transaction safety

### Key design considerations

- Use NUMERIC fields for quantities, weights, and conversion values.
- Keep inventory ledger records append-only.
- Define foreign keys and constraints clearly.
- Add indexes for common queries by product, warehouse, batch, expiry date, and ledger time.
- Wrap inventory-changing operations in transactions.
- Use locking or optimistic concurrency checks to prevent overselling.
- Review Prisma migrations in pull requests and keep them versioned.

## Reflection

Day 2 helped me understand why database technology is one of the most important parts of a warehouse system. A Mini-WMS is not only about UI and business workflows; it is also about storing data correctly and making sure stock information remains reliable under many simultaneous operations.

PostgreSQL gives the system a strong transactional database foundation, while Prisma makes it easier to write safe and maintainable code in NestJS. Together, they are a practical and professional combination for this project.

## Conclusion

This study shows that PostgreSQL 16 and Prisma ORM are essential for building a reliable Mini-WMS. PostgreSQL ensures strong data storage and transaction safety, while Prisma provides a modern, type-safe way to work with that database from the NestJS backend. This combination will support accurate inventory control, traceability, and maintainable software development throughout the project.
