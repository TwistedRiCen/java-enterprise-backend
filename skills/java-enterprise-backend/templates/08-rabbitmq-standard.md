# 08 RabbitMQ Standard

Use RabbitMQ for asynchronous domain events and integration messages with explicit reliability and idempotency.

## Event Naming

```text
purchase.application.submitted
purchase.application.approved
purchase.application.rejected
```

Use lowercase dot-separated event names: `{module}.{aggregate}.{event}`.

## Message Wrapper

```java
public class DomainEvent<T> {
    private String eventId;
    private String eventType;
    private LocalDateTime occurredAt;
    private T payload;
}
```

Rules:

- Every message must have `messageId` or `eventId`.
- Payload must be a DTO/event object, not an Entity.
- Include only fields the consumer needs.
- Avoid sensitive data in payloads.

## Producer Reliability

When events are triggered by database changes, choose a reliability strategy:

- Publish after transaction commit.
- Use a local message table and async dispatcher.
- Use project-provided reliable event infrastructure.

Do not publish directly inside an active transaction without explaining the failure mode.

## Consumer Rules

- Consumers must be idempotent.
- Store processed `eventId` when duplicate delivery is possible.
- Keep consumer transactions short.
- Do not perform long unpredictable operations inside a database transaction.
- Define retry and dead-letter behavior.
- Log `eventId`, `eventType`, business ID, result, and failure reason.

## Review Questions

- Does the event have a stable name and ID?
- Is the payload decoupled from Entity?
- Is producer delivery reliable relative to DB commit?
- Is the consumer idempotent?
- Are retry and dead-letter strategies configured or documented?
