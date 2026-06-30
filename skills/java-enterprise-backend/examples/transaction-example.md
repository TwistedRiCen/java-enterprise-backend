# Transaction Review Example

## Scenario

Purchase application submit, approve, reject, revoke, import, and MQ-producing methods update multiple tables and publish domain events. The user wants to catch rollback, consistency, and concurrency bugs.

## User Prompt

```text
Use $java-enterprise-backend to review transaction safety in PurchaseApplicationServiceImpl.

Review targets:
- create, update, delete, submit, approve, reject, revoke, importExcel, and publishApprovalEvent methods.
- Approval record creation and status transition logic.
- Redis lock/idempotency interaction around submit and approve.
- RabbitMQ or domain event publishing after status changes.

Check:
- @Transactional placement on public Service methods and rollbackFor = Exception.class.
- Self-invocation or private/internal method transaction failure.
- Swallowed exceptions, catch blocks that prevent rollback, and failure-log persistence requirements.
- Status transition validation and optimistic lock/version handling.
- Atomicity between purchase application, item rows, approval records, and local message table.
- MQ publish before database commit or missing after-commit/local-message reliability strategy.
- Redis lock timeout, owner value, unlock safety, and behavior when transaction fails.
- Tests for rollback, duplicate submit/approve, and message consistency.

Return findings first. If code changes are needed, make minimal patches only.
```

## Expected Codex Behavior

- Trace the full call chain instead of reviewing annotations in isolation.
- Identify whether the transaction proxy can actually intercept the method.
- Check catch blocks for rollback and log persistence semantics.
- Prefer after-commit publishing, local message table, or project-provided reliable event infrastructure.
- Keep Redis critical sections small and safe.

## Output Focus

- Transaction boundary defects and self-invocation risks.
- Rollback failure caused by swallowed exceptions or checked exceptions.
- Multi-table consistency and status transition risks.
- MQ publish timing and local message strategy recommendations.
- Minimal patches and tests for rollback, optimistic lock conflict, idempotency, and duplicate MQ delivery.
