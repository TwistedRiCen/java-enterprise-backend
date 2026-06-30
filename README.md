# java-enterprise-backend

Internal backend engineering standards for Java 21 + Spring Boot 3.x enterprise information systems in domestic innovation-compatible environments.

This repository contains one AI Skill:

- `java-enterprise-backend`: guides Codex, Claude Code, and similar agents when generating, reviewing, refactoring, or designing Java enterprise backend code.

## Target Stack

- JDK 21
- Spring Boot 3.x
- MyBatis-Plus
- PostgreSQL 18
- MySQL 8.0
- Domestic innovation databases and compatibility-oriented SQL
- Redis
- RabbitMQ
- Sa-Token
- Maven
- Linux and domestic server operating systems
- No unified workflow engine assumed

## What This Skill Does

Use this skill to enforce pragmatic backend conventions for enterprise systems, government and state-owned enterprise projects, and xinchuang-compatible deployments.

It supports:

- Code generation for Spring Boot modules
- Code review for Controller, Service, Mapper, Entity, DTO, VO, Query, Convert, and Enum layers
- Refactoring with minimal behavioral changes
- API design and response contract review
- Database table and SQL design review
- Transaction boundary review
- Sa-Token permission and data-scope review
- Redis cache, lock, idempotency, captcha, and rate-limit review
- RabbitMQ event and consumer reliability review
- Xinchuang compatibility review for database, OS, CPU architecture, middleware, file paths, and native dependencies
- Production-readiness review

## Current Example Gaps Found

The first version had useful short prompts, but the usage examples were not strong enough for real enterprise Spring Boot work:

- Examples were mostly concept notes, not complete copyable prompts.
- Several examples lacked scenario, expected Codex behavior, and output focus.
- Business module generation was mentioned but not shown with project discovery, files, permissions, transactions, tests, and migration notes.
- Database design, Redis review, RabbitMQ review, and xinchuang compatibility review were missing as standalone examples.
- Review examples did not consistently require findings first, severity, file/line references, root cause, patch suggestions, and verification commands.
- Transaction, permission, Redis, and MQ scenarios were too narrow to guide complex enterprise flows such as approval, idempotency, cache invalidation, delayed messages, retry, and dead-letter handling.

## Installation

```bash
npx skills@latest add https://github.com/TwistedRiCen/java-enterprise-backend --skill java-enterprise-backend
```

## Recommended Install Command

Use the repository URL and install only this skill:

```bash
npx skills@latest add https://github.com/TwistedRiCen/java-enterprise-backend --skill java-enterprise-backend
```

After installation, call it explicitly in prompts:

```text
Use $java-enterprise-backend to review the current Spring Boot module for transaction, permission, Redis, RabbitMQ, database, and xinchuang compatibility risks. Read the related code first, make minimal changes only if needed, and run the most relevant verification command.
```

## Copyable Usage Examples

The full examples live in `skills/java-enterprise-backend/examples/`. Each one includes scenario, user prompt, expected Codex behavior, and output focus.

### Generate A Business Module

```text
Use $java-enterprise-backend to generate an enterprise purchase application module in this Spring Boot project.

Business requirements:
- Users create purchase applications with title, applicant department, expected date, item list, total amount, and remark.
- Draft applications can be updated, deleted, submitted, and revoked.
- Submitted applications can be approved or rejected by authorized approvers.
- Page query must support status, code, applicant, department, created time range, and data scope.
- Use Sa-Token permissions, MyBatis-Plus, transaction-safe submit/approve/reject/revoke actions, and DTO/VO/Entity separation.

Please read the existing package structure, response wrapper, exception model, mapper style, permission style, and tests before editing. Then implement the smallest complete module and run relevant verification.
```

### Review Existing Code

```text
Use $java-enterprise-backend to review PurchaseApplicationController, PurchaseApplicationServiceImpl, PurchaseApplicationMapper.xml, related DTO/VO/Entity classes, Redis usage, and RabbitMQ producer/consumer code.

Focus on bugs and production risks: permission bypass, data-scope gaps, transaction rollback issues, MyBatis-Plus misuse, SQL compatibility, Redis TTL/lock/idempotency problems, RabbitMQ duplicate delivery/retry/dead-letter handling, sensitive logs, and missing tests.

Return findings first by severity with file/line references, root cause, concrete fix, and verification suggestions. Do not rewrite unrelated code.
```

### Design APIs

```text
Use $java-enterprise-backend to design REST APIs for purchase application management.

Include page, detail, create, update, delete, submit, approve, reject, revoke, export, and current-user available actions. Define URL, method, permission code, request DTO/query fields, response VO fields, validation rules, error cases, data-scope behavior, idempotency requirements for submit/approve, and backward compatibility notes.
```

### Design Database Tables

