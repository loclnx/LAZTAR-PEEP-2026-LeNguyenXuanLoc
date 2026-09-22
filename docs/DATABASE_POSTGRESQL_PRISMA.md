# Database Technology PostgreSQL 16 and Prisma ORM

## Overview

The Mini-WMS will use PostgreSQL 16 as its relational database and Prisma ORM as the database toolkit for the NestJS backend. Together, they provide a reliable way to store warehouse data, enforce relationships between business records, and access data through type-safe TypeScript code.

## PostgreSQL 16

PostgreSQL is an open-source relational database system. In this project, it will be the persistent source of data for warehouse operations.

### What PostgreSQL Will Store

PostgreSQL will store the core data needed by the Mini-WMS, including:

- Users, roles, and permissions.
- Warehouses and storage locations.
- Products, SKUs, units of measure, and unit conversions.
- Inventory batches, expiry dates, and stock balances.
- Inventory ledger entries for every stock movement.
- Goods receipts, putaway activities, stock counts, and adjustment requests.
- Customer orders, allocations, pick lists, packages, deliveries, and returns.
- Approval records and audit information.

### Why PostgreSQL Is Suitable

- **Relational data model:** Warehouse data contains many relationships, such as a product having many batches and an order containing many order lines. PostgreSQL is designed to manage these relationships reliably.
- **Data integrity:** Primary keys, foreign keys, unique constraints, check constraints, and transactions help prevent invalid or incomplete records.
- **Transaction support:** A warehouse transaction can update several records together. For example, allocating stock may create allocation records, update reserved quantities, and add a ledger entry. PostgreSQL can commit all of these changes together or roll them all back if an error occurs.
- **Concurrency control:** PostgreSQL provides row locking and transaction isolation mechanisms that can help prevent two users from reserving the same inventory at the same time.
- **Numeric precision:** Warehouse quantities and catch weights require decimal values. PostgreSQL's `NUMERIC` type is appropriate for quantities that must not lose precision through floating-point calculations.
- **Query capability:** SQL can efficiently filter and aggregate stock by product, batch, expiry date, location, and inventory status. This is important for FEFO allocation and inventory reporting.

## Prisma ORM

Prisma ORM is a TypeScript-friendly database toolkit. It connects the NestJS application to PostgreSQL and lets backend code work with database records through a generated, type-safe client.

### What Prisma Will Do

- Define database tables, columns, enums, indexes, and relationships in a Prisma schema.
- Generate TypeScript types and a Prisma Client from that schema.
- Read and write PostgreSQL records through clear TypeScript queries.
- Create and apply versioned database migrations.
- Run related database operations inside transactions.
- Provide a consistent data-access layer for NestJS services.

### Why Prisma Is Suitable

- **Type safety:** Prisma generates types from the database schema, helping developers catch invalid fields, missing values, and incompatible data earlier during development.
- **Clear data access:** Queries are written as TypeScript objects rather than manually assembled SQL strings for common create, read, update, and delete operations.
- **Migration management:** Prisma Migrate keeps database schema changes version-controlled so each developer and CI environment can apply the same structure.
- **Relationship handling:** Prisma makes it easier to work with related records, such as an order together with its lines, allocations, and inventory batches.
- **Developer productivity:** It reduces repetitive database-access code while still allowing raw SQL when a complex, performance-sensitive query is necessary.

## How PostgreSQL and Prisma Work Together

The Prisma schema will describe the intended PostgreSQL structure. When the schema changes, Prisma Migrate will generate a migration that updates the PostgreSQL database. NestJS services will then use Prisma Client to perform database operations.

```text
NestJS Service
      |
      v
Prisma Client and Prisma ORM
      |
      v
PostgreSQL 16 Database
```

For example, when a warehouse user confirms a goods receipt, the backend can use a single Prisma transaction to:

1. Create the goods-receipt record and its lines.
2. Create the related batch records when required.
3. Create inventory ledger entries for the received quantities.
4. Update the current stock balance or stock projection.

If one of these steps fails, PostgreSQL can roll back the complete transaction so the database is not left in a partially updated state.

## Important Design Considerations for This Project

- Use `NUMERIC` fields for quantities, weights, and conversion factors instead of floating-point values.
- Keep inventory ledger entries append-only. Corrections should create reversing entries rather than editing historical movements.
- Enforce foreign keys and database constraints for core relationships and valid values.
- Add indexes for common warehouse queries, especially product, warehouse, location, batch, expiry date, document status, and ledger timestamps.
- Use transactions for every workflow that changes inventory.
- Apply locking or optimistic concurrency checks when allocating or deducting stock to prevent negative inventory.
- Treat Prisma migrations as source-controlled project artifacts and review them in pull requests.
- Use raw SQL carefully for advanced reporting or locking cases that are not expressed clearly through the ORM.

## Expected Outcome

PostgreSQL 16 will provide the reliable transactional database foundation for the Mini-WMS. Prisma ORM will make that foundation easier and safer to use from the TypeScript backend. The combination will support accurate inventory records, traceable warehouse operations, and maintainable backend development.
