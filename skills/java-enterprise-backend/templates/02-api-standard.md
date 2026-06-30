# 02 API Standard

Use clear REST-style APIs with stable response wrappers and explicit request/response models.

## Unified Response

Follow the project wrapper if one exists. Otherwise use this shape:

```json
{
  "code": 0,
  "message": "success",
  "data": {},
  "traceId": "xxx"
}
```

Rules:

- `code` must be stable and documented.
- `message` should be clear for Chinese enterprise users when applicable.
- `traceId` should be included for production troubleshooting.
- Do not expose raw exception messages, SQL errors, stack traces, or internal class names.

## Page Response

```json
{
  "total": 100,
  "pageNum": 1,
  "pageSize": 10,
  "records": []
}
```

Rules:

- Page query DTOs should use explicit `pageNum` and `pageSize`.
- Sort fields must be whitelisted if user-controlled.
- Page queries involving permissions must apply data scope before pagination.
- Use stable ordering to avoid duplicate or missing records across pages.

## URL Naming

Recommended:

```text
GET    /api/purchase/applications/page
GET    /api/purchase/applications/{id}
POST   /api/purchase/applications
PUT    /api/purchase/applications/{id}
DELETE /api/purchase/applications/{id}
POST   /api/purchase/applications/{id}/submit
POST   /api/purchase/applications/{id}/approve
POST   /api/purchase/applications/{id}/reject
POST   /api/purchase/applications/{id}/revoke
```

Avoid unless preserving legacy compatibility:

```text
/add
/update
/delete
/getList
/doApprove
```

## Controller Requirements

- Use validation annotations on DTOs and Query objects.
- Use `@SaCheckPermission` or the project permission mechanism.
- Keep Controller thin: bind, validate, authorize, call Service, return wrapper.
- Do not assemble SQL conditions or business state transitions in Controller.
- Do not accept `userId`, `orgId`, or `tenantId` from frontend when available from login context.

## DTO And VO Rules

- Create and update requests should use separate DTOs when validation differs.
- Action DTOs should model action-specific fields, such as approval comment or reject reason.
- VO should expose only fields needed by clients.
- Avoid using `Map<String, Object>` for stable contracts.
- Use `BigDecimal` for money, `LocalDateTime` for timestamps, and enum strings for status when feasible.

## API Review Questions

- Are REST names resource-oriented and action endpoints clear?
- Are validation rules complete?
- Are permission and data scope checks present?
- Are request and response models separated from Entity?
- Are error codes and messages stable?