```text
Use $java-enterprise-backend to design PostgreSQL/MySQL/domestic-database compatible tables for purchase applications, purchase items, approval records, and reliable domain events.

Include fields, types, audit columns, logical delete, optimistic lock, indexes, unique constraints, status enum values, migration SQL notes, rollback notes, and compatibility risks. Avoid vendor-specific syntax unless clearly marked.
```

### Review Transactions

```text
Use $java-enterprise-backend to review submit, approve, reject, revoke, import, and MQ-producing methods in PurchaseApplicationServiceImpl.

Check @Transactional placement, rollbackFor, self-invocation, swallowed exceptions, status transitions, optimistic lock, approval record consistency, Redis lock interaction, and RabbitMQ publish timing. Propose minimal patches and tests.
```

### Review Permissions

```text
Use $java-enterprise-backend to review the purchase module permission model.

Check @SaCheckPermission coverage, permission code naming, menu/button/API separation, data scope before pagination, whether userId/orgId/deptId/tenantId are trusted from frontend, export authorization, and approver-only actions. Provide concrete fixes and permission test cases.
```

### Review Redis

```text
Use $java-enterprise-backend to review Redis usage in captcha, idempotency token, purchase submit lock, approval rate limit, and dictionary cache code.

Check key naming, TTL, serialization compatibility, cache invalidation, sensitive data exposure, distributed lock owner value, unlock safety, retry behavior, and unbounded key growth. Return findings first and suggest minimal code changes.
```

### Review RabbitMQ

```text
Use $java-enterprise-backend to review RabbitMQ producers and consumers for purchase.application.submitted and purchase.application.approved events.

Check event naming, eventId/messageId, payload DTO design, producer reliability with database transaction commit, local message table or after-commit publishing, consumer idempotency, retry, dead-letter strategy, transaction length, logging, and tests.
```

### Review Xinchuang Compatibility

```text
Use $java-enterprise-backend to review the purchase module for xinchuang compatibility.

Check PostgreSQL/MySQL/domestic database portability, SQL functions, pagination, upsert, JSON/text/blob usage, identifier case, file paths, charset, time zone handling, native dependencies, OS assumptions, CPU architecture assumptions, Redis/RabbitMQ client compatibility, and deployment config. Mark risks and provide compatible alternatives.
```

## Example Files

- `skills/java-enterprise-backend/examples/module-generation-example.md`
- `skills/java-enterprise-backend/examples/code-review-example.md`
- `skills/java-enterprise-backend/examples/api-design-example.md`
- `skills/java-enterprise-backend/examples/database-design-example.md`
- `skills/java-enterprise-backend/examples/transaction-example.md`
- `skills/java-enterprise-backend/examples/permission-example.md`
- `skills/java-enterprise-backend/examples/redis-review-example.md`
- `skills/java-enterprise-backend/examples/rabbitmq-review-example.md`
- `skills/java-enterprise-backend/examples/xinchuang-compatibility-example.md`
- `skills/java-enterprise-backend/examples/mybatis-plus-example.md`

## Best Usage

1. Explicitly name the skill with `Use $java-enterprise-backend`.
2. Point Codex at concrete files, packages, PRs, or modules whenever possible.
3. State whether you want generation, review, design, or a minimal patch.
4. Ask Codex to read project conventions before editing.
5. Require findings first for review tasks.
6. Require verification commands and do not accept unverified "tests passed" claims.
7. For database, Redis, RabbitMQ, transaction, permission, or xinchuang tasks, include the matching example file as the prompt pattern.

## Output Expectations

For code generation, the agent should output:

- Files added or modified
- Key package paths and class names
- Main validation, permission, transaction, Redis, and RabbitMQ decisions
- Database migration notes if schema changes are required
- Compatibility assumptions and risks
- Tests and verification commands actually run

For review, the agent should output:

- Findings first, ordered by severity
- File and line references when available
- Root cause and impact
- Concrete fixes or patch suggestions
- Domestic database and production-readiness risks
- Tests or verification suggestions

## Repository Layout

```text
java-enterprise-backend/
+-- README.md
+-- LICENSE
+-- skills/
    +-- java-enterprise-backend/
        +-- SKILL.md
        +-- templates/
        +-- examples/
```

`LICENSE` may be managed by the repository owner and is intentionally not generated by this first version if it does not already exist.

## Limitations

- This skill defines engineering standards. It does not replace project-specific architecture documents, database migration policies, security baselines, or code owners.
- Domestic database compatibility must still be verified against the actual target database, driver, SQL dialect, and production schema.
- The skill assumes Spring Boot 3.x and Jakarta APIs. Legacy Spring Boot 2.x or `javax.*` projects need project-specific adaptation.
- No unified workflow engine is assumed. Approval workflows should be modeled explicitly in Service logic, status transitions, and audit records unless a project provides a workflow engine.

## License

See `LICENSE` if present in this repository.
