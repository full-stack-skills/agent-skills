# MCP 配置、变量与运行

来源：[正式规范 §7.2、§9](https://agent-plugins.org/specification)、[插件作者：MCP servers](https://agent-plugins.org/plugin-authors/mcp-servers)、[客户端：MCP runtime](https://agent-plugins.org/client-implementers/mcp-runtime)、[mcp.schema.json](https://agent-plugins.org/schemas/1.0.0/mcp.schema.json)。MCP 线上协议、握手和生命周期仍以 [MCP 规范](https://modelcontextprotocol.io/specification)为准。

## 配置文档与逐项失败边界

`mcp.json` 只能放在插件根目录。它必须是 JSON 对象，顶层**恰好只允许** `$schema` 与 `mcpServers`，二者必需；后者是以服务名为键、服务配置为值的对象，可以为空。1.0.0 的 `$schema` 必须是 `https://agent-plugins.org/schemas/1.0.0/mcp.schema.json`，并与根 `plugin.json` 声明同一规范版本。服务条目必须匹配下面一个闭合类型；混用别的传输字段、未知字段或未知 `type` 都只使**该条目**无效。

若文档不是合法 JSON、缺必需顶层字段、有额外顶层字段、Schema 不支持或与 manifest 版本不匹配，禁用此插件的整个 MCP 组件，其他组件继续加载。若某条目无效、声明的传输不受支持，或启动/连接/认证/握手失败，只跳过该条目，继续其他条目与技能；可行时报告诊断。官方 Schema 的 `#/$defs/server` 可用于逐条目验证。客户端不得在加载插件时联网获取 Schema。

## 传输类型

| `type` | 必需 | 可选 | 规则 |
| --- | --- | --- | --- |
| `stdio` | `type`, `command` | `args`, `env`, `cwd` | 启动本地可执行文件；客户端用单独参数数组，不把命令当 shell 字符串。 |
| `streamable-http` | `type`, `url` | `headers` | 当前远程 MCP 传输。 |
| `sse` | `type`, `url` | `headers` | 旧版 HTTP+SSE 传输；客户端支持可选，区别于 Streamable HTTP 内部可能出现的 SSE 响应。 |

实现 MCP 的客户端至少支持 `stdio` 或 `streamable-http` 之一，建议都支持。初次连接必须使用 `type` 声明的传输；Agent Plugins 本身没有定义失败后自动降级。客户端自身的 MCP 向后兼容策略需要单独说明。

## stdio 命令、目录与环境

`command` 是**一个可执行 token**：要么是按平台搜索规则解析的裸名称（如 `node`），要么是以 `./` 开头且真实路径被插件根包含的包内可执行文件。不能写成 `node server.js`、绝对路径、`../bin/server`；也不能在 `command` 中展开 `${PLUGIN_ROOT}`。捆绑在插件里的可执行文件要用 `./` 路径。Windows 必须借助 `.bat`/`.cmd` 解释器时，仍保持原 `command` 为一个 token，参数分别传递。插件不能依赖配置中的 `PATH` 必然参与裸命令查找，因为该行为由客户端决定。

省略 `cwd` 时默认真实插件根目录。显式 `cwd` 仅能为：以 `./` 开头的包内路径、恰好 `${PLUGIN_ROOT}` 或其 `/` 子路径、恰好 `${PLUGIN_DATA}` 或其 `/` 子路径。先展开占位符再解析真实路径；前两类不得逃出插件根，第三类不得逃出插件数据目录。其他形式或越界使该服务条目无效。

客户端为每个 stdio 进程提供：

- `PLUGIN_ROOT`：真实插件根的绝对路径。
- `PLUGIN_DATA`：客户端管理的该安装实例专用数据目录的绝对路径；启动前创建、可写，跨插件更新保留；卸载时可以删除。用于依赖、虚拟环境、生成物和缓存。包内代码与静态配置放在插件根。

客户端选择基础环境，按平台环境变量名称语义用 `env` 覆盖它，最后覆盖写入保留的 `PLUGIN_ROOT`、`PLUGIN_DATA`。配置的 `env` **不得定义**这两个保留名称，否则该服务条目无效。除标准要求的变量及显式配置变量外，插件不能依赖客户端可能继承的其他环境变量（裸命令的平台搜索例外）。

占位符展开只处理 `${PLUGIN_ROOT}` 与 `${PLUGIN_DATA}`：仅用于 `args` 的各字符串、`env` 的各**值**、`cwd`；每个精确出现位置做单次文本替换，不递归扫描替换结果。其他类似占位符保留字面值。`command`、`env` 键、固定组件路径、远程 `url`、HTTP headers 不展开，也不执行任意系统环境变量替换。配置中的 `env` 是包可见数据，不能放密码或 token。

## HTTP URL 与 headers

`url` 必须为绝对 HTTP/HTTPS URL，不能含用户信息或 fragment。非 loopback 端点必须用 HTTPS；HTTP 仅允许主机**恰好是** `localhost` 或 loopback 范围 IP 字面量。`headers` 是字符串映射，名称和值要满足 HTTP 头字段规则；按不区分大小写比较名称，同一名称用不同大小写重复出现时条目无效。

URL、头名称、头值均不做占位符或环境变量展开。头值是包可见数据，不能嵌入凭据或其他秘密。客户端为 HTTP、MCP 或授权生成的头与配置冲突时，客户端生成的头优先。遇到重定向或旧 SSE 的 endpoint event，未经用户明确授权，不得将配置头转发到不同 origin。

Agent Plugins v1 没有可移植 OAuth 或凭据引用字段。授权发现、用户交互和凭据存储是客户端职责；授权失败属于该服务的**连接失败**，不是配置格式无效。

## 作者与客户端检查顺序

1. 验证 `mcp.json` 顶层 JSON、字段闭合、目标 Schema 与 manifest 同版。
2. 对每个服务独立验证所声明传输的闭合字段和类型；不让一个坏服务拖垮其他服务。
3. 对 stdio 做命令 token、真实路径、`cwd`、保留 env、占位符语义检查；对 HTTP 做 URL、头、秘密与跨 origin 规则检查。
4. 客户端启动或连接后按 MCP 规范完成握手；分别记录静态通过、运行成功和失败原因。
