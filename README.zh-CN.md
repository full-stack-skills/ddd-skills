<div align="center">

# ddd-skills

**Domain-Driven Design architecture skills — COLA, microservices, hexagonal, clean, event-driven**

[![GitHub](https://img.shields.io/badge/github-full--stack--skills%2Fddd-skills-green.svg)](https://github.com/full-stack-skills/ddd-skills)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-兼容-purple.svg)](https://agentskills.io)

[English](./README.md) | 简体中文

[简介](#-简介) ·
[安装](#-安装) ·
[技能列表](#-技能列表) ·
[支持的智能体](#-支持的智能体) ·
[生态](#-生态)

</div>

---

## 📖 简介

**DDD 架构技能** 是一组 AI 编码智能体技能，属于 [Full Stack Skills](https://github.com/partme-ai/full-stack-skills) 生态，由 [PartMe.AI](https://github.com/partme-ai) 维护。

本包包含 **16 个技能**。每个技能是一个独立的 `SKILL.md` 文件，AI 智能体按需加载。

## 📦 安装

```bash
npx skills add full-stack-skills/ddd-skills
```

或按需安装特定技能：

```bash
npx skills add full-stack-skills/ddd-skills --skill <skill-name>
```

## 🎯 技能列表 (16)

| 技能 | 描述 |
|------|------|
| `ddd-api-designer` | API design from domain model — CQRS command/query separation, REST API endpoint design, data object conversion chain ... |
| `ddd-architecture-awesome` | Provides comprehensive guidance for Domain-Driven Design (DDD) concepts, strategic design, tactical design, and archi... |
| `ddd-architecture-clean` | Comprehensive guidance for Clean Architecture (整洁架构) — Robert C. Martin's Clean Architecture with Enterprise Business... |
| `ddd-architecture-cola` | Comprehensive guidance for COLA v5 Architecture (菱形架构) — Alibaba's COLA framework with adapter/application/domain/inf... |
| `ddd-architecture-doc` | DDD architecture documentation generation — C4 model diagrams (Context/Container/Component/Code), ADR (Architecture D... |
| `ddd-architecture-evaluator` | DDD architecture evaluation and evolution — DDD maturity 5-level model (AdHoc/Aware/Applied/Scaled/Optimized), archit... |
| `ddd-architecture-hexagonal` | Comprehensive guidance for Hexagonal Architecture (Ports & Adapters) — Alistair Cockburn's hexagonal architecture wit... |
| `ddd-architecture-layered` | Comprehensive guidance for DDD Layered Architecture (DDD 四层架构) — Traditional 3-layer to DDD 4-layer transformation wi... |
| `ddd-architecture-onion` | Comprehensive guidance for Onion Architecture (洋葱架构) — Jeffrey Palermo's onion architecture with domain-centric layer... |
| `ddd-architecture-selector` | Architecture selection decision guide — help users choose from 5 DDD architectures (Layered/Onion/Hexagonal/Clean/COL... |
| `ddd-code-reviewer` | DDD code review and anti-pattern detection — anemia model detection, layered compliance checking, aggregate design re... |
| `ddd-cqrs-architecture` | Comprehensive guidance for CQRS Architecture — independent CQRS skill covering L1/L2/L3 implementation levels, Event ... |
| `ddd-devops-integration` | DDD DevOps integration — CI/CD pipeline with ArchUnit automated architecture validation, multi-module build optimizat... |
| `ddd-domain-designer` | Domain-driven design complete workflow — event-storming-driven 6-step domain modeling process, aggregate design 5-pri... |
| `ddd-event-storming` | Event Storming workshop facilitation guide — collaborative domain exploration methodology with 6-step process (chaos ... |
| `ddd-testing-strategist` | DDD testing strategy — domain model unit testing, aggregate root testing, repository testing with mocks, application ... |

## 🤖 支持的智能体

适用于 [Claude Code](https://code.claude.com)、[Codex](https://developers.openai.com/codex)、[Cursor](https://cursor.com)、[OpenCode](https://opencode.ai)、[Gemini CLI](https://geminicli.com)、[GitHub Copilot](https://github.com/features/copilot)、[Windsurf](https://codeium.com/windsurf) 及 [70+ 其他平台](https://agentskills.io/clients)。

### Claude Code 安装

**方式一：npx skills CLI（推荐）**

```bash
npx skills add full-stack-skills/ddd-skills
```

**方式二：手动安装**

```bash
git clone https://github.com/full-stack-skills/ddd-skills.git
cp -r ddd-skills/skills/* .claude/skills/
```

更多详情请参阅 [Claude Code 技能指南](https://code.claude.com/docs/en/skills) 和 [Agent Skills 规范](https://agentskills.io/)。

## 🌐 生态

| 资源 | 链接 |
|------|------|
| **Full Stack Skills** | [github.com/partme-ai/full-stack-skills](https://github.com/partme-ai/full-stack-skills) |
| **全部技能组** | [github.com/full-stack-skills](https://github.com/full-stack-skills) |
| **Agent Skills 规范** | [agentskills.io](https://agentskills.io) |
| **Skills CLI** | [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills) |

## 📄 许可证

Apache 2.0 — 详见 [LICENSE](LICENSE)。
