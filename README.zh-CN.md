# java-enterprise-backend 中文使用指南

面向 Java 21 + Spring Boot 3.x 企业级后端项目的 AI Skill，适用于政企、国企、信创兼容、审批流、权限、事务、数据库、Redis、RabbitMQ 等常见后端研发场景。

## 适用技术栈

- JDK 21
- Spring Boot 3.x
- MyBatis-Plus
- PostgreSQL 18
- MySQL 8.0
- 国产数据库及兼容性 SQL
- Redis
- RabbitMQ
- Sa-Token
- Maven
- Linux / 国产服务器操作系统

## 安装命令

```bash
npx skills@latest add https://github.com/TwistedRiCen/java-enterprise-backend --skill java-enterprise-backend
```

## 卸载命令

卸载项目级安装的技能：

```bash
npx skills@latest remove java-enterprise-backend
```

如果之前使用全局方式安装，请卸载全局技能：

```bash
npx skills@latest remove java-enterprise-backend --global
```

也可以使用交互式卸载：

```bash
npx skills@latest remove
```

卸载前后可以查看已安装技能：

```bash
npx skills@latest list
npx skills@latest list --global
```

## 最佳使用方式

使用时建议显式写出 `Use $java-enterprise-backend`，并说明你希望 Codex 做“生成、审查、设计、修复”中的哪一种。

推荐基础 Prompt：

```text
Use $java-enterprise-backend to review the current Spring Boot module for transaction, permission, Redis, RabbitMQ, database, and xinchuang compatibility risks. Read related code first, make minimal changes only if needed, and run the most relevant verification command.
```

中文也可以直接这样写：

```text
Use $java-enterprise-backend 审查当前 Spring Boot 模块的事务、权限、Redis、RabbitMQ、数据库和信创兼容性风险。请先阅读相关代码和项目约定，只在必要时做最小修改，并运行最相关的验证命令。
```

## 中文可复制示例

### 1. 生成业务模块

```text
Use $java-enterprise-backend 为当前 Spring Boot 项目生成采购申请业务模块。

业务要求：
- 用户可以创建采购申请，字段包括标题、申请部门、期望日期、明细行、总金额和备注。
- 草稿状态可以修改、逻辑删除、提交和撤回。
- 已提交状态可以由有权限的审批人审批通过或驳回。
- 分页查询支持状态、单号、申请人、部门、创建时间范围和数据权限。
- 详情接口返回基础信息、明细行、审批记录和当前用户可执行操作。
- 提交、审批、驳回、撤回必须保证事务安全，并考虑幂等。
- 使用 Sa-Token 做后端权限控制和数据权限过滤。
- 简单 CRUD 使用 MyBatis-Plus，带数据权限的复杂分页查询使用 Mapper XML。

请先阅读现有包结构、统一响应、异常模型、Mapper 风格、权限风格、事务风格、Redis/RabbitMQ 封装、迁移脚本和测试，再按项目风格实现最小完整模块，并运行相关验证命令。
```

### 2. 代码审查

```text
Use $java-enterprise-backend 审查采购申请模块。

审查对象：
- PurchaseApplicationController
- PurchaseApplicationServiceImpl
- PurchaseApplicationMapper.xml
- Entity、DTO、Query、VO、Convert、Enum
- Redis 幂等和分布式锁代码
- RabbitMQ 生产者和消费者

重点检查：
- Controller 是否包含业务逻辑、是否缺少校验、是否返回 Entity。
- Sa-Token 权限是否完整，数据权限是否在分页前生效。
- 事务边界、rollbackFor、自调用、吞异常、MQ 发送时机是否正确。
- MyBatis-Plus 和 XML SQL 是否存在 SQL 注入、分页不稳定、数据库兼容性问题。
- Redis key、TTL、锁 owner、解锁安全、幂等行为是否可靠。
- RabbitMQ eventId、消息体、生产可靠性、消费幂等、重试和死信是否完整。

请按严重级别先输出 Findings，包含文件/行号、根因、影响、最小修复建议和验证建议。不要重构无关代码。
```

### 3. API 设计

```text
Use $java-enterprise-backend 设计采购申请管理 REST API。

请覆盖：
- 分页、详情、创建、修改、删除、提交、审批、驳回、撤回、导出、当前用户可执行操作。
- 每个接口的 URL、HTTP 方法、权限码、请求 DTO/Query、响应 VO、校验规则、错误码。
- 分页和导出必须说明数据权限行为。
- 提交、审批、驳回、撤回必须说明幂等要求和事务要求。
- 如果项目已有 /add、/update、/delete、/getList 等旧接口，请说明兼容方案。
```

### 4. 数据库设计

