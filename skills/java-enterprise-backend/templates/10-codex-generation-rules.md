# 10 Codex Generation Rules

Use these rules when generating or modifying code.

## Before Generation

- Inspect existing project conventions before generating.
- Follow existing package names, base classes, response wrappers, exception types, permission style, mapper style, and test style.
- Ask only when key conventions are missing and guessing would cause incompatible code.
- Prefer modifying the minimal necessary files.
- Do not introduce dependencies without justification.
- Do not add a workflow engine because approval actions exist. Model status transitions explicitly unless the project already has workflow infrastructure.

## Java And Spring Rules

- Generate code that compiles under Java 21 and Spring Boot 3.x.
- Use `jakarta.*` where applicable.
- Use constructor injection or the project-standard injection style.
- Use `BigDecimal` for money.
- Use `LocalDateTime` for timestamps unless the project standard differs.
- Keep Controller thin.
- Put business orchestration and transactions in Service.
- Keep Entity, DTO, Query, VO, Convert, and Enum separated.

## Database Changes

- Provide migration notes when schema changes are needed.
- State database compatibility assumptions.
- Mark compatibility risks for JSON, arrays, recursive queries, generated columns, full-text search, and vendor-specific SQL.
- Do not assume MySQL-only syntax.

## Permission And Security

- Add backend permission checks for protected APIs.
- Use trusted login context for user, org, department, and tenant identity.
- Apply data scope before pagination.
- Avoid logging sensitive fields.

## Verification

- Provide test suggestions for Controller, Service, Mapper XML, permission, transaction, Redis, and RabbitMQ behavior.
- Run available relevant tests or builds when feasible.
- If tests cannot run, say "Tests not run" and explain why.

## Output Requirements

- State changed files and main decisions.
- Mention risks and compatibility notes.
- Avoid broad unrelated refactors.
- Preserve existing architecture.
