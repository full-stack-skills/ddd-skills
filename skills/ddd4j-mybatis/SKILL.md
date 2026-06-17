---
name: ddd4j-mybatis
description: |
  DDD4J MyBatis-Plus 持久层约定技能。覆盖 ActiveRecord 模式(Entity必须extends BaseEntity→Model<T>)、@TableLogic逻辑删除、审计字段自动填充(createBy/createTime/updateBy/updateTime)、@Param("model")XML参数约定、PaginationEntity分页封装(15条/页/Oracle风格offset)、类型处理器体系(BooleanEnum/JSON双套/ListSetArray拆分)、BaseMapper内置getPagedList/setStatus/getCountBy*等方法。
  当用户在 DDD4J 项目中定义 Entity/Repository、编写 Mapper XML、使用分页查询、配置类型处理器时需要此技能。
  配合 ddd4j-core 技能使用。
license: Apache-2.0
---

# DDD4J MyBatis-Plus 持久层约定

> 编码 DDD4J 项目中 MyBatis-Plus 的使用规则。LLM 会用标准 MyBatis-Plus 写法，但不符项目约定。

## 为什么需要这个技能

LLM 会用 `implements Serializable` 定义实体——DDD4J 要求 `extends BaseEntity<T extends Model<?>>`（ActiveRecord模式）。LLM 会在 XML 中写 `#{entity.beginTime}`——DDD4J 要求 `#{model.beginTime}`（@Param("model")）。这些约定 LLM 不知道。

## Capability Boundaries

### ✅ Strong Suits
1. **实体继承链** — BaseEntity→Model<T>(ActiveRecord)，PaginationEntity→BaseEntity
2. **审计字段** — createBy/createTime(INSERT填充)、updateBy/updateTime(INSERT_UPDATE填充)
3. **逻辑删除** — @TableLogic 标记 isDeleted 字段
4. **@Param("model") 约定** — getPagedList 的参数名是 model，不是 entity
5. **分页封装** — PaginationEntity（默认15条/页，Oracle风格offset=(pageNo-1)*limit+1）
6. **类型处理器** — BooleanEnum(0/1)、JSON(双套)、List/Set/Array
7. **BaseMapper 内置方法** — getPagedList/setStatus/getCountBy*

### ❌ Out of Scope
1. 实体/Controller/Service 继承层级 → **ddd4j-core**
2. Jackson JSON 配置 → **ddd4j-jackson**

## LLM 最常犯的错误

| # | 错误 | 正确做法 |
|---|------|---------|
| 1 | `implements Serializable` | `extends BaseEntity<T>`（继承 Model<T>，ActiveRecord） |
| 2 | 重复声明 createBy/createTime | BaseEntity 已声明，子类不要再加 |
| 3 | XML 写 `#{entity.beginTime}` | 写 `#{model.beginTime}` |
| 4 | `new Page<T>(1, 10)` | 用 `PaginationEntity<T>` 封装 |
| 5 | 逻辑删除写 `UPDATE SET is_delete=1` | 用 @TableLogic，MyBatis-Plus 自动处理 |
| 6 | 自定义 Boolean 字段映射 | 用 BooleanEnum + CustomBooleanEnumTypeHandler |
| 7 | 手动 offset 计算 `(pageNo-1)*limit` | PaginationEntity.getOffset() 已封装 |

## 核心规则速查

```java
// ✅ 正确：实体定义
@Data
@TableName("sys_user")
public class SysUser extends BaseEntity<SysUser> {   // Model<T> → ActiveRecord
    // ↓ 已继承，不再声明
    // Long createBy, createTime, updateBy, updateTime
    // Integer isDeleted (@TableLogic)
    // LocalDateTime beginTime, endTime (@TableField(exist=false), @JsonIgnore)
    // String keywords, params (@TableField(exist=false), @JsonIgnore)

    private String username;
    private String status;    // BooleanEnum 映射: 0=否, 1=是
}

// ✅ 分页查询
PaginationEntity<SysUser> entity = new PaginationEntity<>();
entity.setPageNo(1);          // 第1页
entity.setLimit(15);          // 默认15条
entity.setBeginTime(LocalDateTime.now().minusDays(7));
List<SysUser> list = baseService.getPagedList(
    new Page<>(entity.getPageNo(), entity.getLimit()), entity
);

// ✅ Mapper XML（注意参数名 model）
<select id="getPagedList" resultType="SysUser">
    SELECT * FROM sys_user
    WHERE is_deleted = 0
    <if test="model.beginTime != null">
        AND create_time >= #{model.beginTime}
    </if>
</select>

// ✅ 内置方法（BaseServiceImpl 已提供）
baseService.setStatus(id, "1");          // @Transactional
baseService.getCountByName("admin");
baseService.getCountByCode("ADMIN");
baseService.getCountByParent(parentId);
baseService.getValue("some_key");        // 从缓存读取
```

## Gotchas

1. **实体必须 extends BaseEntity，不能 implements Serializable** — 失去 ActiveRecord 能力
2. **@Param("model") 是固定约定** — XML 引用必须是 #{model.xxx}
3. **PaginationEntity.getOffset() 是 Oracle 风格** — `((pageNo−1)*limit+1)`，不是0开头
4. **beginTime/endTime 在 Entity 上，不在独立 QueryDTO** — 加 `@TableField(exist=false)` 即可
5. **BooleanEnum 映射 Integer 0/1** — 数据库不用原生 Boolean 类型
6. **JSON 类型处理器有两套** — 核心用 fastjson2(jdbc包)，cmet用 hutool
7. **逻辑删除条件自动拼接** — SQL 不用写 `is_deleted = 0`（但复杂查询建议加上）
8. **setStatus 是 @Transactional** — 子类重写也必须加上事务注解

## Data Privacy
本技能不收集、存储或传输任何用户数据。
