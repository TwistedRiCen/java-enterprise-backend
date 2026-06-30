# API Design Example

## Scenario

An enterprise Spring Boot 3.x system needs REST APIs for purchase application management. The frontend needs stable contracts for page query, detail, draft editing, approval actions, export, and current-user available actions.

## User Prompt

```text
Use $java-enterprise-backend to design REST APIs for purchase application management in a Java 21 + Spring Boot 3.x enterprise project.

Business context:
- Purchase applications have draft, submitted, approved, rejected, and revoked statuses.
- Draft applications can be created, updated, deleted, submitted, and revoked.
- Submitted applications can be approved or rejected by authorized approvers.
- Page query must support status, code, applicant, department, created time range, and data scope.
- Detail must include item rows, approval records, and current-user available actions.
- Export must obey the same permission and data-scope rules as page query.

Please output:
- URL, method, purpose, permission code, request DTO/query, response VO, validation, and error cases.
- Unified response and page response assumptions.
- Idempotency requirements for submit/approve/reject/revoke.
- Data-scope and audit-log behavior.
- Backward compatibility risks if existing legacy endpoints already use /add, /update, /delete, or /getList.
```

## Expected Codex Behavior

- Inspect existing controller naming, response wrapper, page wrapper, exception codes, Sa-Token style, and DTO/VO conventions before finalizing names.
- Prefer resource/action URLs such as `/api/purchase/applications/{id}/approve`.
- Separate create/update/action DTOs, query objects, page VO, detail VO, and export VO.
- Derive current user, department, organization, and tenant from login context instead of trusting frontend fields.
- Apply data scope before pagination and export.
- State compatibility notes if legacy URLs must be preserved.

## Output Focus

- Endpoint table with method, URL, permission code, request, response, and notes.
- DTO/query/VO field lists with validation constraints.
- Status transition and error-code list.
- Permission, data-scope, idempotency, transaction, and audit-log requirements.
- Suggested controller/service tests for permission, validation, page scope, and action idempotency.
