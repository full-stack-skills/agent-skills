# 包模型、manifest 与版本（Agent Plugins 1.0.0）

来源：[正式规范 §4–6、§10](https://agent-plugins.org/specification)、[作者版 manifest 指南](https://agent-plugins.org/plugin-authors/manifest)、[plugin.schema.json](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json)。这里用中文重述要求；规范正文优先于 Schema。

## 1. 包边界与固定位置

插件是一个目录，不是规范定义的压缩包或市场条目。根目录必须有且只有一个承担可移植核心语义的 `plugin.json`。可选组件位于固定路径：`skills/` 与根目录 `mcp.json`。`plugin.json` 不能改变发现路径，也不能内嵌技能或 MCP 配置。缺少可选路径是正常情况；路径存在但解析后不是预期目录/普通文件时，仅对应组件类型失效。

客户端读取、发现或执行包提供的路径时，先以文件系统真实路径解析，再确认仍在真实插件根目录内。软链接、Windows junction/reparse point 允许指向包内，不能指向包外。若配置字段在规范中定义为“插件相对路径”，必须以 `./` 开头、相对根目录解析且仍被根目录包含。此规则不把任意 `args`、`env` 字符串强行解释为包路径，也不等于隔离子进程或限制其运行时文件访问。

路径失败要落在最窄边界：根 manifest 逃逸则拒绝整包；固定组件路径逃逸则禁用该组件类型；某 `SKILL.md` 逃逸则跳过该技能；MCP `command`/`cwd` 逃逸则跳过该服务条目；其他包路径逃逸则拒绝访问该路径。

## 2. `plugin.json` 的闭合字段

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "acme.tools",
  "version": "1.2.0",
  "description": "Reusable tooling for Acme workflows",
  "author": {"name": "Acme", "email": "team@example.org", "url": "https://example.org"},
  "homepage": "https://example.org/docs",
  "repository": "https://example.org/source",
  "license": "Apache-2.0",
  "keywords": ["tools", "agents"],
  "extensions": {"org.example.client": {"enabledByDefault": false}}
}
```

| 字段 | 规则 |
| --- | --- |
| `$schema` | 必需，1.0.0 时必须精确等于上面的 canonical 标识；客户端按本地已支持标识选规则，加载时不得在线获取 Schema。 |
| `name` | 必需的合法插件名；规则见下节。 |
| `version`、`description`、`homepage`、`repository`、`license` | 可选字符串。推荐 SemVer 与 SPDX 标识，但格式不标准本身不是拒绝理由。 |
| `author` | 可选对象，仅允许 `name`、`email`、`url` 三个可选字符串。邮箱/URL 语法不标准本身不是拒绝理由。 |
| `keywords` | 可选字符串数组。 |
| `extensions` | 可选对象，键为反向域名命名空间，值为对象。细节见 [skills-and-extensions.md](skills-and-extensions.md)。 |

`plugin.json` 顶层不允许其他字段。**例外处理很重要**：未知顶层字段仍是 Schema 违规，但客户端应报告并忽略该字段，若其余部分有效则继续加载；`extensions` 不是对象时报告并忽略整个字段，也继续加载。其他类型错误、缺失必需字段、无效名称、未知规范版本等均是致命 manifest 错误，必须拒绝整个包，且不能发现或执行其组件。客户端不对不支持的扩展命名空间内容做校验。

## 3. 名称约束

`name` 长度为 1–64 字符，仅用小写 ASCII 字母 `a-z`、数字 `0-9`、连字符 `-`、点 `.`；首尾必须是字母或数字；不允许连续 `--` 或 `..`。例如 `acme.tools`、`lint3r` 有效；`Acme.Tools`、`-demo`、`two--parts`、`a..b` 无效。不要把 Agent Skills 技能名的约束误用为插件名约束：插件名允许点。

## 4. 版本关系

- 规范发布版本、manifest Schema 版本、MCP Schema 版本组成同一个 Agent Plugins 版本。插件的 `plugin.json` 声明目标规范版本；若有 `mcp.json`，两者 `$schema` 版本必须一致。MCP 版本不匹配只禁用 MCP 类型，不使独立技能失效。
- 公开发布过的 canonical Schema 标识不能改指向不同内容；Schema 修改必须产生新的规范版本。旧版插件可以继续存在，客户端是否支持由其本地规则或明确声明的兼容映射决定。
- 插件自身的 `version` 是可选元数据，建议 SemVer。主、次、修订版本分别表达破坏性变化、兼容功能和兼容修复；客户端可将其用于更新与缓存判断，但不能因其不符合 SemVer 而拒绝插件。

## 5. 作者自查

1. 根 `plugin.json` 存在且为对象；`$schema`、`name` 正确；字段类型正确。
2. 额外平台字段移至 `extensions`，不将 Claude/Codex 等原生 manifest 直接冒充可移植 `plugin.json`。
3. 可选组件位置固定；缺少可选组件无需填充空文件。
4. 对真实路径做包含检查，尤其检查符号链接、`../`、Windows 等价路径。
5. 使用官方 Schema 做结构检查后，再做真实路径、版本匹配、URL 等语义检查；JSON Schema 本身不能覆盖全部运行规则。
