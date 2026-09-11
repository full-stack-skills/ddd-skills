# ddd-skills 历史任务：ddd4j 技能迁出与全库链接审计修复（2026-09-11）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 将 5 个 ddd4j 项目专属技能迁出至独立的 `ddd4j-skills` 包（本包回归通用 DDD 技能定位），并对全库做一次完整的链接审计修复（153 处）与 SKILL.md 行数约束治理。

**Architecture:** 两步走：先做技能迁移（清单与技能文件同步增删），再做机械化链接修复（数字前缀对齐）+ 跨技能引用纪律扫尾 + 超长 SKILL.md 外移瘦身。

**Tech Stack:** Markdown、JSON（plugin manifest）、Python（批量链接审计/修复脚本）。

**Spec:** 无独立规格文档；依据当日 2 个提交（877c251、5ad7a63）还原。

## Global Constraints

- plugin.json 的 skills 数组必须与磁盘技能目录严格一致（21 → 16）
- SKILL.md 必须低于 500 行；超长章节外移到 `references/`（按需加载）
- 跨技能引用一律使用「技能名 + 安装命令」（延续 5b7aa66 纪律）

---

### Task 1: 迁移 ddd4j 专属技能到 ddd4j-skills 包

**Files:**
- Delete: `skills/ddd4j-{core,jackson,mybatis,satoken,validation}/SKILL.md`（共 474 行迁出）
- Modify: `.claude-plugin/plugin.json`（移除 5 项注册）

- [x] **Step 1: 删除 5 个 ddd4j 技能目录内容（迁往同组织 `ddd4j-skills` 包）**
- [x] **Step 2: 同步收缩 plugin.json skills 数组**
- [x] **Step 3: 提交** — `877c251 refactor: 迁移 ddd4j 专属技能`

### Task 2: 跨技能引用纪律扫尾 + awesome 拆分 sources

**Files:**
- Modify: 11 个技能的 `SKILL.md` / `references/` / `examples/`（链接目标补齐数字前缀，与磁盘文件名对齐）
- Modify: `skills/ddd-architecture-selector/examples/0*.md`（5 个案例文件的 9 处 `../ddd-*` 兄弟链接改为技能名 + 安装命令）
- Modify: `skills/ddd-architecture-cola/references/{07-archunit-validation,14-ddd4j-scaffold}.md`（修正 scripts/、examples/ 相对路径上溯）
- Create: `skills/ddd-architecture-awesome/references/00-sources.md`（自 SKILL.md 第 11 节外移的 47 行来源清单）
- Modify: `skills/ddd-architecture-awesome/SKILL.md`（510 → 467 行，移除指向不存在的仓库根 `DESIGN.md` 的死引用）
- Modify: `CLAUDE.md`（行数预算更新为实测 168–470，注明 <500 红线）

- [x] **Step 1: 批量审计全部 markdown 链接 + 反引号文件引用，修复 153 处死链（144 自动 + 8 手工层级修正 + 1 死引用删除）**
- [x] **Step 2: selector 案例文件的 9 处兄弟技能相对路径改为纪律格式**
- [x] **Step 3: awesome 外移 Sources 节至 `references/00-sources.md`，行数降至 500 以内**
- [x] **Step 4: 全量复验：死链 0、悬空纯文本引用 0、跨技能违规引用 0、16 技能与清单一致**
- [x] **Step 5: 提交** — `5ad7a63 chore: 完成跨技能引用纪律扫尾 + ddd-architecture-awesome 拆 sources references`
