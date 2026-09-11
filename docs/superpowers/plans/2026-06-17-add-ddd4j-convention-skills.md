# ddd-skills 历史任务：新增 5 个 ddd4j 项目约定技能（2026-06-17）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 从 java-skills 引入 5 个 ddd4j 项目专属约定技能，将 LLM 无法自行得知的项目级编码约定固化为 Agent Skills。

**Architecture:** 每个约定主题一个独立技能（纯 SKILL.md，无 references/examples），并同步登记到 plugin.json 的 skills 数组。

**Tech Stack:** Markdown（Agent Skills 规范）、JSON（plugin manifest）。

**Spec:** 无独立规格文档；依据提交 `26c1899` 还原。

## Global Constraints

- 技能编码的是项目特定约定（project-specific conventions），非通用 DDD 知识
- plugin.json 的 skills 数组必须随新增技能同步更新（16 → 21）

---

### Task 1: 引入 5 个 ddd4j 约定技能

**Files:**
- Create: `skills/ddd4j-core/SKILL.md`（108 行 — Entity/Controller/Service 及注解核心约定）
- Create: `skills/ddd4j-jackson/SKILL.md`（93 行 — 日期/null/@Sensitive 序列化规则）
- Create: `skills/ddd4j-mybatis/SKILL.md`（100 行 — MyBatis-Plus ActiveRecord/type handlers 约定）
- Create: `skills/ddd4j-validation/SKILL.md`（91 行 — @PhoneNumber/@StringDateValue 自定义校验）
- Create: `skills/ddd4j-satoken/SKILL.md`（82 行 — Sa-Token StpKit payload/SaTempKit 扩展）
- Modify: `.claude-plugin/plugin.json`

- [x] **Step 1: 从 java-skills 提取 5 个约定主题，逐一编写 SKILL.md**
- [x] **Step 2: plugin.json 注册 5 个新技能（共 480 行新增）**
- [x] **Step 3: 提交** — `26c1899 feat: add 5 ddd4j convention skills from java-skills`