```text
Use $java-enterprise-backend 设计采购申请模块数据库表，要求兼容 PostgreSQL、MySQL 和国产数据库。

需要设计：
- purchase_application
- purchase_application_item
- purchase_approval_record
- domain_event_message 或项目已有可靠消息表

请输出字段、类型、默认值、是否为空、注释、主键、唯一约束、索引、逻辑删除、乐观锁、审计字段、状态枚举、迁移 SQL 建议、回滚说明和兼容性风险。

不要默认使用某个数据库的专有语法；如果必须使用，请明确标记风险。
```

### 5. 事务审查

```text
Use $java-enterprise-backend 审查 PurchaseApplicationServiceImpl 中 create、update、delete、submit、approve、reject、revoke、importExcel 和消息发布相关方法的事务安全。

重点检查：
- @Transactional 是否放在 public Service 方法上。
- 是否设置 rollbackFor = Exception.class。
- 是否存在 self-invocation 导致事务失效。
- catch 块是否吞异常导致不回滚。
- 申请主表、明细表、审批记录、可靠消息表是否原子一致。
- RabbitMQ 是否在事务提交前发送。
- Redis 锁和幂等 token 在事务失败时是否会留下错误状态。

请先输出问题和根因，再给最小修复方案和测试建议。
```

### 6. 权限审查

```text
Use $java-enterprise-backend 审查采购申请模块权限模型。

检查范围：
- page、detail、create、update、delete、submit、approve、reject、revoke、export 接口。
- @SaCheckPermission 权限码是否完整。
- 菜单权限、按钮权限、接口权限、数据权限是否区分。
- 数据权限是否在分页和导出前生效。
- userId、deptId、orgId、tenantId 是否错误信任前端传参。
- 审批、驳回、撤回是否校验当前登录用户和业务状态。

请输出可直接落地的修复建议和权限测试用例。
```

### 7. Redis 审查

```text
Use $java-enterprise-backend 审查采购模块 Redis 使用。

审查对象：
- 验证码 key
- 提交/审批幂等 token
- 采购提交分布式锁
- 审批限流
- 字典或采购分类缓存

重点检查 key 命名、业务前缀、TTL、序列化兼容、缓存失效、敏感数据、锁 owner value、解锁安全、失败重试、无界 key 增长和重复提交行为。

请按严重级别输出问题，并给最小修复建议。
```

### 8. RabbitMQ 审查

```text
Use $java-enterprise-backend 审查采购模块 RabbitMQ 使用。

审查对象：
- purchase.application.submitted 事件生产者和消费者。
- purchase.application.approved 事件生产者和消费者。
- DomainEvent 消息包装类和 payload DTO。
- 本地消息表或事务提交后发布机制。
- 重试、死信、监听器配置和测试。

重点检查 eventId/messageId、消息体是否使用 Entity、是否泄露敏感字段、是否事务提交后发送、消费者是否幂等、重复投递如何处理、失败重试和死信策略是否清晰。
```

### 9. 信创兼容性审查

```text
Use $java-enterprise-backend 审查采购模块信创兼容性。

检查范围：
- Mapper XML、注解 SQL、迁移脚本、报表 SQL。
- Entity 字段类型和数据库字段类型。
- Redis/RabbitMQ 客户端和配置。
- 文件上传、导入、导出、临时文件路径。
- 日期时间、字符集、时区、路径分隔符。
- Maven 依赖和 native library。
- Docker、Linux service 或部署配置。

重点关注 PostgreSQL/MySQL/国产数据库兼容性、date_format/ifnull/group_concat/limit/upsert/JSON 操作符等专有语法、Windows 路径、x86 假设、国产 OS/CPU 兼容风险。

请输出风险等级、证据、影响、兼容替代方案和验证方法。
```

## 示例文件位置

- `skills/java-enterprise-backend/examples/module-generation-example.md`
- `skills/java-enterprise-backend/examples/code-review-example.md`
- `skills/java-enterprise-backend/examples/api-design-example.md`
- `skills/java-enterprise-backend/examples/database-design-example.md`
- `skills/java-enterprise-backend/examples/transaction-example.md`
- `skills/java-enterprise-backend/examples/permission-example.md`
- `skills/java-enterprise-backend/examples/redis-review-example.md`
- `skills/java-enterprise-backend/examples/rabbitmq-review-example.md`
- `skills/java-enterprise-backend/examples/xinchuang-compatibility-example.md`
- `skills/java-enterprise-backend/examples/mybatis-plus-example.md`

## 输出要求建议

审查类任务建议要求 Codex：

- 先输出 Findings，再输出总结。
- 按严重级别排序。
- 尽量提供文件和行号。
- 每个问题包含根因、影响、最小修复方案和验证建议。
- 不要声称测试通过，除非确实运行过。

生成类任务建议要求 Codex：

- 先读项目结构和已有约定。
- 只做最小完整实现。
- 说明新增或修改文件。
- 说明权限、事务、数据库、Redis、RabbitMQ 和信创兼容性决策。
- 运行相关测试、构建或最小验证命令。
