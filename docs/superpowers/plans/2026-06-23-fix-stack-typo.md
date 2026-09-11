# ddd-skills 历史任务：修正拼写错误 statck → stack（2026-06-23）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 全库修正拼写错误 `statck` → `stack`（遗留自远端仓库 URL `full-statck-skills` 时期的笔误）。

**Architecture:** 纯文档修正，不涉及技能内容语义变化。

**Tech Stack:** Markdown、JSON。

**Spec:** 无独立规格文档；依据提交 `d7a7d2f` 还原。

## Global Constraints

- 仅做拼写替换，不改动其他内容

---

### Task 1: 全库拼写修正

**Files:**
- Modify: `.claude-plugin/plugin.json`（2 处）
- Modify: `CLAUDE.md`（1 处）
- Modify: `README.md`（6 处）
- Modify: `README.zh-CN.md`（6 处）

- [x] **Step 1: 替换全部 `statck` → `stack`（共 15 行）**
- [x] **Step 2: 提交** — `d7a7d2f fix: 修正拼写错误 statck -> stack`
