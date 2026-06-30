---
name: java-enterprise-backend
description: Use when generating, reviewing, refactoring, or designing Java 21 Spring Boot 3.x enterprise backend code involving MyBatis-Plus, PostgreSQL/MySQL/domestic databases, Redis, RabbitMQ, Sa-Token permissions, transactions, APIs, database tables, production readiness, or Chinese enterprise information systems.
---

# Java Enterprise Backend

## Overview

Use this skill for pragmatic enterprise backend work in Java 21 + Spring Boot 3.x systems serving Chinese enterprise, government, state-owned enterprise, and domestic innovation-compatible environments.

Prefer project conventions over generic architecture patterns. Read the existing code, configuration, database style, permission style, exception model, and tests before generating or changing code.

## Operating Rules

- Inspect first: package layout, base response wrapper, exception classes, MyBatis-Plus config, transaction style, Sa-Token usage, Redis/RabbitMQ wrappers, migration scripts, and tests.
- Make the smallest maintainable change that satisfies the task.
- Preserve public API compatibility unless the user explicitly asks to change it.
- Use Java 21 and Spring Boot 3.x compatible code. Prefer `jakarta.*` over outdated `javax.*`.
- Do not introduce dependencies, workflow engines, code generators, or architecture layers without clear project evidence or user approval.
- Do not return Entity objects directly to frontend APIs.
- Do not hide business logic in utility classes.
- Always mention tests or verification commands. Never claim tests passed unless actually run.

## Workflow

1. Explore: read related files, call chain, DTO/VO/Entity/Mapper XML, configuration, tests, and docs.
2. Plan: state the small set of files to change and what will not be changed.
3. Implement: follow existing project naming, layering, response wrapper, exception, logging, and permission conventions.
4. Review: check API contract, validation, permission, data scope, transaction boundaries, database compatibility, Redis/RabbitMQ reliability, logs, and sensitive data handling.
5. Verify: run relevant tests, build, lint, or a minimal compile check when feasible.
6. Report: summarize changes, why they were made, verification results, risks, and follow-up suggestions.

## Standards Navigation

Load the relevant template files when the task touches that area:

- Project/package layout: `templates/01-project-structure-standard.md`
- REST API contracts: `templates/02-api-standard.md`
- Database/table/SQL design: `templates/03-database-standard.md`
- MyBatis-Plus usage: `templates/04-mybatis-plus-standard.md`
- Transaction boundaries and consistency: `templates/05-transaction-standard.md`
- Sa-Token permission and data scope: `templates/06-sa-token-standard.md`
- Redis keys, cache, locks, idempotency, rate limits: `templates/07-redis-standard.md`
- RabbitMQ events, retry, dead-letter, idempotency: `templates/08-rabbitmq-standard.md`
- Review output checklist: `templates/09-code-review-checklist.md`
- Agent generation behavior: `templates/10-codex-generation-rules.md`

Use examples only when a concrete pattern helps:

- Business module generation: `examples/module-generation-example.md`
- Code review: `examples/code-review-example.md`
- API design: `examples/api-design-example.md`
- Database design: `examples/database-design-example.md`
- MyBatis-Plus query and mapper review: `examples/mybatis-plus-example.md`
- Transaction handling: `examples/transaction-example.md`
- Sa-Token permission and data-scope review: `examples/permission-example.md`
- Redis cache, lock, idempotency, captcha, and rate-limit review: `examples/redis-review-example.md`
- RabbitMQ producer/consumer reliability review: `examples/rabbitmq-review-example.md`
- Xinchuang compatibility review: `examples/xinchuang-compatibility-example.md`

Each example is intentionally copyable and includes scenario, user prompt, expected Codex behavior, and output focus. Load only the example that matches the user's current task.

## Core Architecture Rules

Recommended package structure:

```text
com.company.project
+-- common
|   +-- core
|   +-- exception
|   +-- response
|   +-- validation
|   +-- security
|   +-- utils
+-- config
+-- module
|   +-- purchase
|   |   +-- controller
|   |   +-- service
|   |   +-- service/impl
|   |   +-- mapper
|   |   +-- entity
|   |   +-- dto
|   |   +-- vo
|   |   +-- query
|   |   +-- convert
|   |   +-- enums
|   +-- system
+-- Application.java
```

Layer responsibilities:

- Controller: request binding, validation annotations, permission annotations, and Service calls only.
- Service: business orchestration, validation, transaction boundary, and state transitions.
- Mapper: database access only.
- Entity: database table mapping only.
- DTO: create, update, and action request objects.
- Query: list and page query conditions.
- VO: response objects.
- Convert: object conversion.
- Enum: business status and type constants.

Forbidden:

- Controller directly calling Mapper.
- Controller containing business logic.
- Returning Entity directly to frontend.
- Service returning raw `Map` unless explicitly justified.
- Business code scattered across util classes.

## API Rules

Use a unified response shape unless the project already has one:

```json
{
  "code": 0,
  "message": "success",
  "data": {},
  "traceId": "xxx"
}
```

Use stable page responses:

```json
{
  "total": 100,
  "pageNum": 1,
  "pageSize": 10,
  "records": []
}
```

Prefer REST resource/action URLs:

```text
GET    /api/purchase/applications/page
GET    /api/purchase/applications/{id}
POST   /api/purchase/applications
PUT    /api/purchase/applications/{id}
DELETE /api/purchase/applications/{id}
POST   /api/purchase/applications/{id}/submit
POST   /api/purchase/applications/{id}/approve
POST   /api/purchase/applications/{id}/reject
POST   /api/purchase/applications/{id}/revoke
```

