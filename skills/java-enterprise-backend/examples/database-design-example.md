# Database Design Example

## Scenario

The purchase module needs database tables that work across PostgreSQL, MySQL, and domestic innovation database targets. The design must support page queries, approval history, logical delete, optimistic lock, and reliable domain events.

## User Prompt

```text
Use $java-enterprise-backend to design database tables for a purchase application module.

Required tables:
- purchase_application
- purchase_application_item
- purchase_approval_record
- domain_event_message or the project equivalent local message table if no reliable event infrastructure exists

Business requirements:
- Applications have code, title, applicant user, applicant department, expected date, total amount, status, submit time, approve/reject/revoke time, and remark.
- Items have name, specification, quantity, unit price, amount, and sort order.
- Approval records store action, operator, comment/reason, operation time, and previous/next status.
- Domain events must support reliable publish for submitted and approved events.

Please output:
- Table fields, recommended types, nullability, defaults, and comments.
- Primary keys, unique constraints, indexes, and logical delete strategy.
- Optimistic lock and audit fields.
- Status enum values and allowed transitions.
- PostgreSQL/MySQL/domestic database compatibility notes.
- Migration SQL guidance and rollback notes.
- Risks for JSON, text/clob, timestamp precision, generated columns, partial indexes, upsert, and vendor-specific functions.
```

## Expected Codex Behavior

- Inspect existing migration tool and database naming conventions before writing SQL.
- Prefer portable column types and avoid vendor-specific syntax by default.
- Include base fields such as id, create_time, create_by, update_time, update_by, deleted, version, and remark where project conventions use them.
- Design indexes around real query predicates and stable pagination.
- Explain how unique constraints interact with logical delete.
- Mark compatibility risks instead of hiding them.

## Output Focus

- Table-by-table schema proposal.
- Indexes mapped to page query, detail, code lookup, status filters, and event dispatch queries.
- Forward migration SQL notes and rollback notes.
- Compatibility risk list for PostgreSQL, MySQL, and domestic database targets.
- Mapper/entity implications and test suggestions.
