# 03 Database Standard

Design schemas for PostgreSQL 18, MySQL 8.0, and domestic innovation database compatibility.

## Naming

- Use `snake_case` for tables, columns, indexes, and constraints.
- Table names should be semantic: `purchase_application`, `purchase_approval_record`.
- Avoid database keywords, ambiguous abbreviations, and vendor-specific case sensitivity.

## Base Fields

Recommended business table fields:

```text
id
create_time
create_by
update_time
update_by
deleted
version
remark
```

Recommended types:

| Field | Type guidance |
| --- | --- |
| `id` | `bigint` or compatible numeric ID |
| `deleted` | `smallint default 0` |
| `version` | `integer default 0` |
| `status` | `varchar(32)` with readable enum values |
| `amount` | `decimal(18,2)` |
| time fields | `timestamp` |
| business code/no | `varchar(64)` |

## Required Rules

- Default to logical delete for business tables.
- Important business tables must include audit fields.
- Status fields should use readable enum values instead of magic numbers.
- Money must not use `float` or `double`.
- Avoid strong dependency on one database function or syntax.
- Use indexes for business codes, foreign keys, status filters, and common page query predicates.
- Keep unique constraints aligned with logical delete rules. For cross-database compatibility, verify how the target database handles partial indexes or functional indexes before using them.

## Compatibility Risk Markers

Mark and explain compatibility risks when using:

- JSON fields
- Array fields
- Recursive queries
- Generated columns
- Full-text search
- Vendor-specific SQL functions
- `text` or `clob` fields
- Database-specific pagination or upsert syntax

## Migration Guidance

- Provide forward migration SQL and rollback notes where project practice requires it.
- Avoid inserting explicit identity values unless the database and table strategy support it.
- Keep scripts idempotent where possible.
- Do not assume MySQL-only syntax in mixed database projects.
- Verify timestamp precision and timezone behavior for the target database.

## Review Questions

- Does the schema support PostgreSQL, MySQL, and domestic database targets?
- Are audit, logical delete, and optimistic lock fields present?
- Are enum values readable and documented?
- Are indexes aligned with actual queries?
- Are compatibility risks explicitly marked?
