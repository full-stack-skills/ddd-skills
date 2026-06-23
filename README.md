<div align="center">

# ddd-skills

**Domain-Driven Design architecture skills — COLA, microservices, hexagonal, clean, event-driven**

[![GitHub](https://img.shields.io/badge/github-full--stack--skills%2Fddd-skills-green.svg)](https://github.com/full-stack-skills/ddd-skills)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-purple.svg)](https://agentskills.io)

English | [简体中文](./README.zh-CN.md)

[Introduction](#-introduction) ·
[Install](#-install) ·
[Skills](#-skills) ·
[Supported Agents](#-supported-agents) ·
[Ecosystem](#-ecosystem)

</div>

---

## 📖 Introduction

**DDD Architecture Skills** is a curated collection of Agent Skills for AI coding agents, part of the [Full Stack Skills](https://github.com/partme-ai/full-stack-skills) ecosystem maintained by [PartMe.AI](https://github.com/partme-ai).

This package includes **16 skills**. Each skill is a self-contained `SKILL.md` file that AI agents load on-demand.

## 📦 Install

```bash
npx skills add full-stack-skills/ddd-skills
```

Or install specific skills:

```bash
npx skills add full-stack-skills/ddd-skills --skill <skill-name>
```

## 🎯 Skills (16)

| Skill | Description |
|-------|-------------|
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

## 🤖 Supported Agents

Works with [Claude Code](https://code.claude.com), [Codex](https://developers.openai.com/codex), [Cursor](https://cursor.com), [OpenCode](https://opencode.ai), [Gemini CLI](https://geminicli.com), [GitHub Copilot](https://github.com/features/copilot), [Windsurf](https://codeium.com/windsurf), and [70+ others](https://agentskills.io/clients).

### Claude Code Installation

**Option 1: npx skills CLI (Recommended)**

```bash
npx skills add full-stack-skills/ddd-skills
```

**Option 2: Manual Installation**

```bash
git clone https://github.com/full-stack-skills/ddd-skills.git
cp -r ddd-skills/skills/* .claude/skills/
```

For more details, see the [Claude Code Skills Guide](https://code.claude.com/docs/en/skills) and [Agent Skills Spec](https://agentskills.io/).

## 🌐 Ecosystem

| Resource | Link |
|----------|------|
| **Full Stack Skills** | [github.com/partme-ai/full-stack-skills](https://github.com/partme-ai/full-stack-skills) |
| **All Skill Groups** | [github.com/full-stack-skills](https://github.com/full-stack-skills) |
| **Agent Skills Spec** | [agentskills.io](https://agentskills.io) |
| **Skills CLI** | [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills) |

## 📄 License

Apache 2.0 — see [LICENSE](LICENSE).
