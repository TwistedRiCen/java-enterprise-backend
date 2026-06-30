# 05 Transaction Standard

Transactions protect business state. Define boundaries in public Service methods.

## Required Transactional Operations

Use `@Transactional(rollbackFor = Exception.class)` for:

- Create/update/delete involving multiple writes
- Submit, approve, reject, revoke
- Import and batch update
- Payment-like operations
- Inventory, balance, quota, or counter updates
- Database writes that must stay consistent with approval records, logs, or status transitions

Read-only queries usually do not need transactions unless the project has a clear consistency requirement.

## Rules

- Put transaction annotations on public Service methods called through Spring proxies.
- Avoid self-invocation transaction failure.
- Do not swallow exceptions inside transactional methods.
- Do not catch and convert exceptions in a way that prevents rollback.
- If failure logs must persist after rollback, write them in `REQUIRES_NEW` or outside the main transaction.
- Keep transaction scope as small as practical.
- Do not perform long remote calls inside a transaction unless the business explicitly requires it and timeout handling is clear.

## RabbitMQ And Transactions

Do not send RabbitMQ messages directly inside a database transaction without a reliability strategy.

Preferred strategies:

- Publish after transaction commit.
- Write a local message table in the same transaction and dispatch asynchronously.
- Use a reliable event mechanism already provided by the project.

## Example Pattern

```java
@Transactional(rollbackFor = Exception.class)
public void submit(Long id) {
    PurchaseApplicationEntity entity = getRequiredForUpdate(id);
    validateCanSubmit(entity);
    entity.setStatus(PurchaseStatus.SUBMITTED.name());
    updateById(entity);
    approvalRecordService.createSubmitRecord(entity);
    domainEventPublisher.publishAfterCommit(PurchaseApplicationSubmittedEvent.from(entity));
}
```

## Review Questions

- Is the transaction boundary at the correct Service method?
- Can rollback be skipped because exceptions are swallowed?
- Is there self-invocation?
- Are status transition and approval record persisted atomically?
- Is message delivery reliable after database changes?
