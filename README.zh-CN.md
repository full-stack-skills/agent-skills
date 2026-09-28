<div align="center">

# agent-skills

**技能管理工具 — 创建、评测、安装、编排 Agent Skills**

[![GitHub](https://img.shields.io/badge/github-full--stack--skills%2Fagent--skills-green.svg)](https://github.com/full-stack-skills/agent-skills)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-兼容-purple.svg)](https://agentskills.io)

[English](./README.md) | 简体中文

</div>

---

## 📖 简介

**实用技能工具** 提供 Agent Skills 的完整工具链 — 从创建、评测、安装到工作流编排。属于 [Full Stack Skills](https://github.com/partme-ai/full-stack-skills) 生态，由 [PartMe.AI](https://github.com/partme-ai) 维护。

## 📦 安装

```bash
npx skills add full-stack-skills/agent-skills
```

## 🎯 技能列表 (16)

| 技能 | 描述 |
|------|------|
| `agent-browser` | AI 代理的浏览器自动化 CLI。安装、命令、选择器、会话、CDP/流式传输。 |
| `agent-plugins-skill` | 基于 Agent Plugins 1.0.0 开发、迁移、审查可移植插件与客户端，覆盖清单、技能、MCP、扩展和一致性验证。 |
| `autoresearch` | 面向可验证目标的有界自主迭代，支持调试、修复、安全审计、回归检查等子流程。 |
| `browser-trace` | 旁路采集浏览器 CDP 事件、截图和 DOM，用于调试和审计浏览器自动化。 |
| `caveman` | 在保留技术信息的前提下压缩回答，支持多种简洁程度。 |
| `code-generator` | 代码生成指南 — 基于模板的生成、脚手架和自动化代码创建。 |
| `codebase-design` | 使用深模块、清晰边界和可测试接口设计或改进代码库。 |
| `codegraph` | 基于 tree-sitter 知识图谱的语义代码智能工具，为 AI 编码代理提供代码结构查询、符号追踪、变更影响分析、调用链查找能力。20+语言，100%本地。 |
| `mcp-builder` | 创建高质量 MCP（Model Context Protocol）服务器的指南。Python FastMCP / TypeScript SDK。 |
| `skill-awesome` | 设计 Agent Skills 的权威知识库，包含精炼的 Agent Skills 规范。 |
| `skill-creator` | 创建有效技能的指南。当用户想要创建新技能或更新现有技能时使用。 |
| `skill-installer` | 管理 PartMe 市场中 AI 技能的安装和发现。 |
| `skill-official-evaluation` | Agent Skills 质量评估的官方评估框架。 |
| `skill-sop-creator` | 通过组合原子 Agent Skills 创建标准操作流程和复合工作流的指南。 |
| `skill-trace-checker` | TRACE 评估报告和技能质量验证检查器。 |
| `skill-trace-evaluation` | 对任意 Agent Skill 做 TRACE 五维度评测（T/R/A/C/E），输出 Markdown/HTML 报告与雷达图。 |


## 🤖 支持的智能体

适用于 [Claude Code](https://code.claude.com)、[Codex](https://developers.openai.com/codex)、[Cursor](https://cursor.com)、[OpenCode](https://opencode.ai)、[Gemini CLI](https://geminicli.com)、[GitHub Copilot](https://github.com/features/copilot)、[Windsurf](https://codeium.com/windsurf) 及 [70+ 其他](https://agentskills.io/clients)。

### Claude Code 安装

**方式一：npx skills CLI（推荐）**

```bash
npx skills add full-stack-skills/agent-skills
```

**方式二：手动安装**

```bash
git clone https://github.com/full-stack-skills/agent-skills.git
cp -r agent-skills/skills/* .claude/skills/
```

更多详情请参阅 [Claude Code 技能指南](https://code.claude.com/docs/en/skills) 和 [Agent Skills 规范](https://agentskills.io/)。

## 📄 License

仓库自有内容采用 Apache 2.0，详见 [LICENSE](LICENSE)。内含的第三方组件保留各自的原始许可证，详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) 以及各技能目录中的许可证文件。
