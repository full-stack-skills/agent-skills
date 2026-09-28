# 官方来源、适用范围与客户端支持

此资料于 **2026-09-28** 对照官网整理。规范版本为 1.0.0、状态为 Published。以下客户端支持信息会变化，作兼容性声明或发布前必须重新核对[官网名单](https://agent-plugins.org/compatible-clients)及目标客户端的安装说明。名单只说明其文档列出的组件与 MCP 传输，不证明某个具体插件已在该客户端成功安装或运行。

## 规范与 Schema

| 来源 | 用途 |
| --- | --- |
| [Agent Plugins 首页](https://agent-plugins.org/) | 标准目标、包结构与作者/客户端入口。 |
| [Agent Plugins 1.0.0 正式规范](https://agent-plugins.org/specification) | 唯一规范性依据；§1–11 定义包、manifest、发现、组件、扩展、变量、版本和客户端一致性。附录与设计理由为非规范性说明。 |
| [JSON Schemas 索引](https://agent-plugins.org/schemas) | 官方机器可读 Schema 入口；规范正文与 Schema 冲突时以正文为准。 |
| [plugin.schema.json](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json) | 根 `plugin.json` 的结构校验，JSON Schema 2020-12。 |
| [mcp.schema.json](https://agent-plugins.org/schemas/1.0.0/mcp.schema.json) | 根 `mcp.json` 与 `#/$defs/server` 逐服务结构校验，JSON Schema 2020-12。 |
| [Agent Skills 规范](https://agentskills.io/specification) | `SKILL.md` 格式、frontmatter 与资源目录的事实源。 |
| [Model Context Protocol 规范](https://modelcontextprotocol.io/specification) | MCP 消息、传输、握手、授权与生命周期事实源。 |

## 插件作者页面

| 页面 | 对应本技能 |
| --- | --- |
| [Build an Agent Plugin](https://agent-plugins.org/plugin-authors/build-an-agent-plugin) | 最小包、固定目录与包边界；见 [examples.md](examples.md)。 |
| [Plugin manifest](https://agent-plugins.org/plugin-authors/manifest) | 字段、命名、未知字段；见 [package-and-manifest.md](package-and-manifest.md)。 |
| [Skills](https://agent-plugins.org/plugin-authors/skills) | 即时子目录与无效技能隔离；见 [skills-and-extensions.md](skills-and-extensions.md)。 |
| [MCP servers](https://agent-plugins.org/plugin-authors/mcp-servers) | MCP 配置、传输、变量、安全与失败；见 [mcp-servers.md](mcp-servers.md)。 |
| [Client extensions](https://agent-plugins.org/plugin-authors/client-extensions) | 反向域名 manifest/目录；见 [skills-and-extensions.md](skills-and-extensions.md)。 |

## 客户端实现页面

| 页面 | 对应本技能 |
| --- | --- |
| [Implement an Agent Plugins client](https://agent-plugins.org/client-implementers/implement-an-agent-plugins-client) | 最小实现与职责边界；见 [client-implementation.md](client-implementation.md)。 |
| [Loading and discovery](https://agent-plugins.org/client-implementers/loading-and-discovery) | 加载顺序、固定位置与路径失败；见 [client-implementation.md](client-implementation.md)。 |
| [MCP runtime](https://agent-plugins.org/client-implementers/mcp-runtime) | 各传输运行映射；见 [mcp-servers.md](mcp-servers.md)。 |
| [Client conformance checklist](https://agent-plugins.org/client-implementers/conformance) | 非规范性核对表；见 [client-implementation.md](client-implementation.md) 的测试矩阵。 |

## 兼容客户端名单快照

官网在上述日期列出下列客户端。全部列出 Agent Skills；MCP 传输支持按其页面原样分组。这里不推断版本、启用方式、市场分发或运行质量。

| 客户端 | 官网列出的 MCP 传输 |
| --- | --- |
| VS Code、Cursor、GitHub Copilot、Kiro、OpenClaw、Grok Bot | stdio、Streamable HTTP、旧 SSE |
| ChatGPT & Codex、Hermes Agent、NanoClaw、OpenHands | stdio、Streamable HTTP |

客户端能逐步采用组件：只支持技能也可能符合规范；某客户端未列旧 SSE 支持，不代表不支持 Streamable HTTP 中的 SSE 流。实际验证某插件时，从[兼容客户端页面](https://agent-plugins.org/compatible-clients)进入其**安装说明**，分别验证发现、加载和 MCP 运行。

## 重要边界

Agent Plugins 1.0.0 的可移植核心只有 **Agent Skills 与 MCP servers** 两种组件。插件市场、安装/升级、权限提示、信任/沙箱机制、hooks、commands、agents、rules、LSP、客户端 UI 与扩展内部行为都不由该规范统一定义。可按客户端自有格式实现，但不能写成通用合规要求。

本技能是对官网资料的中文释义和操作指南，不复制官网全文。Agent Plugins 文档页面署名 Agent Plugins documentation contributors，并标注 CC BY 4.0；引用时保留上面的来源链接。

## 设计理由（非规范性）

[正式规范的 Design Decisions](https://agent-plugins.org/specification)解释：目录包便于直接检查与版本控制；固定路径避免发现配置的多套优先级；v1 只纳入已有独立规范且跨客户端有实际采用的 Agent Skills 与 MCP；根 manifest 与闭合字段使插件身份和拼写错误可被一致识别；反向域名让客户端独立扩展而无需中心登记。

MCP 配置显式区分传输，避免把 URL 猜成某种协议；允许客户端只支持 stdio 或 Streamable HTTP，是为不同部署与信任模型留出选择。两份 Schema 共用规范版本，避免三条版本线；`PLUGIN_ROOT` 与 `PLUGIN_DATA` 分别锚定只随包发布的文件和跨更新保留的可写状态；单个组件失败不致使独立组件不可用。以上是理解规则的背景，不应当替代正文的 MUST/MUST NOT 要求。
