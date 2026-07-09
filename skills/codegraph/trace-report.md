# TRACE Evaluation Report

Target: `/Users/wandl/workspaces/workspace-full-stack-skills/full-stack-skills-repositories/agent-skills/skills/codegraph/`

## Overall Assessment

**Overall Score: 4.4 / 5**
**Overall Rating: Good (良好)**

文档体系完整，边界清晰，Gotchas 和 FAQ 覆盖到位。主要扣分项：脚本未能正确识别 Capability Boundaries 章节导致 T3/R4/A1 基分偏低，经语义校准后仍受限于 ±0.3 调幅规则；examples 为单文件含 7 个工作流但脚本按文件计数导致 C1/E2 基分偏低。

## TRACE Dimension Explanation

SkillHub TRACE 评测体系从**可信任度、可靠性、适用性、规范性、有效性**五个维度全面评估。
[了解详情](https://skillhub.cn/tutorials#trace-evaluation)

评测基于 AI 自动化检测，结果供参考。

## Evaluation Details

### T · Trust — 4.6 / 5
纯文档型零风险，全中文适配，数据隐私声明完整。T3 因脚本未识别 Capability Boundaries 章节导致基分偏低，经校准后仍有提升空间。

| Sub-item | Score | Commentary |
|----------|-------|------------|
| 安全性扫描 | 5.0 | 无密钥/无脚本/有安全声明（"不收集、存储或传输任何用户数据"） |
| 国内适配性 | 5.0 | 全中文，示例基于国内场景（微信/飞书等 Agent 平台） |
| 边界透明度 | 3.3 | 脚本未识别 Capability Boundaries 章节（✅8项/⚠️4项/❌4项），实际边界清晰 → 校准 +0.3 |
| 数据隐私规范 | 5.0 | Data Privacy 独立章节，声明"不收集、存储或传输任何用户数据" |

### R · Reliability — 4.4 / 5
异常处理有 Gotchas 7 条覆盖核心陷阱，Quick Fixes 表格实用。降级兜底因边界检测问题基分偏低。

| Sub-item | Score | Commentary |
|----------|-------|------------|
| 异常处理 | 4.5 | Gotchas 7 条 + Quick Fixes 表格 + validation 标记，但缺交互式引导模板 |
| 功能完善性 | 4.8 | Workflow 5 步骤覆盖完整，CLI 型 1 个 cli_section |
| 运行稳定性 | 4.5 | Gotchas + validation，但无 rules 约束章节 |
| 降级兜底 | 3.8 | 脚本未识别边界章节 → 实际 ❌ 范围每项有替代方案引导 → 校准 +0.3 |

### A · Adaptability — 4.2 / 5
场景化路由清晰，description 实际 200+ 字符但脚本解析 YAML 多行格式异常导致基分偏低。

| Sub-item | Score | Commentary |
|----------|-------|------------|
| 能力边界定义 | 4.3 | 实际有三分类边界 + When to Use 章节 → 校准 +0.3 |
| 触发方式 | 3.8 | description 实际 200+ 字符含场景路由 → 脚本解析失败 → 校准 +0.3 |
| 受众广度 | 4.3 | 全中文 + Audience 表格（新用户/高级用户/CI 维护者） |
| 定制化支持 | 4.5 | CLI 型天然有参数定制 + CODEGRAPH_* 环境变量说明 |

### C · Convention — 4.45 / 5
三层结构清晰（Quick Start → 功能详情 → references/），Gotchas 7 条 + FAQ 6 题。

| Sub-item | Score | Commentary |
|----------|-------|------------|
| 文档质量 | 3.8 | examples/ 单文件含 7 个工作流示例可直接复制 → 脚本按文件计数 → 校准 +0.3 |
| 渐进式披露 | 4.7 | CLI 型，5 个 references 文件，body 296 行，trigger_hints=True |
| 结构清晰 | 4.5 | name 规范，references/ 含 5 文件但无子目录 → 校准 -0.2 |
| 反模式与FAQ | 4.8 | Gotchas 7 条覆盖核心陷阱 + FAQ 6 题非充数 |

### E · Effectiveness — 4.3 / 5
Workflow + 工具选择矩阵确保不同场景有明确执行路径。examples 单文件含 7 个完整工作流但脚本按文件计数。

| Sub-item | Score | Commentary |
|----------|-------|------------|
| 输出准确性 | 4.8 | CLI 型 + Workflow + validation，工具选择矩阵区分场景 |
| 内容完整度 | 3.3 | 单文件含 7 个 CLI 工作流示例 → 脚本按文件计数 → 校准 +0.3 |
| 创造力与增值 | 4.2 | 5 个 reference 文件覆盖工具/CLI/安装/语言/排障，有深度 → 校准 +0.2 |
| 开箱即用度 | 4.8 | Quick Start 三步上手 + 5 个可复制开场白 |

## Improvement Suggestions (prioritized)

1. **P1: 拆分 examples/ 为多个文件** — 将 7 个工作流示例拆分为独立文件（如 `architecture.md`、`call-chain.md`、`impact-analysis.md` 等），CLI 型阈值为 4 个 examples 文件，当前仅 1 个。C1/E2 可提升至 5.0。
2. **P1: 为 references/ 添加子目录** — 如 `references/mcp/`、`references/cli/`、`references/setup/` 等，CLI 型 threshold 为 ≥2 个子目录。C3 可提升至 5.0。
3. **P2: 添加交互式引导模板** — 在 Gotchas 或 Workflow 中添加"信息不足时先给假设版本 + 列具体缺少项"的模板。R1 可提升至 5.0。
4. **P2: 修复 YAML frontmatter description 解析** — 当前 `description: >` 格式导致脚本解析为长度 1。建议改为单行 `description: "..."` 或确保脚本正确解析多行 YAML。A2 可提升至 5.0。
5. **P3: 扩充 FAQ 至 8-10 题** — 当前 6 题覆盖基础问题，建议增加边缘场景（如 monorepo 多语言项目、大型代码库性能、与 CI/CD 集成最佳实践等）。

## Skill 基础画像

| 指标 | 值 |
|------|------|
| 技能名称 | codegraph |
| 技能类型 | CLI |
| SKILL.md 行数 | 305 |
| SKILL.md 字符数 | 8,869 |
| references/ 文件数 | 5 |
| references/ 子目录数 | 0 |
| examples/ 文件数 | 1（含 7 个工作流） |
| Gotchas 条数 | 7 |
| FAQ 题数 | 6 |
| 有 Workflow | ✅（5 步） |
| 有 Validation | ✅ |
| 有 Rules | ❌ |
| 有安全声明 | ✅ |
| 有数据隐私 | ✅ |
| 有 When to Use | ✅ |
| 有 Boundary | ✅（脚本未识别） |
| 有 Trigger Hints | ✅ |
| 中文内容 | ✅ |
| 脚本文件 | 0 |
| Secrets 检测 | 无 |

---

> 评测时间：2026-06-16
> 评测脚本：trace_evaluate.py
> 校准规则：calibration-guide.md（±0.3 调幅）