Avoid `/add`, `/update`, `/delete`, `/getList`, and `/doApprove` unless maintaining legacy compatibility.

## Database Rules

Design for PostgreSQL 18, MySQL 8.0, and domestic database compatibility:

- Use `snake_case` table and column names.
- Avoid database keywords and vendor-specific functions by default.
- Default to logical delete for business tables.
- Important business tables need audit fields.
- Prefer readable status enum values over magic numbers.
- Never use `float` or `double` for money. Use `decimal(18,2)` or project equivalent.
- Mark compatibility risk for `json`, arrays, recursive queries, generated columns, full-text search, `text`/`clob`, and vendor-specific SQL.

Recommended base fields: `id`, `create_time`, `create_by`, `update_time`, `update_by`, `deleted`, `version`, `remark`.

## MyBatis-Plus Rules

```java
@TableName("purchase_application")
public class PurchaseApplicationEntity {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;

    @TableLogic
    private Integer deleted;

    @Version
    private Integer version;
}
```

- Simple CRUD may use MyBatis-Plus Service methods.
- Complex queries, joins, reports, permission-filtered queries, and aggregations should use Mapper XML.
- Use `LambdaQueryWrapper` for simple typed conditions.
- Avoid unreadable complex wrappers in Service.
- Never concatenate SQL strings in Service.
- Mapper method names must describe business intent.
- Pagination must be explicit and stable.

## Transaction Rules

- Write operations must define transaction boundaries.
- Submit, approve, reject, revoke, import, batch update, and payment-like operations must be transactional.
- Use `@Transactional(rollbackFor = Exception.class)` on public Service methods.
- Avoid self-invocation transaction failure.
- Do not swallow exceptions in transactional methods.
- If failure logs must persist after rollback, use `REQUIRES_NEW` or write outside the main transaction.
- Do not send RabbitMQ messages directly inside a database transaction without a reliability strategy.
- Prefer transaction-after-commit event, local message table, or a reliable event mechanism.

## Permission Rules

Permission code style:

```text
purchase:application:list
purchase:application:view
purchase:application:create
purchase:application:update
purchase:application:delete
purchase:application:submit
purchase:application:approve
purchase:application:reject
purchase:application:export
```

- Backend permission checks are mandatory.
- Frontend button hiding is not security.
- Separate menu permissions, button permissions, and data permissions.
- APIs involving "my data", "department data", or "all data" must implement data scope filtering.
- Use `@SaCheckPermission` on protected controller methods unless the project has centralized permission interception.
- Do not trust `userId`, `orgId`, or `tenantId` from frontend when they can be derived from login context.

## Redis Rules

Allowed use cases: Sa-Token/session, captcha, short-term cache, distributed lock, idempotency token, hot dictionaries, and rate limiting.

- Redis keys must have a business prefix.
- Temporary keys must have TTL.
- Cache object compatibility must be considered.
- Distributed locks must have timeout and unique value.
- Do not cache sensitive data unless necessary and protected.
- Describe cache invalidation strategy.

Key examples:

```text
project:captcha:{uuid}
project:dict:{dictType}
project:idempotent:{token}
project:lock:purchase-submit:{id}
```

## RabbitMQ Rules

Event naming:

```text
purchase.application.submitted
purchase.application.approved
purchase.application.rejected
```

Message wrapper:

```java
public class DomainEvent<T> {
    private String eventId;
    private String eventType;
    private LocalDateTime occurredAt;
    private T payload;
}
```

- Every message must have `messageId` or `eventId`.
- Consumers must be idempotent.
- Failure handling must include retry or dead-letter strategy.
- Do not use Entity as message payload.
- Do not put long unpredictable transactions inside consumers.
- Consider delivery reliability when messages are triggered by database changes.

## Exceptions And Logging

Recommended exceptions: `BusinessException`, `ValidationException`, `NotFoundException`, `ForbiddenException`, and `SystemException`.

- Use a global exception handler.
- Business exceptions must have clear error codes.
- Do not expose raw database exceptions to frontend.
- Do not use `e.printStackTrace()`.
- Do not swallow exceptions.
- Error messages should be clear for Chinese enterprise users when applicable.
- Use SLF4J.
- Logs must include `traceId` when available.
- Important operations must include `userId`, `businessId`, `operation`, and `result`.
- Approval, import/export, message sending, and permission-sensitive operations need business logs.
- Do not log passwords, tokens, ID cards, phone numbers, signatures, or sensitive fields in plain text.

## Domestic Innovation Compatibility

- Avoid database-specific syntax unless explicitly accepted.
- Avoid strong MySQL-only functions.
- Mark compatibility risks for JSON fields, recursive queries, array fields, generated columns, full-text search, and vendor-specific SQL.
- File paths must be Linux compatible.
- Do not assume x86 architecture.
- Avoid native libraries that may not support domestic operating systems or CPUs.
- Prefer standard SQL and application-layer compatibility.

## Review Checklist

Use this checklist for generation and review:

- Package structure correct?
- Controller thin enough?
- DTO/VO/Entity separated?
- Validation complete?
- Permission annotation present?
- Data scope considered?
- Transaction boundary correct?
- MyBatis-Plus usage appropriate?
- Complex SQL in XML?
- Logical delete and optimistic lock considered?
- Redis key and TTL correct?
- RabbitMQ idempotency and retry considered?
- Exceptions handled by global handler?
- Logs include `traceId` and business identifiers?
- Sensitive data protected?
- Domestic database compatibility risk marked?
- Unit/integration test suggestions provided?
