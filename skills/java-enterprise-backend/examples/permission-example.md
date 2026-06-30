# Permission Review Example

## Scenario

The purchase module uses Sa-Token for API permissions and a custom data-scope service for department-level filtering. The user wants to prevent direct-call authorization bypass and data leakage.

## User Prompt

```text
Use $java-enterprise-backend to review the purchase module permission model.

Review targets:
- PurchaseApplicationController endpoints for page, detail, create, update, delete, submit, approve, reject, revoke, export, and current-user actions.
- PurchaseApplicationServiceImpl data-scope and approver checks.
- Mapper XML page query and export query.
- Any DTO/query fields containing userId, deptId, orgId, tenantId, applicantId, or approverId.

Check:
- @SaCheckPermission coverage and permission code naming.
- Separation of menu, button, API, and data permissions.
- Data scope applied before pagination and export.
- Detail, update, delete, submit, approve, reject, revoke, and export authorization.
- Whether frontend-provided userId/deptId/orgId/tenantId can expand access.
- Whether approver-only actions validate current login user and business status.
- Whether permission-sensitive failures are logged without leaking sensitive data.

Return findings first with concrete fixes and permission test cases.
```

## Expected Codex Behavior

- Inspect project permission conventions before adding annotations or service checks.
- Use backend login context for trusted identity.
- Apply data scope in database queries before pagination.
- Distinguish view/list/create/update/delete/submit/approve/reject/export permissions.
- Avoid widening access through optional query filters.

## Output Focus

- Missing or incorrect permission annotations.
- Data-scope gaps and frontend-trust risks.
- Action-specific authorization and status-transition risks.
- Suggested permission codes and where to enforce them.
- Test cases for direct API calls, cross-department access, export scope, and unauthorized approval.
