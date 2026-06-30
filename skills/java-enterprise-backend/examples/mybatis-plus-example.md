# MyBatis-Plus Review Example

## Scenario

A module uses MyBatis-Plus for basic CRUD and Mapper XML for permission-filtered page queries. The user wants to check whether query logic is readable, safe, compatible across databases, and aligned with project conventions.

## User Prompt

```text
Use $java-enterprise-backend to review MyBatis-Plus usage in the purchase application module.

Review targets:
- PurchaseApplicationEntity
- PurchaseApplicationMapper
- PurchaseApplicationMapper.xml
- PurchaseApplicationServiceImpl page, detail, update, submit, approve, and reject methods

Check:
- @TableName, @TableId, @TableLogic, @Version, fill fields, and type choices.
- Whether simple conditions use LambdaQueryWrapper clearly.
- Whether complex joins, data-scope filtering, aggregation, and report queries are moved to Mapper XML.
- SQL injection risk from ${}, raw order fields, string concatenation, or frontend-controlled columns.
- Stable pagination ordering.
- Logical delete and optimistic lock behavior.
- PostgreSQL/MySQL/domestic database compatibility.
- Whether Service contains unreadable SQL-building logic that belongs in Mapper XML.

Return findings first. If changes are needed, make the smallest patch and run the most relevant test or compile command.
```

## Expected Codex Behavior

- Inspect MyBatis-Plus configuration, meta object handler, mapper scan, pagination plugin, and existing mapper style.
- Keep simple wrappers readable and move complex permission-filtered queries to XML.
- Avoid SQL string concatenation in Service.
- Verify logical delete and optimistic lock assumptions against existing config.
- Mark vendor-specific SQL and pagination risks.

## Output Focus

- Entity mapping issues and field annotation gaps.
- Wrapper readability and safety findings.
- Mapper XML SQL injection, ordering, pagination, and compatibility findings.
- Minimal patch suggestions for wrappers, XML parameters, indexes, and tests.
- Verification commands or reasons tests were not run.
