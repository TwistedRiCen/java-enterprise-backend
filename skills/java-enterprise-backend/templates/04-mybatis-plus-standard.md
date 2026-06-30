# 04 MyBatis-Plus Standard

Use MyBatis-Plus for simple CRUD and typed wrappers. Use Mapper XML for complex business SQL.

## Entity Mapping

```java
@TableName("purchase_application")
public class PurchaseApplicationEntity {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;

    @TableLogic
    private Integer deleted;

    @Version
    private Integer version;
}
```

Rules:

- Entity represents table fields only.
- Use `@TableId(type = IdType.ASSIGN_ID)` unless the project has another ID strategy.
- Use `@TableLogic` for logical delete fields.
- Use `@Version` for optimistic locking where concurrent updates matter.
- Do not put frontend-only or computed presentation fields in Entity.

## Query Rules

- Simple CRUD may use MyBatis-Plus service methods.
- Use `LambdaQueryWrapper` for simple typed conditions.
- Keep wrappers readable. If a wrapper becomes long or nested, move the query to Mapper XML.
- Reports, joins, permission-filtered queries, aggregation queries, and complex ordering should use Mapper XML.
- Do not concatenate SQL strings in Service.
- Use `@Param` names consistently in Mapper methods.

## Pagination

- Pagination must be explicit.
- Apply data scope filters before pagination.
- Always use stable ordering for page queries.
- Avoid returning unbounded lists from page endpoints.

## XML SQL Rules

- Keep SQL readable and formatted.
- Use result maps when joins return fields not matching a single Entity.
- Prefer VO or dedicated projection objects for list/detail queries.
- Avoid vendor-specific functions unless compatibility is documented.

## Review Questions

- Is MyBatis-Plus used for simple cases only?
- Are complex joins or reports moved to XML?
- Is SQL injection prevented?
- Are logical delete and optimistic lock configured?
- Is pagination stable and scoped?
