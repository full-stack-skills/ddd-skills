# ddd-skills 历史任务：仓库初始化与 16 技能迁移（2026-06-05）

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
> **本计划为历史任务回填**：依据 git 提交记录还原，全部任务已完成，复选框均为 `- [x]`。

**Goal:** 将 full-stack-skills monorepo 中的 16 个 DDD 技能拆分为独立的 ddd-skills 仓库，建立每技能独立目录结构、双语 README 和插件清单。

**Architecture:** 单仓库单包（one package repo）模式：`skills/<skill-name>/SKILL.md` 为技能主体，`examples/`、`references/` 存放支撑材料，`.claude-plugin/plugin.json` 作为插件清单注册全部技能。

**Tech Stack:** Markdown（Agent Skills 规范）、JSON（plugin manifest）、Git。

**Spec:** 无独立规格文档；依据当日 8 个提交（014b0a4 → 467623c）还原。

## Global Constraints

- 技能源自 full-stack-skills monorepo，迁移后保持内容不变（仅重组目录）
- README 提供英文版（README.md）与中文版（README.zh-CN.md）
- 插件清单 `.claude-plugin/plugin.json` 的 `skills` 数组必须与磁盘技能目录一一对应

---

### Task 1: 初始导入 monorepo 技能集合

**Files:**
- Create: `skills/`（当时为扁平结构 + `skills/SKILL.md`）
- Create: `LICENSE`（首次引入，此后未再变更）
- Create: `README.md`（2671 行的全量说明）

- [x] **Step 1: 导入 16 个技能及支撑材料** — 含各技能 examples、evaluation-report.html 等
- [x] **Step 2: 提交** — `014b0a4 feat: initial skills from full-stack-skills monorepo (16 skills)`

### Task 2: 重组为每技能独立目录 + 双语 README

**Files:**
- Create: `skills/<skill-name>/SKILL.md` ×16（每技能一个目录）
- Create: `README.zh-CN.md`（中文版，77 行）
- Modify: `README.md`（2698 行精简为目录化文档）
- Create: `.gitignore`（95 行）

- [x] **Step 1: 将扁平技能文件迁移为 `skills/<skill-name>/` 目录结构**
- [x] **Step 2: 拆分双语 README**
- [x] **Step 3: 提交** — `ca70c65 feat: migrate 16 skills with bilingual README`

### Task 3: 添加插件清单 plugin.json

**Files:**
- Create: `.claude-plugin/plugin.json`（30 行，注册 16 个技能）

- [x] **Step 1: 编写插件清单**（name/version/description/author/homepage/repository/skills 数组）
- [x] **Step 2: 提交** — `dd2433a feat: add .claude-plugin/plugin.json`

### Task 4: 清理 agent 配置文件

**Files:**
- Modify: `.gitignore`

- [x] **Step 1: 移除 AGENTS.md / AGENTS_EN.md 的跟踪约定，改为跟踪 CLAUDE.md** — `271cb27 chore: remove AGENTS.md/AGENTS_EN.md, track CLAUDE.md, update .gitignore`
- [x] **Step 2: 二次清理 agent 相关忽略规则** — `3a1a725 chore: clean up agent files`

### Task 5: README 补充 Claude Code 安装说明

**Files:**
- Modify: `README.md` / `README.zh-CN.md`
- Create: `CLAUDE.md`（仓库结构与技能创作约定，72 行）

- [x] **Step 1: 双语 README 增加 npx skills CLI 与手动安装两种方式**
- [x] **Step 2: 提交** — `9ff4d44 docs: add Claude Code installation instructions to README`

### Task 6: 合并远端分支并修正 plugin.json

- [x] **Step 1: 合并远端 main**（远端 URL 当时含拼写 full-statck-skills）— `0b16226 Merge branch 'main' of https://github.com/full-statck-skills/ddd-skills`
- [x] **Step 2: 修正 plugin.json（2 处字段）** — `467623c feat: add .claude-plugin/plugin.json`
