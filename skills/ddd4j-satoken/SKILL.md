---
name: ddd4j-satoken
description: |
  DDD4J Sa-Token 鉴权扩展技能。覆盖 StpKit 扩展载荷模式(登录时写user/org/role、获取时用StpUtil.getExtra)、SaTempKit一次性令牌、与ApiRestResponse异常处理集成、跨服务Same-Token配置、@SaCheckPermission在DDD4J Controller中的使用方式。
  当用户在 DDD4J 项目中使用 Sa-Token 做鉴权、需要登录扩展载荷(uuid/username/orgId)、生成一次性临时令牌时需要此技能。
  配合 ddd4j-core 和 sa-token 技能使用。
license: Apache-2.0
---

# DDD4J Sa-Token 鉴权扩展

> 编码 DDD4J 项目中 Sa-Token 的扩展用法。LLM 知道 Sa-Token 的标准 API，但不知道 DDD4J 的 StpKit 扩展模式。

## 为什么需要这个技能

LLM 会用 `StpUtil.getLoginId()` 获取登录用户——DDD4J 要求用 `StpUtil.getExtra("uuid")` 获取扩展载荷。LLM 不知道 DDD4J 登录时注入了 user/org/role 扩展参数。LLM 会用 `SaTempUtil.createToken()`——DDD4J 封装了 `SaTempKit`。

## Capability Boundaries

### ✅ Strong Suits
1. **StpKit 扩展载荷** — 登录时注入 user/org/role，获取时用 StpUtil.getExtra()
2. **SaTempKit** — DDD4J 封装的一次性临时令牌工具
3. **异常处理集成** — Sa-Token 异常与 ApiRestResponse 的统一处理
4. **多账号体系** — StpKit 门面模式区分用户和管理员

### ❌ Out of Scope
1. Sa-Token 基础 API（login/checkPermission/注解鉴权） → **sa-token**（通用技能）
2. 实体/Controller/Service 约定 → **ddd4j-core**

## LLM 最常犯的错误

| # | 错误 | 正确做法 |
|---|------|---------|
| 1 | `StpUtil.getLoginId()` 获取用户信息 | `StpUtil.getExtra("uuid")` / `getExtra("username")` |
| 2 | 登录只传 userId | 登录时注入扩展载荷(user/org/role) |
| 3 | `SaTempUtil.createToken()` | 用 `SaTempKit.createToken()`（DDD4J封装） |
| 4 | 直接抛 Sa-Token 异常 | 集成到 GlobalExceptionHandler 返回 ApiRestResponse |
| 5 | `StpUtil.login(userId)` 标准写法 | 用 StpKit 指定账号体系的 login |
| 6 | 跨服务鉴权用 Authorization header | 用 Same-Token + SaSameUtil |

## 核心规则速查

```java
// ✅ 登录：注入扩展载荷
StpUtil.login(userId, new SaLoginParameter()
    .setExtra("uuid", user.getUid())          // 用户唯一标识
    .setExtra("username", user.getUserName()) // 用户名
    .setExtra("orgId", user.getOrgId())       // 组织ID
    .setExtra("role", user.getRoleCodes())    // 角色列表
    .setExtra("realName", user.getRealName()) // 真实姓名
);

// ✅ 获取扩展载荷（而非 getLoginId）
String uuid = (String) StpUtil.getExtra("uuid");
String username = (String) StpUtil.getExtra("username");
String orgId = (String) StpUtil.getExtra("orgId");

// ✅ DDD4J 临时令牌
String token = SaTempKit.createToken(userId, 300);  // 5分钟
Long id = SaTempKit.parseToken(token, Long.class);

// ✅ 跨服务鉴权
// Gateway: 注入 Same-Token
SaSameUtil.getToken();
// 子服务: 校验
SaSameUtil.checkCurrentRequestToken();
// 定时刷新（每5分钟）
@Scheduled(cron = "0 0/5 * * * ?")
public void refreshToken() { SaSameUtil.refreshToken(); }
```

## Gotchas

1. **登录时必须注入扩展载荷** — 否则 `StpUtil.getExtra()` 返回 null
2. **扩展载荷只在 JWT 模式下生效** — 非 JWT 模式数据存在 Redis 中
3. **SaTempKit 是 DDD4J 封装，不是 Sa-Token 内置** — 依赖引入 `sa-token-temp-jwt`
4. **Same-Token 默认一天有效期** — 生产环境必须配置定时刷新
5. **@SaCheckPermission 需在 StpInterface 中注册权限码** — 光加注解不够
6. **Sa-Token 异常需统一转换为 ApiRestResponse** — 在 GlobalExceptionHandler 中处理

## Data Privacy
本技能不收集、存储或传输任何用户数据。
