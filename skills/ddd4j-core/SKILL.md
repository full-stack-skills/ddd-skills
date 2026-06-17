---
name: ddd4j-core
description: |
  DDD4J Boot 核心开发约定技能。覆盖实体继承链(BaseEntity→Model<T>/PaginationEntity)、DDD注解规则(@DomainEntity是SOURCE/@DomainService≡@Service)、Controller层级(BaseController→BaseMapperController)、Service层级(IBaseService→BaseServiceImpl)、返回值约定(ApiRestResponse三态success/fail/error、分页返回rows非data)、查询参数位置(beginTime/endTime/keywords/params在Entity上)、国际化NestedMessageSource、审计字段自动填充、Dozer对象映射。
  当用户在 DDD4J 项目中编写 Entity/Service/Controller、处理返回值格式、使用 DDD 注解时需要此技能。
  与 ddd4j-jackson(序列化)、ddd4j-mybatis(持久层)、ddd4j-validation(校验)、ddd4j-satoken(鉴权)协同使用。
license: Apache-2.0
---

# DDD4J Boot 核心开发约定

> 基于 ddd4j-boot 项目源码，编码那些 LLM 不知道的隐式约定。

## 为什么需要这个技能

LLM 训练数据中包含 Spring Boot/MyBatis-Plus 的 API 知识，但不知道 DDD4J 项目中的**架构约定**。直接让 LLM 写代码会产生架构上正确但风格上错误的胶水代码。本技能编码这些规则。

## Capability Boundaries

### ✅ Strong Suits
1. **实体定义规则** — 必须 extends BaseEntity→Model<T>(ActiveRecord)，审计字段已内置
2. **DDD 注解规则** — 区分 SOURCE 和 RUNTIME 注解，知道哪个注解已经是 Spring Bean
3. **Controller 层级** — BaseController(success/fail/error)→BaseMapperController(Dozer注入)
4. **Service 层级** — BaseServiceImpl 已注入 MessageSource/Cache/EventPublisher/Dozer
5. **返回值格式** — ApiRestResponse 三态(success/fail/error)，分页返回 rows 不是 data
6. **查询参数约定** — beginTime/endTime/keywords/params 在 Entity 上，不是独立 QueryDTO
7. **国际化** — NestedMessageSource，异常消息用 key 不用硬编码

### ⚠️ Requirements
1. 实体必须 extends BaseEntity（或 PaginationEntity），不能直接 extends Model
2. 使用 `@DomainService` 代替 `@Service`，使用 `@ApplicationService` 代替应用层 `@Service`
3. 分页查询使用 PaginationEntity，不要直接使用 MyBatis-Plus Page

### ❌ Out of Scope
1. JSON 序列化规则（null处理、日期格式、@Sensitive） → **ddd4j-jackson**
2. MyBatis-Plus 类型处理器、@Param("model")约定 → **ddd4j-mybatis**
3. 自定义校验注解 → **ddd4j-validation**
4. Sa-Token 扩展用法（StpKit扩建载荷） → **ddd4j-satoken**

## LLM 最常犯的10个错误

| # | 错误 | 正确做法 |
|---|------|---------|
| 1 | 把 `@DomainEntity` 当成 Spring Bean | 它是 SOURCE 保留，仅作文档，不是 @Component |
| 2 | 在 `@DomainService` 上再加 `@Service` | `@DomainService` 已经元标注了 @Service，重复会导致问题 |
| 3 | 创建独立的 QueryDTO 放查询参数 | beginTime/endTime/keywords/params 直接放在 Entity 上拓展字段 |
| 4 | 分页返回 `data` 字段 | 使用 `rows`（bootstrap-table 协议） |
| 5 | 直接 `new Page<T>()` 做分页 | 使用 `PaginationEntity<T>` 封装，默认15条/页 |
| 6 | MyBatis XML 中写 `#{entity.beginTime}` | 应写 `#{model.beginTime}`（@Param("model")） |
| 7 | 实体 implements Serializable | 必须 `extends BaseEntity<T extends Model<?>>`（ActiveRecord） |
| 8 | 响应返回 `{code:200,data:{}}` | 使用 `BaseController.success/fail/error` 返回 ApiRestResponse |
| 9 | 硬编码错误消息 | 使用 `getMessage("error.key")` NestedMessageSource |
| 10 | 手动 setter/getter 做对象映射 | BaseMapperController 已注入 Dozer，直接用 |

## 核心规则速查

```java
// ✅ 正确：实体定义
@Data
@TableName("sys_user")
public class SysUser extends BaseEntity<SysUser> {
    // createBy/createTime/updateBy/updateTime 已继承，不用再声明
    // isDeleted 已继承(@TableLogic)，不用再声明
    // beginTime/endTime/keywords 已继承(@TableField(exist=false))，用于查询
    private String username;
    private String password;
}

// ✅ 正确：DDD 注解
@DomainEntity          // SOURCE 保留 → 仅文档标注
public class Order { }
@DomainService         // RUNTIME 保留 → 已经是 @Service，不要再加
public class OrderService { }
@ApplicationService    // RUNTIME 保留 → 已经是 @Service，用于应用层
public class OrderAppService { }

// ✅ 正确：Controller 返回值
@RestController
public class UserController extends BaseMapperController {
    @GetMapping("/list")
    public ApiRestResponse<Result<UserDTO>> list(PaginationEntity<User> entity) {
        List<User> list = getBaseService().getPagedList(new Page<>(entity.getPageNo(), entity.getLimit()), entity);
        return success(new Result<>(list)); // 返回 {code:200, status:"success", rows:[...]}
    }
}

// ✅ 正确：分页查询
PaginationEntity<User> entity = new PaginationEntity<>();
entity.setPageNo(1); entity.setLimit(15); entity.setBeginTime(LocalDateTime.now().minusDays(7));
List<User> list = baseService.getPagedList(new Page<>(1, 15), entity);
// XML: <select id="getPagedList"> WHERE create_time >= #{model.beginTime} </select>
```

## Gotchas

1. **@DomainEntity 不能替代 @Component/@Service** — 它是 SOURCE 保留的文档注解
2. **@DomainService 已经包含 @Service** — 不要重复添加
3. **BaseEntity 的 beginTime/endTime 是查询辅助字段** — 不会持久化(@TableField(exist=false))
4. **PaginationEntity 的 getOffset() 是 Oracle 风格** — `((pageNo-1)*limit+1)`，不是0开头
5. **ApiRestResponse 有三态** — success(200)/fail(业务失败)/error(系统错误=500)
6. **分页返回字段是 rows 不是 data** — bootstrap-table 协议要求
7. **插入时审计字段自动填充** — createBy/createTime 由 MyBatis-Plus FieldFill.INSERT 处理
8. **更新时 updateBy/updateTime 自动填充** — 由 FieldFill.INSERT_UPDATE 处理
9. **BaseMapperController 注入的是 Dozer** — 做对象映射用 `mapper.map(source, Target.class)`
10. **所有 Controller 应继承 BaseController 或 BaseMapperController** — 获得 success/fail/error 方法

## Data Privacy
本技能不收集、存储或传输任何用户数据。
