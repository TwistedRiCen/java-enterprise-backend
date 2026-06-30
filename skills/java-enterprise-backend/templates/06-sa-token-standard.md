# 06 Sa-Token Permission Standard

Backend permission checks are mandatory. Frontend button hiding is not security.

## Permission Code Style

```text
purchase:application:list
purchase:application:view
purchase:application:create
purchase:application:update
purchase:application:delete
purchase:application:submit
purchase:application:approve
purchase:application:reject
purchase:application:export
```

## Controller Rules

- Use `@SaCheckPermission` on protected methods unless the project has centralized permission interception.
- Keep permission codes consistent with menu and button configuration.
- Do not trust `userId`, `orgId`, `deptId`, or `tenantId` from frontend when they can be derived from login context.
- Use login context utilities already present in the project.

## Permission Categories

- Menu permission: user can access the module or page.
- Button permission: user can trigger an operation.
- Data permission: user can access "my data", department data, tenant data, or all data.

Separate these concerns. A button permission does not automatically grant all data access.

## Data Scope

APIs involving "my data", "department data", "organization data", "tenant data", or "all data" must implement data scope filtering.

Data scope filtering should be applied:

- Before pagination.
- In database queries where possible.
- Based on trusted login context, role, department, tenant, or organization relation.

## Review Questions

- Is each protected API covered by backend permission checks?
- Are action permissions distinct from list/view permissions?
- Is data scope applied before pagination?
- Are frontend-provided identity fields ignored or validated against login context?
- Are permission failures returned through the global exception handler?
