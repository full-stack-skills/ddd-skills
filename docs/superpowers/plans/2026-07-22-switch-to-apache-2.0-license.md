# ddd-skills 历史任务：许可证切换为 Apache 2.0（2026-07-22）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 将项目开源协议统一为 Apache License 2.0。

**Architecture:** 许可证声明调整。注意：提交说明描述了 LICENSE 文件的全文替换（移除 BSD 2-Clause 与 GPLv3 信息），但该提交的实际 diff 仅含 plugin.json 一处变更——LICENSE 文件自初始提交 `014b0a4` 起即未再变更（当前即为 Apache 2.0 全文，46 KB）。

**Tech Stack:** JSON（plugin manifest）。

**Spec:** 无独立规格文档；依据提交 `536f515` 还原。

## Global Constraints

- 保留版权保护与责任限制条款
- 与 full-stack-skills 生态的仓库级 Apache 2.0 约定保持一致

---

### Task 1: 许可证声明切换

**Files:**
- Modify: `.claude-plugin/plugin.json`（license 字段，1 行）

- [x] **Step 1: 更新 plugin.json 的 license 声明为 Apache-2.0**
- [x] **Step 2: 提交** — `536f515 chore(license): 更新许可证文件为Apache 2.0版本`

> 备注：提交说明与实际 diff 存在偏差（见 Architecture），以磁盘上的 LICENSE 文件（Apache 2.0 全文）为准。
