# Xinchuang Compatibility Review Example

## Scenario

An enterprise customer plans to deploy the Spring Boot backend on domestic operating systems, domestic CPU architectures, and a domestic database compatible with PostgreSQL or MySQL. The user wants to find portability risks before delivery.

## User Prompt

```text
Use $java-enterprise-backend to review the purchase module for xinchuang compatibility.

Review targets:
- SQL in Mapper XML, annotations, migrations, and reports.
- Entity field types and database column types.
- Redis and RabbitMQ client usage and configuration.
- File upload/export/import code.
- Date/time, charset, path, and temporary-file handling.
- Maven dependencies and any native libraries.
- Docker, Linux service, or deployment config if present.

Check:
- PostgreSQL/MySQL/domestic database portability.
- Vendor-specific SQL functions such as date_format, ifnull, group_concat, limit-only pagination, upsert syntax, JSON operators, generated columns, recursive queries, full-text search, text/clob handling, and identifier case sensitivity.
- SQL Server/MySQL/PostgreSQL syntax assumptions if the actual target database is unclear.
- File paths using Windows separators or hard-coded local directories.
- Charset and time zone assumptions.
- Native dependencies that may not support ARM, LoongArch, Phytium, Kylin, UOS, or other domestic environments.
- Redis/RabbitMQ config assumptions that may break in restricted networks or clustered deployments.
- Logging of sensitive production data.

Return a compatibility risk report with severity, evidence, impact, compatible alternative, and verification method. Make minimal patches only if the compatibility issue is clear from the code.
```

## Expected Codex Behavior

- Inspect actual database config, migrations, mapper XML, dependencies, and deployment files before making claims.
- Avoid assuming a specific domestic database when the project does not state one.
- Mark uncertain compatibility issues as risks with verification steps.
- Prefer standard SQL and application-layer formatting when practical.
- Avoid native or OS-specific assumptions unless the project already requires them.

## Output Focus

- Database compatibility risks and SQL alternatives.
- OS, path, charset, timezone, and CPU architecture risks.
- Dependency and native library risks.
- Redis/RabbitMQ deployment compatibility notes.
- Verification matrix for target database, OS, CPU architecture, middleware, and JDK.
