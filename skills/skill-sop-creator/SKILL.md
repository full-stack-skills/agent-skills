---
name: skill-sop-creator
description: Guide for creating Standard Operating Procedures and SOPs and composite workflows by combining atomic Agent Skills. Use when users want to design, structure, or implement multi-step workflows that orchestrate multiple skills for complex business goals. Transforms loose tool collections into reliable, repeatable business processes.
---

# skill-sop-creator

Guide for creating Standard Operating Procedures (SOPs) and composite workflows by combining atomic Agent Skills. Use this skill when users want to design, structure, or implement multi-step workflows (SOPs) that orchestrate multiple skills to achieve complex business goals. This skill transforms loose collections of tools into reliable, repeatable business processes.

## Parameters

| Name     | Type   | Required | Description |
|----------|--------|----------|-------------|
| action   | string | true     | The action to perform. One of `search_skills`, `draft_sop`, `validate_sop`. |
| context  | string | false    | Context for the action (e.g. business goal for drafting, search query for searching). |

## Actions

### 1. Search Atomic Skills
Finds available atomic skills to include in your SOP.
- **Command**: `skill-sop-creator(action="search_skills", context="pdf processing")`
- **Output**: List of relevant atomic skills (e.g., `pdf-split`, `pdf-merge`, `ocr-extract`) that can be steps in your SOP.

### 2. Draft SOP Structure
Generates a draft `SKILL.md` for a composite skill (SOP) based on a business goal.
- **Command**: `skill-sop-creator(action="draft_sop", context="Daily Competitor Analysis Report")`
- **Output**: A structured markdown template defining the workflow steps, required atomic skills, and decision logic.

### 3. Validate SOP
Checks if a proposed SOP structure is valid and if referenced skills exist.
- **Command**: `skill-sop-creator(action="validate_sop", context="<sop_content>")`
- **Behavior**: Verifies that steps are logical and dependencies are met.

## SOP Design Principles

1. **Atomic Foundation**: SOPs should rely on small, single-purpose skills (e.g., `pencil-draw-rect` not `draw-entire-ui`).
2. **Deterministic Flow**: Define clear steps: Step 1 -> Step 2 -> Decision -> Step 3.
3. **Human-in-the-loop**: Include checkpoints where the Agent should ask for user confirmation.

## Example SOP Structure

```markdown
# Daily Competitor Analysis SOP

## 1. Information Gathering
- **Skill**: `web-search`
- **Action**: Search for "competitor name" + "news"
- **Output**: List of URLs

## 2. Content Extraction
- **Skill**: `web-scraper`
- **Input**: URLs from Step 1
- **Output**: Raw text

## 3. Analysis
- **Skill**: `llm-summarize`
- **Input**: Raw text
- **Prompt**: "Identify key pricing changes and feature launches."

## 4. Reporting
- **Skill**: `pdf-generator`
- **Input**: Summary
- **Output**: PDF Report
```

## Notes

- An SOP is essentially a "Composite Skill" that acts as a conductor for other "Atomic Skills".
- This skill helps you *design* that conductor.

## 国内适配

- 支持中文文档和中文注释
- 示例代码兼容国内开发环境
- 提供中文 FAQ 和常见问题解答

## 能力边界

### ✅ 适用场景
- 当你需要使用此技能对应的技术栈时
- 当项目需要遵循最佳实践时
- 当需要快速上手或深入理解核心概念时

### ⚠️ 需要注意
- 复杂业务逻辑需要结合具体场景调整
- 性能优化需要根据实际数据量评估

### ❌ 不适用场景
- 不相关的技术栈或框架
- 需要完全自定义的特殊场景
