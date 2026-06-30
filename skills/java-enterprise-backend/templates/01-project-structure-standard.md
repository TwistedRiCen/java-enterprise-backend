# 01 Project Structure Standard

Use a business-module-first structure for enterprise information systems.

```text
com.company.project
+-- common
|   +-- core
|   +-- exception
|   +-- response
|   +-- validation
|   +-- security
|   +-- utils
+-- config
+-- module
|   +-- purchase
|   |   +-- controller
|   |   +-- service
|   |   +-- service/impl
|   |   +-- mapper
|   |   +-- entity
|   |   +-- dto
|   |   +-- vo
|   |   +-- query
|   |   +-- convert
|   |   +-- enums
|   +-- system
+-- Application.java
```

## Layer Responsibilities

| Layer | Responsibility |
| --- | --- |
| Controller | Request binding, validation annotations, permission annotations, response wrapping, Service calls only. |
| Service | Business orchestration, validation, transaction boundary, state transitions, integration with Redis/RabbitMQ wrappers. |
| Mapper | Database access only. Mapper XML for complex SQL. |
| Entity | Database table mapping only. No frontend response responsibility. |
| DTO | Create, update, and action request objects. |
| Query | List and page query conditions. |
| VO | Response objects returned to frontend. |
| Convert | Object conversion between DTO, Entity, VO, and event payloads. |
| Enum | Business status and type constants. |

## Required Separation

- Controller must not call Mapper.
- Controller must not contain business logic or status transition rules.
- Service must not return Entity directly to frontend.
- Service must not return raw `Map` unless explicitly justified.
- Entity must not include frontend-only fields.
- Utility classes must not become hidden business services.
- Cross-module calls should go through Service interfaces or explicit integration services.

## Naming

- Use module names that match business domains: `purchase`, `contract`, `asset`, `finance`, `system`.
- Use suffixes consistently: `CreateDTO`, `UpdateDTO`, `ActionDTO`, `Query`, `VO`, `Entity`, `Mapper`, `Service`, `ServiceImpl`, `Convert`, `StatusEnum`.
- Prefer readable business names over generic names such as `CommonService`, `DataService`, or `HandleUtil`.

## Review Questions

- Is the package structure aligned with existing project conventions?
- Are DTO, Query, VO, and Entity separated?
- Is business logic only in Service or domain-specific components?
- Are conversions centralized and testable?
- Are cross-module dependencies explicit?
