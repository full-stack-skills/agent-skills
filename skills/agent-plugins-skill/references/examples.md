# 插件包示例与反例

以下是根据 [Agent Plugins 1.0.0 规范](https://agent-plugins.org/specification)编写的原创示例。`../examples/` 下有可复制的最小技能包、远程 MCP 包和扩展包；配置片段展示结构与语义，示例 URL/命令不是已部署服务。

## A. 最小可移植包

```text
minimal-plugin/
├── plugin.json
└── skills/
    └── greet/
        └── SKILL.md
```

见 [plugin.json](../examples/minimal-plugin/plugin.json) 与 [SKILL.md](../examples/minimal-plugin/skills/greet/SKILL.md)。根 manifest 只有 canonical `$schema` 与合法 `name` 就足够；技能目录是即时子目录。没有 MCP 需求时，无须建立 `mcp.json`。

## B. 包内 stdio 服务

假设插件**实际捆绑了可执行的** `bin/validator`，并需要持久数据：

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/mcp.schema.json",
  "mcpServers": {
    "validator": {
      "type": "stdio",
      "command": "./bin/validator",
      "args": ["--rules", "${PLUGIN_ROOT}/config/rules.json", "--cache", "${PLUGIN_DATA}/cache"],
      "env": {"CONFIG_PATH": "${PLUGIN_ROOT}/config/rules.json"},
      "cwd": "${PLUGIN_DATA}"
    }
  }
}
```

客户端先创建 `PLUGIN_DATA`，校验 `command` 和 `cwd` 的真实路径，并将参数作为数组传递。包内命令仍需真实存在、可执行并遵循 MCP stdio 协议；JSON 合法不等于握手成功。

## C. 远程 MCP 与旧 SSE

完整文件见 [远程插件 manifest](../examples/remote-mcp-plugin/plugin.json) 与 [远程 MCP 配置](../examples/remote-mcp-plugin/mcp.json)。其中 `mcp.example.org` 是演示地址，静态结构有效，不代表连接可成功。以下再展示同时声明旧 SSE 服务的写法：

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/mcp.schema.json",
  "mcpServers": {
    "remote-api": {
      "type": "streamable-http",
      "url": "https://mcp.example.org/api",
      "headers": {"X-Public-Tenant": "demo"}
    },
    "legacy-api": {
      "type": "sse",
      "url": "https://legacy.example.org/events"
    }
  }
}
```

`sse` 是旧 HTTP+SSE 传输；客户端可以不支持。认证由客户端管理，不能把 token 写入上述静态 headers。Streamable HTTP 内部出现 SSE 响应，不会把 `type` 变成 `sse`。

## D. 客户端扩展

```text
extension-plugin/
├── plugin.json
├── skills/
│   └── review/
│       └── SKILL.md
└── org.example.editor/
    └── hooks/
        └── hooks.json
```

若 manifest 还需要配置，写入 `"extensions": {"org.example.editor": {...}}`。扩展目录和 manifest 键可以分别存在；没有实现该命名空间的客户端忽略它。`hooks` 在这里是该客户端的专有定义，并非 Agent Plugins 1.0.0 的第三种可移植组件。

另见 [扩展包 manifest](../examples/extension-plugin/plugin.json)、[扩展数据](../examples/extension-plugin/org.example.editor/settings.json) 和 [技能](../examples/extension-plugin/skills/review/SKILL.md)。`org.example.editor` 的数据字段纯属示意，须由拥有该命名空间的客户端自行定义。

## E. 反例与正确处理

| 反例 | 为什么不符合 | 处理边界 / 修正 |
| --- | --- | --- |
| 只有 `.claude-plugin/plugin.json`，无根 `plugin.json` | 原生清单不能替代可移植根 manifest | 整包不符合；新增合法根 manifest。 |
| 根 manifest 使用 `"skills": ["./skills/review"]` | 顶层字段非规范字段，固定位置不能由 manifest 改写 | 客户端报告并忽略；作者改用 `skills/review/SKILL.md`。 |
| `skills/team/review/SKILL.md` | 不是 `skills/` 的即时子目录 | 不发现该技能；改成 `skills/review/SKILL.md`。 |
| `mcp.json` 把 `mcpServers` 写在 `plugin.json` 中 | MCP 只能从根 `mcp.json` 发现 | 将配置移至独立文件。 |
| stdio `"command": "python server.py"` | 命令被写成 shell 字符串 | 改成 `"command": "python", "args": ["${PLUGIN_ROOT}/server.py"]`；先确认解释器和脚本可用。 |
| stdio `"command": "../server"` | 包相对命令逃出根且未以 `./` 开头 | 跳过该条目；将二进制置于包内并使用 `./bin/server`。 |
| `"cwd": "data"` | 显式目录不符合三种允许形式 | 用 `./data` 或 `${PLUGIN_DATA}`。 |
| `"env": {"PLUGIN_DATA": "/tmp/x"}` | 保留变量不能由包配置覆盖 | 跳过该条目；由客户端提供。 |
| 远程 `"url": "http://example.org/mcp"` | 非 loopback 端点必须 HTTPS | 跳过该条目；使用 HTTPS。 |
| `"headers": {"Authorization": "Bearer secret"}` | 包可见 headers 不能放凭据 | 由客户端完成授权与凭据存储。 |
| `mcp.json` 多出 `"timeout": 30` 顶层字段 | MCP 顶层闭合，无非致命例外 | 禁用 MCP 类型；移至宿主专有配置。 |
| `plugin.json` 多出 `"commands": {}` 顶层字段 | manifest 顶层闭合 | 客户端报告并忽略字段；专有配置移入扩展。 |
| `plugin.json` 的 `extensions` 为数组 | 类型错误，但规范规定非致命 | 报告并忽略该字段，继续有效组件。 |

作者在示例迁移时，要以目标宿主的真实加载证据验证，而不是只凭目录树与 JSON 解析宣布兼容。
