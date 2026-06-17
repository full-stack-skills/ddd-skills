---
name: ddd4j-jackson
description: |
  DDD4J Jackson 序列化规则技能。覆盖日期格式(非ISO-8601的yyyy-MM-dd HH:mm:ss)、Null值序列化规则(数组→[]、字符串→""、数字/布尔→保持null)、@Sensitive敏感数据脱敏(5种策略)、JacksonKit与WebObjectMapper双实例(Redis用JacksonKit/Web用ObjectMapper)、ToLongDeserializer(前端JS精度丢失处理)。
  当用户在 DDD4J 项目中使用 Jackson、需要配置ObjectMapper、处理敏感数据脱敏、解决前后端JSON格式问题时使用。
  配合 ddd4j-core 技能使用。
license: Apache-2.0
---

# DDD4J Jackson 序列化规则

> 编码 DDD4J 项目中 Jackson 的使用规则。LLM 训练数据中的 Jackson 用法会与项目约定冲突。

## 为什么需要这个技能

LLM 会用 ISO-8601 格式 `2024-03-15T10:30:00Z` 序列化日期——DDD4J 要求 `2024-03-15 10:30:00`。LLM 不会给 null 数组返回 `[]`——DDD4J 前端期望 null 数组永远不出现。这些规则 LLM 不知道。

## Capability Boundaries

### ✅ Strong Suits
1. **日期格式规则** — yyyy-MM-dd HH:mm:ss（非ISO-8601）
2. **Null值规则** — 数组→[]、字符串→""、数字→保持null、布尔→保持null
3. **@Sensitive 脱敏** — 5种策略(手机/身份证/姓名/地址/邮箱)
4. **JacksonKit** — Redis/缓存专用的独立ObjectMapper(DefaultTyping.NON_FINAL)
5. **ToLongDeserializer** — 处理前端JS大数字精度丢失

### ❌ Out of Scope
1. 实体继承/返回值约定 → **ddd4j-core**
2. MyBatis JSON字段处理 → **ddd4j-mybatis**
3. API签名/安全 → **ddd4j-satoken**

## LLM 最常犯的错误

| # | 错误 | 正确做法 |
|---|------|---------|
| 1 | 日期用 ISO-8601 `2024-03-15T10:30:00Z` | 用 `yyyy-MM-dd HH:mm:ss` |
| 2 | null 数组返回 `null` | 返回 `[]` |
| 3 | null 字符串返回 `null` | 返回 `""` |
| 4 | 给 null 数字返回 `0` | 数字/布尔保持 null，不转化 |
| 5 | 手动写脱敏逻辑 | 用 `@Sensitive(strategy = SensitiveStrategy.PHONE)` |
| 6 | 缓存用 Web ObjectMapper | 用 JacksonKit（DefaultTyping.NON_FINAL + ALL visibility） |
| 7 | 大数字直接返回 Long | 用 ToLongDeserializer 处理精度丢失 |

## 核心规则速查

```java
// ❌ 错误：LLM会这样写
@JsonFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss")
private LocalDateTime createTime;

// ✅ 正确：DDD4J 已全局配置
private LocalDateTime createTime;  // 序列化为 "2024-03-15 10:30:00"

// ✅ Null 处理：前端收到的 JSON
// null 数组 → []      (默认开启)
// null 字符串 → ""    (默认开启)
// null 对象 → {}      (默认开启)
// null 数字 → null    (不转化)
// null 布尔 → null    (不转化)

// ✅ 敏感数据脱敏
public class UserVO {
    @Sensitive(strategy = SensitiveStrategy.PHONE)
    private String phone;    // 185****1653
    @Sensitive(strategy = SensitiveStrategy.ID_CARD)
    private String idCard;   // 1234****5678
    @Sensitive(strategy = SensitiveStrategy.USERNAME)
    private String name;     // 张*三
    @Sensitive(strategy = SensitiveStrategy.EMAIL)
    private String email;    // r*****o@qq.com
    @Sensitive(strategy = SensitiveStrategy.ADDRESS)
    private String address;  // 北京市****海淀区****
}

// ✅ 两个 ObjectMapper
// Web用：JacksonAutoConfiguration 自动配置的，用于 HTTP 请求响应
@Autowired private ObjectMapper objectMapper;
// 缓存用：JacksonKit 独立 ObjectMapper，带 DefaultTyping.NON_FINAL
JacksonKit.toJson(obj);    // 序列化时写入类型信息
JacksonKit.fromJson(json, Type.class);  // 反序列化时根据类型信息恢复
```

## Gotchas

1. **不要自己配置 Jackson2ObjectMapperBuilderCustomizer** — DDD4J 已预配置，冲突会导致日期格式错乱
2. **对象存 Redis 必须用 JacksonKit 不是 ObjectMapper** — Web ObjectMapper 没有 DefaultTyping，多态反序列化会丢类型
3. **数字 null 不会转 0** — 前端需处理数字可能为 null 的情况（与数组/字符串不同）
4. **@Sensitive 只对 String 类型生效** — 其他类型忽略
5. **ToLongDeserializer 接受字符串转 Long** — 前端传 `"1234567890123456789"` 也能正确解析
6. **日期格式同时影响序列化和反序列化** — 前端提交的日期也必须 `yyyy-MM-dd HH:mm:ss`

## Data Privacy
本技能不收集、存储或传输任何用户数据。@Sensitive 注解仅用于在序列化时脱敏已存在的敏感数据。
