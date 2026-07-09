<div align="center">

# agent-skills

**技能管理工具 — 创建、评测、安装、编排 Agent Skills**

[![GitHub](https://img.shields.io/badge/github-full--stack--skills%2Futility--skills-green.svg)](https://github.com/full-stack-skills/agent-skills)
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

## 🎯 技能列表 (8)

| 技能 | 描述 |
|------|------|
| `codegraph` | 基于 tree-sitter 知识图谱的语义代码智能工具，为 AI 编码代理提供代码结构查询、符号追踪、变更影响分析、调用链查找能力。20+语言，100%本地。 |
| `skill-awesome` | The canonical knowledge base for designing Agent Skills. Contains distilled Agent Skills specifications (naming conve... |
| `skill-creator` | Guide for creating effective skills. This skill should be used when users want to create a new skill (or update an ex... |
| `skill-installer` | Manages the installation and discovery of AI skills from the PartMe marketplace. |
| `skill-official-evaluation` | > |
| `skill-sop-creator` | Guide for creating Standard Operating Procedures and SOPs and composite workflows by combining atomic Agent Skills. U... |
| `skill-trace-checker` | > |
| `skill-trace-evaluation` | 对任意 Agent Skill 做 TRACE 五维度评测（T/R/A/C/E），输出 Markdown/HTML 报告与雷达图；当用户要求“TRACE 评测/打分/生成 TRACE 报告/五维度评估”或需要改进建议时使用。 |


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

Apache 2.0
