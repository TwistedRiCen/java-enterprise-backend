# Business Module Generation Example

## Scenario

A Java 21 + Spring Boot 3.x enterprise system needs a new purchase application module. The existing project already has response wrappers, exception classes, mapper conventions, Sa-Token permissions, audit fields, and tests that must be followed.

## User Prompt

```text
Use $java-enterprise-backend to generate a purchase application business module in this Spring Boot project.

Business requirements:
- Users can create purchase applications with title, applicant department, expected date, item rows, total amount, and remark.
- Draft applications can be updated, logically deleted, submitted, and revoked.
- Submitted applications can be approved or rejected by users with approval permission.
- Page query supports status, code, applicant keyword, department, created time range, and data scope.
- Detail returns base fields, item rows, approval records, and current-user available actions.
- Submit, approve, reject, and revoke must be transaction-safe and idempotency-aware.
- Use Sa-Token backend permissions and data scope.
- Use MyBatis-Plus for simple CRUD and Mapper XML for data-scope page query.
- If domain events are needed, publish only after transaction commit or use the project reliable-message pattern.
- Add or update focused tests where the project test style is clear.

Before editing:
- Read the existing package structure, response wrapper, exception classes, MyBatis-Plus config, transaction style, permission style, Redis/RabbitMQ wrappers, migration scripts, and tests.
- Follow existing naming and layering.
- Do not introduce a workflow engine, code generator, or unrelated dependency.

Please implement the smallest complete module and run the most relevant verification command.
```

## Expected Codex Behavior

- Explore existing conventions before creating files.
- Generate Controller, Service, ServiceImpl, Mapper, Mapper XML, Entity, DTO, Query, VO, Convert, Enum, and tests only when they match project structure.
- Keep Controller thin and put transaction boundaries in Service.
- Use DTO/VO/Entity separation and avoid returning Entity to frontend.
- Add permission annotations and data-scope filtering.
- Use readable status enum values and optimistic lock where project conventions support it.
- Mention migration SQL or schema notes instead of silently assuming tables exist.

## Output Focus

- Files added or modified with package paths and class names.
- API endpoints, permission codes, DTO/VO/Query/Entity design, and status transitions.
- Transaction, idempotency, Redis, RabbitMQ, and audit-log decisions.
- Database migration notes and domestic database compatibility assumptions.
- Verification commands actually run and any test gaps.
