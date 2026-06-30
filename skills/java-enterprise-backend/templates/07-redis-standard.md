# 07 Redis Standard

Use Redis only where it improves correctness or performance and the invalidation strategy is clear.

## Allowed Use Cases

- Sa-Token/session
- Captcha
- Short-term cache
- Distributed lock
- Idempotency token
- Hot dictionaries
- Rate limiting

## Key Naming

Keys must have a project or business prefix:

```text
project:captcha:{uuid}
project:dict:{dictType}
project:idempotent:{token}
project:lock:purchase-submit:{id}
```

Rules:

- Temporary keys must have TTL.
- Key format must be stable and documented.
- Avoid unbounded key growth.
- Avoid storing sensitive data unless necessary and protected.

## Cache Rules

- Define cache source of truth.
- Define invalidation on create, update, delete, import, and dictionary refresh.
- Consider serialization compatibility when object classes evolve.
- Avoid caching permission-sensitive data unless the permission context is part of the key or result is safely filtered.

## Distributed Lock Rules

- Lock value must be unique per owner.
- Lock must have timeout.
- Unlock must verify the owner value.
- Critical section must be small.
- Lock failure behavior must be explicit: retry, fail fast, or return idempotent result.

## Idempotency Rules

- Idempotency keys must have TTL.
- Repeated submit/import/payment-like requests must not create duplicate business records.
- Store enough result state to answer repeated requests safely when required.

## Review Questions

- Does every temporary key have TTL?
- Is the key prefix clear and collision-resistant?
- Is cache invalidation described?
- Are locks safe against accidental unlock by another owner?
- Is sensitive data protected?
