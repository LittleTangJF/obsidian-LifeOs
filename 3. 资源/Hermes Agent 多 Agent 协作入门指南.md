---
tags: [hermes-agent, AI-Agent, 多Agent协作, Kanban]
title: Hermes Agent 多 Agent 协作入门指南
created: 2026-05-31
---

# Hermes Agent 多 Agent 协作入门指南

> Nous Research 出品的开源 AI Agent 框架（175k+ GitHub Stars）
> 参考文章：[我把多 Agent 协作搬进 Hermes Kanban](https://cloud.tencent.com/developer/article/2672168) — 作者：孟健（腾讯前端工程师）

---

## 什么是 Hermes Agent

Hermes Agent 是 Nous Research 开发的 AI Agent 框架，用 Python 编写，核心特点是：

- **多 Agent 协作** — 多个 Agent 通过 Kanban 看板沟通协同，而不是硬编码的对话链
- **多平台接入** — Telegram / Discord / Slack / WeChat / CLI 等 18+ 种渠道
- **模块化架构** — 自由组合 Agent 能力与工具
- **MCP 支持** — 可以连接任意 MCP Server 扩展能力
- **自动调度** — 支持 Cron 定时任务

### 资源链接

| 资源 | 地址 |
|------|------|
| GitHub 主仓库 | https://github.com/NousResearch/hermes-agent |
| 官方文档 | https://hermes-agent.nousresearch.com/docs/ |
| 橙皮书（中文） | https://github.com/alchaincyf/hermes-agent-orange-book |
| Awesome 列表 | https://github.com/0xNyk/awesome-hermes-agent |
| Kanban 面板 | https://github.com/amanning3390/hermes-agent-kanban |
| 并行 Worker | https://github.com/r0b0tlab/hermes-concurrent-agents |

---

## Kanban 多 Agent 协作的核心思想

> **核心原则：不要用对话（Chat）来编排 Agent，用量（Kanban）来编排。**

### 为什么用 Kanban？

传统多 Agent 系统的问题：
1. Agent 之间用对话沟通 → 上下文无限膨胀，Token 消耗巨大
2. 依赖关系不清晰 → 不知道哪个 Agent 在等什么
3. 错误难以定位 → 一个 Agent 挂掉整条链断裂

Hermes 的 Kanban 方案：
1. **看板即真相**（Kanban as Source of Truth） — 所有任务状态都在看板上
2. **阻塞即安全阀**（Block as Safety Valve） — 遇到问题就 Block，不浪费 Token 硬猜
3. **Artifact 路径代替自然语言** — Agent 之间传递文件路径而非长篇对话
4. **下游由上游工件驱动** — 下一个任务的内容由上游产出的文件决定

### 看板任务状态流转

```
Ready ──► Running ──► Complete
              │
              └──► Blocked
```

---

## 快速上手

### 安装

```bash
# 一键安装
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 或者用 pip
pip install hermes-agent

# 初始化配置
hermes init
```

### 基础配置

创建 `hermes.yml` 配置文件：

```yaml
name: my-kanban-project

kanban:
  lanes:
    - name: 需求分析
      agents:
        - analyzer
      output: artifacts/requirements.md

    - name: 代码实现
      agents:
        - coder
      depends_on:
        - 需求分析
      input: artifacts/requirements.md
      output: artifacts/implementation/

    - name: 代码审查
      agents:
        - reviewer
      depends_on:
        - 代码实现
      input: artifacts/implementation/
      output: artifacts/review.md

agents:
  analyzer:
    model: nousresearch/hermes-3-opus
    system_prompt: "你是一个需求分析师"
  
  coder:
    model: nousresearch/hermes-3-opus
    system_prompt: "你是一个 Python 开发者"
  
  reviewer:
    model: nousresearch/hermes-3-opus
    system_prompt: "你是一个代码审查员"
```

### 启动看板

```bash
# 启动 Hermes Kanban 服务
hermes kanban start

# 或者使用独立看板面板
git clone https://github.com/amanning3390/hermes-agent-kanban
cd hermes-agent-kanban
# 按 README 配置并启动
```

---

## 实战案例：自动化博客工作流

参考文章中的例子，一个完整的自动化博客发布流程：

### 看板设计（4 个泳道）

| 泳道 | Agent | 输入 | 输出 |
|------|-------|------|------|
| 📝 选题策划 | Topic Agent | 用户提示 | 选题提案文档 |
| ✍️ 内容撰写 | Writer Agent | 选题文档 | 初稿 Markdown |
| 🎨 配图生成 | Image Agent | 文章内容 | 配图 URL |
| [OK] 审核发布 | Editor Agent | 初稿+配图 | 终稿+发布确认 |

### 4 条铁律（来自原文章）

1. **Kanban as Source of Truth** — 不看对话，只看看板状态
2. **Block as Safety Valve** — 遇到不确定就 Block，让用户介入
3. **Artifact Paths Over Natural Language** — 传文件路径，不要传对话历史
4. **Next Step Driven by Upstream Artifacts** — 下一个 Agent 的工作完全由上游产出的文件驱动

---

## 最佳实践

### 泳道设计原则
- 每个泳道一个明确的职责（单一职责原则）
- 泳道之间通过文件 artifact 传递，不要通过对话
- 每个泳道输出物要有明确格式和路径

### Agent 配置建议
- 为每个 Agent 编写清晰的 System Prompt
- 指定明确的输入/输出格式
- 设置合理的超时和重试策略

### 阻塞处理
- 当 Agent 不确定时，标记为 Blocked 而不是猜测
- 阻塞后通知用户介入
- 用户解决后恢复为 Ready

---

## 相关资源

- [Nous Research Hermes Agent GitHub](https://github.com/NousResearch/hermes-agent)
- [Hermes Agent 官方文档](https://hermes-agent.nousresearch.com/docs/)
- [橙皮书 — Hermes Agent 中文指南](https://github.com/alchaincyf/hermes-agent-orange-book)
- [Awesome Hermes Agent 资源列表](https://github.com/0xNyk/awesome-hermes-agent)
- [Hermes Agent Kanban Dashboard](https://github.com/amanning3390/hermes-agent-kanban)
- [Concurrent Agents Worker](https://github.com/r0b0tlab/hermes-concurrent-agents)

---

> **总结：Hermes Agent 用 Kanban 看板替代传统 Agent 对话链，让多个 AI Agent 像团队一样协作。核心是让任务状态可见、依赖关系清晰、错误处理优雅。适合：内容生产流水线、代码开发审核、数据处理管道等场景。**


## 实操记录：Windows 安装 Hermes Agent v0.15.1

### 踩坑记录

#### 1. Git 代理 SSL 问题
- 翻墙代理 `http://127.0.0.1:7890` 在 Git for Windows 上遇到 `SSL_ERROR_SYSCALL`
- **解决**：切换到 Windows 原生 SSL 后端
  ```bash
  git config --global http.sslBackend schannel
  ```

#### 2. Python 版本要求
- Hermes Agent 要求 **Python >= 3.11**
- 本机有 Python 3.8（PATH 默认）和 Python 3.13（通过 `py -3.13` 启动）
- **安装方式**（推荐从源码安装，方便修改）：
  ```powershell
  git clone https://github.com/NousResearch/hermes-agent.git
  py -3.13 -m pip install -e hermes-agent
  ```
- 安装后命令在：`C:\Users\T\AppData\Local\Programs\Python\Python313\Scripts\hermes.exe`

#### 3. 将 Hermes 加入 PATH 环境变量
```powershell
# 路径：C:\Users\T\AppData\Local\Programs\Python\Python313\Scripts
# 一行命令添加到用户 PATH（已执行）
[Environment]::SetEnvironmentVariable('Path', [Environment]::GetEnvironmentVariable('Path', 'User') + ';C:\Users\T\AppData\Local\Programs\Python\Python313\Scripts', 'User')
```

添加后新开终端即可直接使用 `hermes` 命令，无需全路径。


### 验证安装

```powershell
hermes --version
hermes kanban --help
```

### Kanban 命令速查

| 命令 | 用途 |
|------|------|
| `kanban init` | 创建 kanban.db |
| `kanban boards create <slug>` | 创建新看板 |
| `kanban create <title>` | 创建任务 |
| `kanban list` | 查看任务列表 |
| `kanban claim` | 认领任务 |
| `kanban complete <id>` | 完成任务 |
| `kanban block <id>` | 阻塞任务 |
| `kanban link <parent> <child>` | 建立依赖 |
| `kanban context <id>` | 查看任务上下文 |
| `kanban swarm` | 创建并行 Worker 集群 |
| `kanban dispatch` | 执行一次调度 |
| `kanban stats` | 看板统计 |
