---
name: agent-browser
description: A comprehensive skill for using agent-browser, a CLI tool for browser automation designed for AI agents, developed by Vercel Labs. This skill covers installation, core commands, selectors (refs, CSS, XPath, semantic locators), agent mode, sessions, options, and best practices. Use this skill whenever the user needs to automate browser interactions via CLI commands, especially for AI agents that need to interact with web pages.
license: Complete terms in LICENSE.txt
---

## When to use this skill

Use this skill whenever the user wants to:
- Automate browser interactions via CLI commands
- Use browser automation for AI agents
- Navigate websites and interact with pages using command-line tools
- Use refs-based element selection for deterministic automation
- Integrate browser automation into AI agent workflows
- Capture snapshots of web pages with accessibility trees
- Fill forms, click elements, and extract content via CLI
- Use semantic locators for more reliable element selection
- Work with browser automation in agent mode with JSON output
- Manage multiple browser sessions
- Debug browser automation with headed mode
- Use authenticated sessions with custom headers
- Connect to existing browsers via CDP
- Stream browser viewport for live preview

## How to use this skill

This skill is organized to match the agent-browser official documentation structure (https://github.com/vercel-labs/agent-browser/blob/main/README.md). When working with agent-browser:

1. **Install agent-browser**:
   - Load `examples/getting-started/installation.md` for installation instructions

2. **Quick Start**:
   - Load `examples/quick-start/quick-start.md` for basic workflow examples

3. **Learn core commands**:
   - Load `examples/commands/basic-commands.md` for basic commands (open, click, fill, etc.)
   - Load `examples/commands/advanced-commands.md` for advanced commands (snapshot, eval, etc.)
   - Load `examples/commands/get-info/` for information retrieval commands
   - Load `examples/commands/check-state/` for state checking commands
   - Load `examples/commands/find-elements/` for semantic locator commands
   - Load `examples/commands/wait/` for wait commands
   - Load `examples/commands/mouse-control/` for mouse control commands
   - Load `examples/commands/browser-settings/` for browser configuration
   - Load `examples/commands/cookies-storage/` for cookies and storage management
   - Load `examples/commands/network/` for network interception
   - Load `examples/commands/tabs-windows/` for tab and window management
   - Load `examples/commands/frames/` for iframe handling
   - Load `examples/commands/dialogs/` for dialog handling
   - Load `examples/commands/debug/` for debugging commands
   - Load `examples/commands/navigation/` for navigation commands
   - Load `examples/commands/setup/` for setup commands

4. **Understand selectors**:
   - Load `examples/selectors/refs.md` for refs-based selection (@e1, @e2, etc.)
   - Load `examples/selectors/traditional-selectors.md` for CSS, XPath, and semantic locators

5. **Use agent mode**:
   - Load `examples/agent-mode/introduction.md` for agent mode overview
   - Load `examples/agent-mode/optimal-workflow.md` for optimal AI workflow
   - Load `examples/agent-mode/integration.md` for integrating with AI agents

6. **Advanced features**:
   - Load `examples/advanced/sessions.md` for session management
   - Load `examples/advanced/headed-mode.md` for debugging with visible browser
   - Load `examples/advanced/authenticated-sessions.md` for authentication via headers
   - Load `examples/advanced/custom-executable.md` for custom browser executable
   - Load `examples/advanced/cdp-mode.md` for Chrome DevTools Protocol integration
   - Load `examples/advanced/streaming.md` for browser viewport streaming
   - Load `examples/advanced/architecture.md` for architecture overview
   - Load `examples/advanced/platforms.md` for platform support
   - Load `examples/advanced/usage-with-agents.md` for AI agent integration patterns

7. **Configure options**:
   - Load `examples/options/global-options.md` for global CLI options
   - Load `examples/options/snapshot-options.md` for snapshot-specific options
   - Load `examples/options/session-options.md` for session management options

8. **Reference API documentation** when needed:
   - `api/commands.md` - Complete command reference
   - `api/selectors.md` - Selector reference
   - `api/options.md` - Options reference

9. **Use templates** for quick start:
   - `templates/basic-automation.md` - Basic automation workflow
   - `templates/ai-agent-workflow.md` - AI agent workflow template


### Doc mapping (one-to-one with official documentation)

- See examples and API files → https://github.com/vercel-labs/agent-browser

## Examples and Templates

This skill includes detailed examples organized to match the official documentation structure. All examples are in the `examples/` directory (see mapping above).

**To use examples:**
- Identify the topic from the user's request
- Load the appropriate example file from the mapping above
- Follow the instructions, syntax, and best practices in that file
- Adapt the code examples to your specific use case

**To use templates:**
- Reference templates in `templates/` directory for common scaffolding
- Adapt templates to your specific needs and coding style

## API Reference

- **Commands API**: `api/commands.md` - Complete command reference with syntax and examples
- **Selectors API**: `api/selectors.md` - Selector types and usage reference
- **Options API**: `api/options.md` - All options reference

## Best Practices

1. **Use Refs**: Prefer refs (@e1, @e2) over traditional selectors for deterministic automation
2. **Snapshot First**: Always snapshot before interacting with elements to get refs
3. **Agent Mode**: Use `--json` flag for machine-readable output in agent mode
4. **Session Management**: Use `--session` to maintain state across commands
5. **Interactive Snapshot**: Use `-i` flag for interactive snapshot selection
6. **Semantic Locators**: Use semantic locators (role/name) when refs are not available
7. **Error Handling**: Check command exit codes and error messages
8. **Wait for Navigation**: Commands automatically wait for navigation to complete
9. **Headed Mode**: Use `--headed` for debugging, headless for production
10. **CDP Integration**: Use `--cdp` for Chrome DevTools Protocol integration
11. **Streaming**: Use `AGENT_BROWSER_STREAM_PORT` for live browser preview
12. **Authenticated Sessions**: Use `--headers` for authentication without login flows
13. **Custom Executable**: Use `--executable-path` for serverless deployments or custom browsers
14. **Snapshot Options**: Combine `-i`, `-c`, `-d`, `-s` options to optimize snapshot output

## Resources

- **GitHub Repository**: https://github.com/vercel-labs/agent-browser
- **Official README**: https://github.com/vercel-labs/agent-browser/blob/main/README.md
- **Agent Mode Documentation**: https://agent-browser.dev/agent-mode
- **Issues**: https://github.com/vercel-labs/agent-browser/issues

## Keywords

agent-browser, CLI browser automation, AI agents, browser automation CLI, refs, snapshot, agent mode, semantic locators, browser automation tool, command-line browser, AI agent browser, deterministic selectors, accessibility tree, browser commands, web automation CLI, sessions, headed mode, authenticated sessions, CDP mode, streaming, Chrome DevTools Protocol, Playwright, browser automation for AI

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

## 使用流程

### Step 1: 环境准备
确保开发环境已安装必要的依赖和工具。

### Step 2: 配置初始化
根据项目需求进行基础配置。

### Step 3: 核心功能使用
按照示例代码实现核心功能。

### Step 4: 测试验证
运行测试确保功能正常。

### Step 5: 部署上线
完成开发后进行部署和监控。
