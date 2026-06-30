# Code Review Example

## Scenario

A purchase application module already exists. The user wants a production-focused review before merging a PR that touches Controller, Service, Mapper XML, Redis lock/idempotency code, and RabbitMQ event publishing.

## User Prompt

```text
Use $java-enterprise-backend to review the purchase application module before merge.

Review targets:
- PurchaseApplicationController
- PurchaseApplicationServiceImpl
- PurchaseApplicationMapper and PurchaseApplicationMapper.xml
- PurchaseApplicationEntity, DTO, Query, VO, Convert, and status enum
- Redis code used for submit idempotency and distributed locks
- RabbitMQ producer and consumers for purchase.application.submitted and purchase.application.approved

Focus on real bugs and production risks:
- Controller business logic, missing validation, Entity exposure, and response wrapper consistency.
- Sa-Token permission gaps, data scope before pagination, and unsafe frontend-provided userId/deptId/tenantId.
- @Transactional placement, rollbackFor, self-invocation, swallowed exceptions, and MQ publish timing.
- MyBatis-Plus misuse, unreadable wrappers, SQL injection, unstable pagination, and domestic database compatibility.
- Redis key naming, TTL, lock owner value, unlock safety, idempotency behavior, and cache invalidation.
- RabbitMQ eventId/messageId, payload DTO, producer reliability, consumer idempotency, retry, and dead-letter behavior.
- Sensitive logs and missing tests.

Return findings first by severity with file/line references when available. For each finding include root cause, impact, minimal fix, and verification suggestion. Do not rewrite unrelated code.
```

## Expected Codex Behavior

- Read related files and call chains before judging.
- Prioritize exploitable or production-breaking issues over style comments.
- Use severity such as P0/P1/P2/P3 and place findings before summary.
- Provide concrete patches only for in-scope issues and preserve existing project conventions.
- Mention tests or verification commands; never claim tests passed unless actually run.

## Output Focus

- Findings ordered by severity with file and line references.
- Root cause, impact, and minimal fix for each finding.
- Notes on permission, transaction, Redis, RabbitMQ, SQL compatibility, and sensitive logging risks.
- Suggested tests for controller authorization, service transactions, mapper SQL, Redis idempotency, and MQ duplicate delivery.
- Short summary only after findings.
