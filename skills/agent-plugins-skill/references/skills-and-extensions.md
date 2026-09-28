# 技能与客户端扩展

来源：[正式规范 §6、§7.1、§8](https://agent-plugins.org/specification)、[插件作者：Skills](https://agent-plugins.org/plugin-authors/skills)、[插件作者：Client extensions](https://agent-plugins.org/plugin-authors/client-extensions)。

## 技能的发现与验证

插件根目录的 `skills/` 是唯一的可移植技能发现位置。客户端只查看其**即时子目录**；子目录中名为 `SKILL.md` 的路径必须解析成包内的普通文件。不要递归寻找 `skills/group/deploy/SKILL.md`。技能内容和 frontmatter 的真实性由 [Agent Skills 规范](https://agentskills.io/specification)决定，Agent Plugins 只定义放置位置及失败边界。

```text
my-plugin/
├── plugin.json
└── skills/
    └── deploy/
        ├── SKILL.md
        ├── scripts/
        │   └── rollback.sh
        ├── references/
        │   └── runbook.md
        └── assets/
```

`scripts/`、`references/`、`assets/` 是 Agent Skills 常见目录，不是 Agent Plugins 对技能内容的穷尽白名单。`skills/` 缺失时插件仍有效。`skills/` 存在但不是目录时只使技能组件失效。某技能的 `SKILL.md` 无效或解析后逃出包根时，只跳过该技能，继续其余技能及 MCP；适用时报告错误。

作者在技能内引用包中其他文件时，应使包安装后仍可用；不要假定另一个独立安装的技能或宿主专有目录一定存在。若跨独立技能交接，清楚写出技能名与获取方式。宿主如何把合法技能呈现给用户或模型，不由 Agent Plugins 统一规定。

## 客户端扩展

1. 可移植 manifest 的专有数据只能放在 `plugin.json` 的 `extensions` 下；键使用反向域名，例如 `org.example.client`，值是对象。不要在根对象加 `hooks`、`commands` 等自造的可移植顶层字段。
2. 专有文件只能放在与命名空间完全同名的**顶层目录**，例如 `org.example.client/hooks/hooks.json`。manifest 对象与目录可以单独存在或同时存在。
3. 客户端应以自己控制的域名命名并稳定维持命名空间。只处理自己实现的命名空间；未知命名空间内容直接忽略，不能因其内部数据不符合本客户端想象的格式而拒绝包。
4. 扩展的发现细则、验证、加载与失败方式由所属客户端定义。扩展能力不属于 Agent Plugins v1 的可移植组件；不要把某一客户端的 hooks/commands/agents/rules/LSP 支持宣传为所有客户端都会加载。

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "example-plugin",
  "extensions": {
    "org.example.client": {"menuLabel": "Example"}
  }
}
```

迁移旧插件时，可保留原生配置以继续服务该宿主，同时新建根 `plugin.json` 和固定组件。不要让原生配置覆盖或补充可移植 manifest 的核心字段；专有能力要明确映射到已实现的扩展命名空间，或留在规范范围外。
