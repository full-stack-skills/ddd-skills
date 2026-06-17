---
name: ddd4j-validation
description: |
  DDD4J 自定义校验规则技能。覆盖四个自定义约束(@AllowedValues/@PhoneNumber/@NumberValue/@StringDateValue)、@StringDateValue空字符串静默通过规则、校验器禁用默认消息插值约束、校验层位置(Param/DTO而非独立Validator)、libphonenumber国际手机号校验。
  当用户在 DDD4J 项目中编写参数校验、手机号/日期格式校验、枚举值校验时需要此技能。
  配合 ddd4j-core 技能使用。
license: Apache-2.0
---

# DDD4J 自定义校验规则

> 编码 DDD4J 项目中四个自定义 Bean Validation 约束的使用规则。LLM 不知道这些约束存在，会自己写校验逻辑。

## 为什么需要这个技能

LLM 会用 `@Pattern(regexp="^1[3-9]\\d{9}$")` 校验手机号——DDD4J 有 `@PhoneNumber`（基于 libphonenumber，支持国际化）。LLM 会用 `@DateFormat` 校验日期——DDD4J 有 `@StringDateValue`（空字符串静默通过）。这些自定义约束 LLM 完全不知道。

## Capability Boundaries

### ✅ Strong Suits
1. **@AllowedValues** — 字符串数组白名单校验，支持 nullable
2. **@PhoneNumber** — Google libphonenumber 手机号校验
3. **@NumberValue** — 正则数字格式校验
4. **@StringDateValue** — 日期字符串格式校验（空串通过）

### ❌ Out of Scope
1. 实体定义/返回值约定 → **ddd4j-core**
2. Jackson 序列化校验 → **ddd4j-jackson**

## LLM 最常犯的错误

| # | 错误 | 正确做法 |
|---|------|---------|
| 1 | `@Pattern(regexp="1[3-9]\\d{9}")` 校验手机号 | 用 `@PhoneNumber`（libphonenumber，国际化） |
| 2 | `@DateTimeFormat` 校验日期 | 用 `@StringDateValue(format="yyyy-MM-dd HH:mm:ss")` |
| 3 | 自定义 Validator 类做校验 | 校验注解放在 Param/DTO 字段上 |
| 4 | 给 @StringDateValue 的字段传空字符串 | 空字符串静默通过，不是校验失败 |
| 5 | 不写 message 属性 | 所有自定义约束必须显式写 message |
| 6 | 用 `@NotEmpty` 代替 `@AllowedValues.nullable=false` | nullable 控制空值行为 |

## 核心规则速查

```java
// ✅ 正确：四个自定义约束

// @AllowedValues — 白名单校验
public class UserParam extends BaseParam {
    @AllowedValues(values = {"MALE", "FEMALE", "OTHER"}, nullable = false,
        message = "性别必须为 MALE/FEMALE/OTHER 之一")
    private String gender;

    @AllowedValues(values = {"ACTIVE", "INACTIVE"}, nullable = true,
        message = "状态必须为 ACTIVE/INACTIVE")
    private String status;  // nullable=true → 空值通过校验
}

// @PhoneNumber — 手机号校验（libphonenumber）
public class RegisterParam extends BaseParam {
    @PhoneNumber(lang = "CN", message = "手机号格式不正确")
    private String phone;

    @PhoneNumber(lang = "US", message = "Invalid US phone number")
    private String usPhone;
}

// @NumberValue — 数字格式
public class IdParam {
    @NumberValue(regex = "^[0-9\\-]+$", message = "ID只能包含数字和横线")
    private String id;
}

// @StringDateValue — 日期字符串
public class DateParam {
    @StringDateValue(format = "yyyy-MM-dd HH:mm:ss", message = "日期格式错误")
    private String beginTime;   // "" 空字符串 → 静默通过
    // SimpleDateFormat.setLenient(false) → 严格模式
}
```

## Gotchas

1. **@StringDateValue 空字符串静默通过** — 空值不触发校验，需要用 @NotEmpty 配合
2. **所有自定义约束必须提供 message** — 校验器 `disableDefaultConstraintViolation` 禁用了默认消息
3. **@PhoneNumber 依赖 libphonenumber** — 需要引入 `com.googlecode.libphonenumber`
4. **@AllowedValues.nullable=true 时 null/空串都通过** — nullable=false 时空值不通过
5. **@NumberValue 默认正则 `^[0-9\\-]+$`** — 只允许数字和横线
6. **校验注解放在 Param/DTO 上** — 不要创建独立的 Validator 类
7. **@StringDateValue 使用严格模式** — `SimpleDateFormat.setLenient(false)`，非严格日期不通过

## Data Privacy
本技能不收集、存储或传输任何用户数据。
