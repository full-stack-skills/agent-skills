---
name: agent-plugins-skill
description: 基于 Agent Plugins 1.0.0 规范设计、创建、迁移或审查可移植智能体插件（plugin.json、Agent Skills、mcp.json、客户端扩展）；实现插件加载器或做兼容性评测时也使用。
license: Apache-2.0
---

# Agent Plugins 插件开发

本技能以 [Agent Plugins 1.0.0 正式规范](https://agent-plugins.org/specification)为事实源，帮助智能体产出可移植插件，或实现符合规范的客户端。下列参考资料是中文释义和原创示例；出现分歧时，以正式规范正文为准，再参考[官方 JSON Schemas](https://agent-plugins.org/schemas)。不要把本技能的版本 `agent-plugins-skill`、插件自身 `version` 与 Agent Plugins 规范版本混为一谈。

## 何时使用

- **插件作者**：新建插件；将现有 Claude/Codex/Cursor 等专有包改造成可移植包；添加技能、MCP 服务器或客户端专有扩展。
- **客户端实现者**：实现目录加载、校验、发现、MCP 连接、路径安全和故障隔离；制定一致性测试。
- **审查者**：对某个真实插件或客户端逐条核对 1.0.0 要求，并给出具体修复。仅有 `SKILL.md`、某客户端的 `plugin.json` 或 MCP 配置，不能据此声称 Agent Plugins 兼容。

若用户只要求 Agent Skills 格式或普通 MCP 服务端开发，使用相应专门技能；这里处理二者在 **Agent Plugin 包** 中的组合与加载契约。

## 快速开始

在本技能目录下复制 `examples/minimal-plugin/` 到新插件目录，按 [包与 manifest](references/package-and-manifest.md)修改合法 `name`，再把真实技能放进 `skills/<名称>/SKILL.md`。需要 MCP 时按 [MCP 配置与运行](references/mcp-servers.md)新增根 `mcp.json`。先做静态校验，再在目标客户端加载；演示用远程 URL 不能当作可用服务。

## 首先确定任务

1. 确认目标目录、是**作者**、**客户端**还是**审查**任务，以及目标规范版本。用户未指定版本时以 1.0.0 为基线；需要最新版本时重新核对官网。
2. 对已有项目，先查看工作树、现有包布局、宿主专有清单和运行方式；保留已有非目标改动。识别可移植内容和必须留在宿主扩展中的内容。
3. 选择下方相应参考文件，只读所需章节。规范中的 **MUST/MUST NOT** 是一致性要求，**SHOULD/RECOMMENDED** 是建议，**MAY** 是允许；不要把建议升级成硬性失败。
4. 交付时明确区分：静态结构与 Schema 检查、客户端加载测试、实际 MCP 握手/工具调用、目标宿主安装验证。一个层面的成功不能替代另一个层面。

## 插件作者工作流

1. 读 [包与 manifest](references/package-and-manifest.md)：根目录创建唯一的 `plugin.json`，含 canonical `$schema` 和合法 `name`。元数据按字段类型填写，客户端专有数据进入 `extensions`。
2. 按需读 [技能与扩展](references/skills-and-extensions.md)：在 `skills/<skill-name>/SKILL.md` 放符合 Agent Skills 规范的技能；客户端专有文件放反向域名顶层目录。
3. 如需 MCP，读 [MCP 配置与运行](references/mcp-servers.md)：在根目录放 `mcp.json`；逐项决定 `stdio`、`streamable-http` 或兼容旧服务的 `sse`，处理路径、变量和凭据边界。
4. 从 [完整示例与反例](references/examples.md) 选最接近的例子改写；`examples/minimal-plugin/` 提供可复制的最小技能包。示例中的服务地址和命令仅用于演示，运行前必须替换为真实资源。
5. 检查 JSON 与官方 Schema，再按参考文件的语义清单做路径、URL、占位符、版本、扩展和敏感信息检查。尽可能在目标客户端做加载与实际调用；记录未验证项。

### 作者交付清单

- 包根目录与 `plugin.json` 的目标版本、合法名称、字段类型均已核对。
- `skills/`、`mcp.json` 是可选组件；若存在，固定位置与内容有效。没有在 manifest 中内嵌技能或 MCP 配置。
- 包内被访问的文件经真实路径解析仍在包根；配置中的包相对路径按规则以 `./` 起始。
- 没有将密钥放入可见的 `env` 或 `headers`；客户端负责授权发现、交互与凭据存储。
- 客户端专有能力明确标为扩展；安装、发布、权限和沙箱策略没有被误称为可移植核心功能。

## 客户端实现工作流

读 [加载器与一致性](references/client-implementation.md)，按顺序实现：目录根与真实路径边界 → 先校验 `plugin.json` → 发现支持的固定组件 → 各级故障隔离 → 可选的扩展处理。支持 MCP 时再读 [MCP 配置与运行](references/mcp-servers.md) 的运行段落。至少实现技能或 MCP 两类组件中的一类；MCP 客户端至少支持 `stdio` 或 `streamable-http` 之一。

用 [一致性测试矩阵](references/client-implementation.md#可执行的一致性测试矩阵)覆盖可观察结果，特别是未知 manifest 顶层字段、非对象 `extensions`、错误组件种类、单个坏技能、单个坏 MCP 条目、启动失败和路径逃逸的**不同故障边界**。客户端运行时不得在线抓取 Schema；本地实现按所识别的 canonical `$schema` 选择规则。

## 审查输出

按下表逐项给出 `通过 / 不符合 / 未验证 / 不适用`、文件或运行证据、对应规范小节及最小修复。对客户端列出支持的组件和传输；对插件列出目标客户端的实测结果。可以参考 [兼容客户端与来源](references/sources-and-compatibility.md)，但该名单随时间变化，使用前重新打开官方页面。

| 维度 | 重点 |
| --- | --- |
| 包 | 单一根目录、路径包含、根 `plugin.json`、版本 |
| manifest | 闭合字段、必需字段、名称、非致命例外、扩展命名空间 |
| 发现 | `skills/` 即时子目录、根 `mcp.json`、缺失与错误类型 |
| MCP | Schema 同版、逐项类型、命令/URL/headers、变量、隔离 |
| 客户端 | 至少一类组件、加载顺序、局部故障继续、真实运行证据 |

## 常见故障的定位

- **宿主能安装、标准客户端不发现**：先看根 `plugin.json` 是否存在、`$schema` 是否为受支持的 Agent Plugins 版本，再看固定组件位置；宿主原生 manifest 不能代替它。
- **一个坏配置让整个包消失**：检查客户端是否误将未知 manifest 顶层字段或非对象 `extensions` 当致命错误；这两种情况应报告并忽略。其他 manifest Schema 错误才拒绝整包。
- **MCP 失败导致技能也消失**：区分 `mcp.json` 顶层无效、单个服务无效和连接/握手失败，分别在 MCP 类型或服务条目边界处理。
- **本地 MCP 命令找不到**：检查 `command` 是否单 token、包内路径是否 `./` 开头、可执行文件是否实际打包，`cwd` 与 `${PLUGIN_DATA}` 是否符合规则。
- **远程 MCP 静态通过却无法调用**：分开记录 URL/headers 规则、目标客户端传输支持、授权和真实 MCP 握手；不要将 Schema 通过当作运行成功。

## 资料导航

| 任务 | 读取 |
| --- | --- |
| 包布局、manifest、名称、版本、Schema | [package-and-manifest.md](references/package-and-manifest.md) |
| 技能发现与客户端扩展 | [skills-and-extensions.md](references/skills-and-extensions.md) |
| MCP 配置、环境变量与运行 | [mcp-servers.md](references/mcp-servers.md) |
| 客户端加载、失败边界、测试矩阵 | [client-implementation.md](references/client-implementation.md) |
| 可复制结构、有效/无效配置 | [examples.md](references/examples.md) |
| 官网各页、Schema、兼容客户端 | [sources-and-compatibility.md](references/sources-and-compatibility.md) |

**版本提示：**上述释义核对于 2026-09-28。制作跨版本包或声称某客户端当前支持情况前，重新核对官网和目标客户端文档。
