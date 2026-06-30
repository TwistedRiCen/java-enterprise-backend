# RabbitMQ Review Example

## Scenario

The purchase module publishes RabbitMQ events when applications are submitted and approved. Consumers update downstream statistics and notify approvers. The user wants to verify reliability, idempotency, and transaction consistency.

## User Prompt

```text
Use $java-enterprise-backend to review RabbitMQ usage for the purchase application module.

Review targets:
- Producer code for purchase.application.submitted and purchase.application.approved.
- Domain event wrapper and payload DTOs.
- Local message table or after-commit publish mechanism, if present.
- Consumers that handle submitted/approved events.
- RabbitMQ retry, dead-letter, and listener configuration.
- Tests or integration tests around producer and consumer behavior.

Check:
- Event naming and stable eventType.
- eventId/messageId generation and propagation.
- Payload DTO design and whether Entity objects or sensitive fields are sent.
- Whether messages are published only after database commit or via local message table.
- What happens if DB commit succeeds but MQ publish fails.
- Consumer idempotency and duplicate event handling.
- Retry strategy, dead-letter queue, poison-message handling, and alert/logging.
- Consumer transaction length and external calls inside transactions.
- Logs containing eventId, eventType, businessId, result, and failure reason.

Return findings first. For fixes, prefer project-standard after-commit publisher or local message table instead of introducing unrelated infrastructure.
```

## Expected Codex Behavior

- Trace producer calls from service transaction methods.
- Verify event payloads are decoupled from persistence Entity classes.
- Check consumer duplicate delivery behavior and processed-event storage.
- Preserve existing MQ abstraction and configuration style.
- Recommend after-commit publishing, local message table, or existing reliable event infrastructure when database state drives events.

## Output Focus

- Producer reliability risks relative to transaction commit.
- Event wrapper and payload issues.
- Consumer idempotency, retry, and dead-letter findings.
- Logging and observability gaps.
- Minimal fixes and tests for duplicate delivery, publish failure, and consumer failure.
