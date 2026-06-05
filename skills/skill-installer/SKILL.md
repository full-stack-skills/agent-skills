---
name: skill-installer
description: Manages the installation and discovery of AI skills from the PartMe marketplace.
---

# Skill Installer

## Overview

The `skill-installer` is a core utility skill that allows users and agents to discover, install, and manage other AI skills within the PartMe ecosystem. It serves as the local package manager for the "Knowledge as a Service" (KaaS) and "Tool as a Service" (TaaS) architecture.

## Features

- **Search Skills**: Query the local marketplace for available skills by keyword.
- **Install Skills**: Register skills into the local environment.
- **List Installed Skills**: View currently active skills.

## Usage

### Search for a skill
```
skill-installer(action="search_skills", query="vue")
```

### Install a skill
```
skill-installer(action="install_skill", skill_path="./skills/vue3")
```

## Tools

### `search_skills`
- **Input**: `query` (string)
- **Output**: JSON list of matching skills with descriptions and paths.

### `install_skill`
- **Input**: `skill_path` (string)
- **Output**: Confirmation of installation.

### `list_installed_skills`
- **Input**: None
- **Output**: JSON list of installed skills.

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
