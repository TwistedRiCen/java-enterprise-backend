# Redis Review Example

## Scenario

The purchase module uses Redis for captcha, submit idempotency, distributed locks, approval rate limits, and dictionary cache. The user wants to find correctness and production risks.

## User Prompt

```text
Use $java-enterprise-backend to review Redis usage in the purchase module.

Review targets:
- Captcha create/verify code.
- Submit and approve idempotency token code.
- Distributed lock around purchase submit/approve.
- Approval rate-limit code.
- Dictionary or purchase category cache.
- Any Redis key constants and serialization configuration.

Check:
- Key prefix, naming stability, collision risk, and tenant/project isolation.
- TTL on temporary keys and unbounded key growth.
- Captcha single-use behavior and expiration.
- Idempotency token TTL, duplicate request result behavior, and failure rollback behavior.
- Distributed lock owner value, timeout, unlock owner verification, and critical section size.
- Cache source of truth and invalidation on create/update/delete/import/dictionary refresh.
- Serialization compatibility when DTO classes evolve.
- Sensitive data stored in Redis or logs.
- Tests for duplicate submit, lock contention, expired token, and cache invalidation.

Return findings first by severity. Propose minimal patches only for confirmed issues.
```

## Expected Codex Behavior

- Inspect existing Redis client/template/wrapper conventions and key constants.
- Avoid replacing the project's Redis abstraction without reason.
- Check both happy path and failure path for idempotency and locks.
- Prefer explicit TTLs and documented invalidation.
- Treat permission-sensitive cache entries as risky unless scope is part of the key or result.

## Output Focus

- Key naming and TTL findings.
- Distributed lock safety findings.
- Idempotency and duplicate-submit behavior.
- Cache invalidation and serialization compatibility issues.
- Sensitive-data and logging risks.
- Minimal patch suggestions and Redis-focused tests.
