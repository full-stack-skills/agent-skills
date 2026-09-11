---
name: codegraph
license: Apache-2.0
description: CodeGraph 语义代码智能：代码结构查询、符号追踪、变更影响分析、调用链查找。处理"X如何工作""修改X影响什么""谁调用了X"等代码库问题。20+语言，100%本地。
---

# CodeGraph — 语义代码智能

CodeGraph 是基于 [tree-sitter](https://tree-sitter.github.io/) 的本地代码知识图谱工具，为 AI 编码代理（Claude Code、Cursor、Codex、Gemini 等）提供符号关系、调用图和代码结构的即时查询能力。

**核心收益**：平均节省 ~16% 成本、~58% 减少工具调用、~47% 减少 token 消耗、~22% 加速响应。

**100% 本地** — 无数据离开机器，无 API 密钥，无外部服务，仅 SQLite 数据库。

## Capability Boundaries

### ✅ Strong Suits
1. 代码结构查询 — "X 如何工作"、"X 如何到达 Y"
2. 符号搜索与调用链追踪 — 查找函数/类的调用者与被调用者
3. 变更影响分析 — 修改某符号前分析影响范围（blast radius）
4. 框架路由识别 — Django/Express/NestJS/Spring/Gin 等 14 个框架的路由绑定
5. 跨语言桥接 — iOS（Swift↔ObjC）、React Native（JS↔原生）、Expo Modules
6. 自动同步 — 文件监听器 + 防抖自动同步，索引随代码实时更新
7. 全文搜索 — 基于 FTS5 的符号名即时搜索
8. 受影响测试文件追踪 — `codegraph affected` 溯源变更影响的测试文件

### ⚠️ Requirements
1. 项目需运行 `codegraph init -i` 初始化知识图谱索引
2. MCP 服务器需通过 `codegraph install` 配置到目标 Agent
3. 配置后需重启 Agent 才能加载 MCP 服务器
4. 首次索引可能需要几分钟（取决于代码库大小）

### ❌ Out of Scope（附替代方案）
1. 实时调试 / 运行时动态分析 → 使用调试器（如 GDB、Chrome DevTools）
2. 非文件系统项目（如远程仓库未检出）→ 先 clone 到本地
3. 代码编辑/生成 → 使用 Agent 自身的编辑能力
4. CI/CD 流水线管理 → 使用专用 CI 工具

## When to Use This Skill

Use this skill when the agent needs to:
- 理解代码库架构："X 如何工作"、"请求如何到达数据库"
- 追踪符号依赖："谁调用了 X"、"X 调用了谁"
- 分析变更影响："修改 X 会影响什么"
- 查找调用链："从 A 到 B 的完整路径"
- 审查代码结构：获取项目文件结构、符号索引
- 确定受影响的测试文件：`codegraph affected`

## Data Privacy

CodeGraph 不收集、存储或传输任何用户数据。所有索引存储在项目本地的 `.codegraph/` 目录中，SQLite 数据库不会离开你的机器。无 API 密钥，无外部服务。

## How It Works

```
Agent → CodeGraph MCP Server (explore/search/callers/callees/impact/node)
         → SQLite 知识图谱 (符号 · 边 · 文件 · FTS5)
```

1. **Extraction** — tree-sitter 解析源代码为 AST，提取节点和边
2. **Storage** — 存入本地 SQLite 数据库（`.codegraph/codegraph.db`），支持 FTS5
3. **Resolution** — 解析引用关系：调用→定义、导入→源文件、框架路由
4. **Auto-Sync** — 原生 OS 文件事件监听 + 防抖同步（默认 2s），索引实时更新

## Quick Start

```bash
# Step 1: 安装 CLI（无需 Node.js）
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh
# Step 2: 配置 Agent
codegraph install
# Step 3: 初始化项目
cd your-project
codegraph init -i
```

## Workflow

Step 1. **初始化项目** — 确认 `.codegraph/` 存在，若不存在则运行 `codegraph init -i`

Step 2. **选择合适工具** — 根据查询意图选择 MCP 工具（见工具选择矩阵）

Step 3. **执行查询** — 调用 MCP 工具获取结果。**关键约束**：
   - 绝不在不熟悉的 API 或不确定的符号关系上胡编乱造；如果 CodeGraph 返回空结果或无法确认，直接告诉用户"未找到该符号/关系"，不强加虚构信息
   - 信息不足时：先输出基于已有信息的假设版本，再列出具体缺少的 N 项内容，引导用户补充。禁止笼统提示"请提供更多信息"

Step 4. **处理结果** — 将返回的符号源码视为已读，无需再用 grep/read 验证

Step 5. **检查时效性** — 编辑文件后检查响应中的 `⚠️` staleness banner，若有则直接 Read 对应文件

## MCP 工具选择矩阵

| 查询意图 | 推荐工具 | 说明 |
|---------|---------|------|
| "X 如何工作" / 流程追踪 | `codegraph_explore` | **首选**。一次调用返回源码、关系图和影响范围 |
| 查找符号位置 | `codegraph_search` | 按名称搜索 |
| 谁调用了 X | `codegraph_callers` | 查找调用者 |
| X 调用了谁 | `codegraph_callees` | 查找被调用者 |
| 修改 X 的影响 | `codegraph_impact` | 影响范围分析 |
| 获取符号详情 | `codegraph_node` | 完整源码和元数据 |
| 项目文件结构 | `codegraph_files` | 已索引文件列表 |
| 索引健康状态 | `codegraph_status` | 统计和健康度 |

各工具参数详解见 [references/mcp-tools.md](references/mcp-tools.md)

## CLI 命令速查

```bash
codegraph install/uninstall         # 安装/移除 Agent 配置
codegraph init/uninit [path]        # 初始化/移除项目索引
codegraph index/sync [path]         # 索引/增量同步
codegraph status/query/files        # 状态查询
codegraph callers/callees/impact    # 调用链/影响分析
codegraph affected [files...]       # 受影响测试文件
codegraph serve --mcp               # 启动 MCP 服务器
codegraph upgrade [version]         # 升级
```

完整参考见 [references/cli-reference.md](references/cli-reference.md)

## 安装速查

自动安装（推荐）：
```bash
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh
codegraph install
```

手动配置 Claude Code 见 [references/installation.md](references/installation.md)

## Gotchas

1. **先连接 MCP 服务器** — `codegraph install` 仅配置 Agent，不会自动启动 MCP；需重启 Agent 才能加载
2. **`codegraph init -i` 是必需的** — 每个项目首次使用前必须初始化索引，否则所有 MCP 工具失败
3. **防抖窗口（2s）内的数据可能过时** — 编辑后立即查询可能返回旧数据，检查 `⚠️` banner
4. **WSL/Windows 共享检出** — 不要共享同一 `.codegraph/`，使用 `CODEGRAPH_DIR` 区分
5. **MCP 服务器自行交付使用指南** — 无需手动编辑 CLAUDE.md，指南通过 MCP initialize 自动交付
6. **排除目录** — `node_modules`、`dist`、`build` 等默认排除，通过 `.gitignore` 管理
7. **信息不足时不要笼统提问** — 先输出假设版本，再列具体缺少项（见 Workflow Step 3 模板）

## Quick Fixes

| 问题 | 解决 |
|------|------|
| "not initialized" | 运行 `codegraph init -i` |
| 索引慢 | 确认 `node_modules` 等已被排除 |
| `database is locked` | 升级到最新版；检查 Journal 是否为 `wal` |
| 符号缺失 | 等待自动同步或 `codegraph sync`；检查语言是否支持 |

详见 [references/troubleshooting.md](references/troubleshooting.md)

## FAQ

**Q: CodeGraph 和 grep/find 有什么区别？**
A: CodeGraph 构建完整的符号关系图谱，理解调用链、继承关系和框架路由；grep 只能做文本匹配，无法理解代码语义。

**Q: 索引会占用多少空间？**
A: 取决于代码库大小。典型项目（~1000 文件）约几 MB；大型项目（~10k 文件）约几十 MB。

**Q: 支持 monorepo 吗？**
A: 支持。在 monorepo 的各子项目目录中分别运行 `codegraph init -i` 即可。

**Q: 如何更新索引？**
A: MCP 服务器运行时自动同步。手动可运行 `codegraph sync` 增量更新或 `codegraph index --force` 重建。

**Q: 可以离线使用吗？**
A: 完全离线。所有数据存储在本地 SQLite 数据库中，无任何网络请求。

**Q: 如何卸载？**
A: 运行 `codegraph uninstall` 移除所有 Agent 配置；`codegraph uninit` 移除项目索引。

**Q: 我的项目是 Java/Python/Rust，CodeGraph 支持吗？**
A: 支持 20+ 语言，包括 Java、Python、Rust、Go、TypeScript 等。完整列表见 [references/supported-languages.md](references/supported-languages.md)

**Q: 框架路由识别有什么用？**
A: CodeGraph 能自动识别 Django、Express、Spring、Gin 等 14 个 Web 框架的路由绑定，将 URL 路径直接关联到处理器函数，方便追踪请求流转。

**Q: 跨语言桥接覆盖哪些场景？**
A: 支持 Swift↔ObjC 自动桥接、React Native 旧桥/TurboModules/Fabric、Expo Modules、以及原生→JS 事件通道。

**Q: 索引失败或卡住怎么办？**
A: 运行 `codegraph status` 查看状态；检查是否有大文件（>1MB 被跳过）；确认目录不在排除列表中；可尝试 `codegraph index --force` 重建。

## Official Sources

- [CodeGraph 官方文档](https://colbymchenry.github.io/codegraph/)
- [GitHub 仓库](https://github.com/colbymchenry/codegraph)
- [npm 包](https://www.npmjs.com/package/@colbymchenry/codegraph)

## References

- [MCP 工具详解](references/mcp-tools.md)
- [CLI 命令参考](references/cli-reference.md)
- [支持的语言](references/supported-languages.md)
- [安装指南](references/installation.md)
- [故障排除](references/troubleshooting.md)
- [快速入门示例](examples/README.md)
- [官方文档索引](docs/official/official-sources.md)

## Audience

| 用户类型 | 使用方式 |
|---------|---------|
| **新用户** | 通过 Quick Start 三步上手，使用 `codegraph_explore` 解答代码问题 |
| **高级用户** | 组合使用 callers/callees/impact 进行深度架构分析 |
| **CI/维护者** | 使用 `codegraph affected` 追踪变更影响的测试文件 |
