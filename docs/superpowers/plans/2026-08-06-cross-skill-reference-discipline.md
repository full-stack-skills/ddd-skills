# ddd-skills 历史任务：跨技能引用纪律落地（2026-08-06）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 将 SKILL.md 中所有跨技能的相对路径引用（`../ddd-*/SKILL.md`）改为「技能名 + 安装命令」模式，消除单技能安装场景下的死链。

**Architecture:** 统一改写为 `hand off to **<skill-name>** skill` 并附 `npx skills add full-stack-skills/ddd-skills --skill <name>` 安装命令；顶层 `../DESIGN.md` 改为 GitHub 永久链接。此纪律后来固化为工作区 AGENTS.md 的「Cross-Skill Reference Discipline」规则。

**Tech Stack:** Markdown。

**Spec:** 无独立规格文档；依据提交 `5b7aa66` 还原。改写原因（记录于提交说明）：
1. 用户可能只安装单一 skill，相对路径目标不存在
2. Agent 按 skill 名调度，不按文件路径
3. 顶层相对路径在 GitHub 跨仓库渲染时通常 404

## Global Constraints

- 跨技能引用一律使用「技能名 + 安装命令」，禁止 `../` 相对路径

---

### Task 1: 8 个技能的跨技能引用改写

**Files:**
- Modify: `skills/ddd-api-designer/SKILL.md`（3 处）
- Modify: `skills/ddd-architecture-awesome/SKILL.md`（3 处）
- Modify: `skills/ddd-architecture-clean/SKILL.md`（约 5 处，16 行变更）
- Modify: `skills/ddd-architecture-doc/SKILL.md`（5 处）
- Modify: `skills/ddd-architecture-hexagonal/SKILL.md`（2 处）
- Modify: `skills/ddd-architecture-onion/SKILL.md`（4 处）
- Modify: `skills/ddd-architecture-selector/SKILL.md`（11 处）
- Modify: `skills/ddd-testing-strategist/SKILL.md`（4 处）

- [x] **Step 1: 逐技能将 `../ddd-*/SKILL.md` 链接改写为技能名 + 安装命令（共 41 增 39 删）**
- [x] **Step 2: 顶层 `../DESIGN.md` 改为 GitHub 永久链接**
- [x] **Step 3: 提交** — `5b7aa66 fix: 跨 skill 相对路径引用改为 skill 名 + 安装命令`
