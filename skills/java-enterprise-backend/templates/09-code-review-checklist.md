# 09 Code Review Checklist

Use findings-first output. List concrete issues before summaries.

## Checklist

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

## Severity Guidance

- P0: Security bypass, data corruption, financial/accounting error, irreversible production breakage.
- P1: Transaction inconsistency, permission or data-scope leak, SQL injection risk, message loss, severe compatibility issue.
- P2: Incorrect API contract, weak validation, missing idempotency, pagination bugs, maintainability issue with clear impact.
- P3: Naming, style, missing minor logs, optional test improvement.

## Review Output Shape

```text
Findings
- [P1] file:line - Issue title.
  Why it matters. Concrete fix.

Open Questions
- ...

Summary
- ...

Verification
- Tests run: ...
- Not run: ...
```

## Review Discipline

- Do not praise before listing findings.
- Do not invent line numbers.
- Do not claim tests passed unless commands were run.
- If no findings are found, state that clearly and mention residual risks or test gaps.
- Prefer patchable suggestions over generic advice.
