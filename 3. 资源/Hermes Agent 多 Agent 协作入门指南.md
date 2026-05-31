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



## 实操验证：Kanban 初始化 + Gateway 启动

```powershell
# 初始化看板数据库（成功）
hermes kanban init
# 输出：Kanban DB initialized at C:\Users\T\.hermes\kanban.db

# 启动Gateway（首次需要安装为服务）
hermes gateway start
# 输出：✓ Created Scheduled Task 'Hermes_Gateway'
#       ✓ Gateway started (PID: ...)
```

### Gateway 工作原理
- Gateway 内嵌调度器（dispatcher），每 **60 秒** 扫描一次看板
- 检测到 Ready 状态的任务 → 自动分配给对应 Profile 的 Agent 执行
- 执行结果写入任务的工作目录作为 Artifact
- 依赖该任务的下游任务自动转为 Ready

### 初始化后的状态
- **看板数据库**：`C:\Users\T\.hermes\kanban.db`
- **Gateway 服务**：Windows 计划任务 `Hermes_Gateway`，开机自启
- **可用 Profile**：`default`（已注入 80+ 个 skill）
- **日志文件**：`C:\Users\T\.hermes\logs\gateway.log`

### 快速上手：创建一个多 Agent 协作看板

```powershell
# 1. 创建一个博客发布看板
hermes kanban boards create blog-publish

# 2. 创建任务链（选题 → 写作 → 配图 → 审核）
hermes kanban create "选题策划" --body "确定本周博客主题和大纲"
hermes kanban create "内容撰写" --body "根据选题文档撰写博客正文"
hermes kanban create "配图生成" --body "为文章生成配图和封面"
hermes kanban create "审核发布" --body "审核内容并发布到博客"

# 3. 建立依赖关系（content depends on topic, image & review depend on content）
hermes kanban link 2 1   # 内容撰写 依赖 选题策划
hermes kanban link 3 2   # 配图生成 依赖 内容撰写
hermes kanban link 4 2   # 审核发布 依赖 内容撰写
hermes kanban link 4 3   # 审核发布 也依赖 配图生成

# 4. 查看看板状态
hermes kanban list
hermes kanban stats

# 5. 认领并执行第一个任务
hermes kanban claim 1
```

> **小提示**：创建任务后需要手动 `claim` 认领，或等 dispatcher 自动调度。任务执行期间可以用 `hermes kanban tail <id>` 实时查看进度。

---



## 日常使用流程

### 三步启动 Hermes Agent

```powershell
# 1. （如果翻墙工具没开）先启动翻墙代理
# git/pip 走代理才能连接 GitHub/PyPI
# 代理地址：http://127.0.0.1:7890

# 2. （可选）启动 Web Dashboard
hermes dashboard --port 9119
# 浏览器打开 http://localhost:9119 查看

# 3. 开始使用
hermes chat                              # 交互式对话
hermes -z "帮我写个 Python 脚本"          # 一次性任务
hermes kanban list                       # 查看看板任务
```

> **注意**：Gateway 已安装为 Windows 计划任务 `Hermes_Gateway`，开机自动启动，**无需手动运行**。Gateway 每 60 秒扫描一次看板，自动调度 Ready 任务。

### 日常 Kanban 工作流

```powershell
# 查看看板状态
hermes kanban stats                      # 各状态任务数量统计
hermes kanban list                       # 查看所有任务
hermes kanban boards list                # 查看所有看板
hermes kanban boards switch <slug>       # 切换当前看板

# 管理任务
hermes kanban create "任务标题" --body "任务描述"
hermes kanban claim <id>                 # 认领任务
hermes kanban complete <id>              # 完成任务
hermes kanban block <id>                 # 阻塞任务
hermes kanban unblock <id>               # 解除阻塞
hermes kanban link <child> <parent>      # 建立依赖

# 查看任务详情
hermes kanban show <id>                  # 任务详情 + 评论
hermes kanban context <id>               # 任务上下文（Worker 视角）
hermes kanban tail <id>                  # 实时跟踪任务进度
hermes kanban log <id>                   # 查看 Worker 执行日志
```

### 常用命令速查

| 场景 | 命令 |
|------|------|
| 启动 Dashboard | `hermes dashboard --port 9119` |
| 停止 Dashboard | `hermes dashboard --stop` |
| 查看 Dashboard 状态 | `hermes dashboard --status` |
| Gateway 状态 | `hermes gateway status` |
| 查看 Gateway 日志 | `type $env:USERPROFILE\.hermes\logs\gateway.log` |
| 查看 Hermes 日志 | `hermes logs` |
| 查看 Hermes 配置 | `hermes config` |
| 编辑配置 | `hermes config edit` |
| 更新 Hermes | `hermes update` |

### 常用目录

| 路径 | 说明 |
|------|------|
| `~\.hermes\kanban.db` | Kanban 看板数据库 |
| `~\.hermes\logs\` | 日志文件目录 |
| `~\.hermes\config.yaml` | 主配置文件 |
| `~\.hermes\.env` | 环境变量 / API Key |
| 克隆的源码 | `C:\Users\T\Documents\Codex\2026-05-31\new-chat-2\hermes-agent` |

### 注意事项

1. **Python 版本**：Hermes Agent 需要 Python >= 3.11，当前系统默认 Python 3.8，使用 `py -3.13` 启动正确环境
2. **代理**：Git 配置了代理 `http://127.0.0.1:7890`，如果翻墙工具未启动，Git 操作会超时
3. **SSL 后端**：Git 已配置 `http.sslBackend = schannel`（Windows 原生），解决代理 SSL 问题
4. **PATH**：Hermes 命令已添加到用户 PATH，`C:\Users\T\AppData\Local\Programs\Python\Python313\Scripts`



## 配置 API Key

Hermes Agent 本身是框架，**必须配置一个 LLM 的 API Key 才能使用**。支持的 Provider：

| Provider | 环境变量 | 说明 |
|----------|----------|------|
| OpenRouter | `OPENROUTER_API_KEY` | 聚合平台，一个 Key 用几十种模型 |
| Anthropic | `ANTHROPIC_API_KEY` | Claude 系列模型 |
| OpenAI | `OPENAI_API_KEY` | GPT 系列 |
| Google Gemini | `GOOGLE_API_KEY` 或 `GEMINI_API_KEY` | Gemini 系列 |
| DeepSeek ❓ | 需用 `custom` provider + `base_url` | 见下方说明 |
| GitHub Copilot | `GITHUB_TOKEN` | 有 Copilot 订阅可用 |
| ZhipuAI GLM | `GLM_API_KEY` | 智谱 |
| Kimi | `KIMI_API_KEY` | Moonshot |
| LM Studio | 本地 | 本地跑模型 |
| Ollama / vLLM / llama.cpp | 本地 | 别名都映射到 `custom` |

### 配置步骤

```powershell
# 1. 创建环境变量文件
notepad $env:USERPROFILE\.hermes\.env
```

粘贴以下内容（选一个 Provider 取消注释）：

```env
# ── 方案一：OpenRouter（推荐，一个 Key 通用）──
# OPENROUTER_API_KEY=sk-or-v1-你的key
# 注册：https://openrouter.ai/keys

# ── 方案二：DeepSeek（通过 custom provider）──
# OPENAI_API_KEY=sk-你的deepseek-key
# 然后在 config.yaml 中设置 provider: custom + base_url

# ── 方案三：直接 DeepSeek API ──
# DEEPSEEK_API_KEY=sk-你的deepseek-key

# ── 方案四：Anthropic Claude ──
# ANTHROPIC_API_KEY=sk-ant-你的key

# ── 方案五：OpenAI ──
# OPENAI_API_KEY=sk-你的key

# ── 方案六：Google Gemini ──
# GOOGLE_API_KEY=你的key
```

```powershell
# 2. 运行交互式配置（选 Provider 和默认模型）
hermes model

# 3. 测试是否生效
hermes -z "你好，用中文回复一句话"
```

### DeepSeek 详细配置（custom provider）

因为 DeepSeek 用的是 OpenAI 兼容接口，可以用 `custom` provider：

编辑 `~\.hermes\config.yaml`：

```yaml
model:
  default: "deepseek-chat"
  provider: "custom"
  api_key: "sk-你的deepseek-key"    # 或写在 .env 的 OPENAI_API_KEY
  base_url: "https://api.deepseek.com/v1"
```

### 最简单上手方案：OpenRouter

> OpenRouter 是一个模型聚合平台，注册后拿到 Key，填到 `.env` 里就能用所有主流模型。
> https://openrouter.ai/keys

```env
OPENROUTER_API_KEY=sk-or-v1-你的key
```

然后运行 `hermes model` 选模型，或直接：

```powershell
hermes -m "openrouter/deepseek/deepseek-chat" -z "你好"
```

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
## 深入理解：Hermes 多 Agent 架构全解析

> 以下内容基于对 Hermes Agent 源码和核心 Skill（kanban-orchestrator、kanban-worker、kanban-codex-lane）的代码级分析。

### 核心架构：Profiles 是 Worker，不是 Codex/Claude Code

**关键结论：Hermes 本身就是一个多 Agent 框架，它的 Agent 就是 "Profiles"。Codex/Claude Code 是可选的辅助工具，不是默认 Worker。**

#### 架构层级图

```
┌──────────────────────────────────────────────────────────────┐
│                    用户 / CLI / Chat                          │
└─────────────────────┬────────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────────┐
│              Gateway (调度器，每 60 秒扫描看板)                 │
│   ┌──────────────────────────────────────────────────────┐   │
│   │  kanban.db (SQLite 看板数据库，即 Source of Truth)      │   │
│   │  - Ready ► Running ► Complete / Blocked                │   │
│   │  - 依赖关系：parents → children (自动 Ready 传播)       │   │
│   └──────────────────────────────────────────────────────┘   │
└──────────┬──────────┬──────────┬─────────────────────────────┘
           │          │          │
           ▼          ▼          ▼
┌─────────────┐ ┌─────────┐ ┌─────────┐
│ Profile A   │ │Profile B│ │Profile C│  ← Hermes 自己的 Agent Profile
│ (编排)      │ │ (专家)  │ │ (审查)  │     每个 Profile 是一个独立 Agent
└──────┬──────┘ └────┬────┘ └─────────┘
       │             │
       │    ┌────────┴────────┐
       │    │  (可选) Codex   │  ← 仅在 worker 有 kanban-codex-lane skill 时
       │    │  CLI 输入通道   │     才会启动 Codex 作为辅助
       │    └─────────────────┘
       │
       ▼
┌─────────────┐
│ Worker A    │ ← 分解大任务为子任务 → 创建更多 Kanban Card
│(Orchestrator)│
└─────────────┘
```

### 关键概念详解

#### 1. 什么是 Profile？

Profile 是 Hermes 的 Agent 实例。每个 Profile 有：
- **独立的 LLM 模型**（可不同 Provider）
- **独立的 System Prompt**
- **独立的 Skill 集合**（决定它有什么能力）
- **独立的配置**

初始化时自动创建了一个 `default` profile，注入了 80+ 个 skill。

#### 2. 编排器（Orchestrator）的工作流程

当用户在 Kanban 看板上创建一个复杂任务时，Orchestrator Profile 会：

1. **Step 0：发现可用 Profile**
   - 运行 `hermes profile list` 或询问用户
   - 必须基于实际存在的 Profile 来分配任务

2. **Step 1：理解目标并画任务图**
   - 提取请求中的多个工作流（lanes）
   - 将每个 lane 映射到对应的 Profile
   - 决定哪些可以并行，哪些有依赖

3. **Step 2：创建 Kanban Cards 并建立依赖**
   ```python
   t1 = kanban_create(title="研究 Postgres 成本", assignee="research-profile")
   t2 = kanban_create(title="研究 Postgres 性能", assignee="research-profile")
   t3 = kanban_create(title="综合推荐方案", assignee="analyst-profile",
                      parents=[t1, t2])  # t3 依赖 t1 和 t2
   ```

4. **Step 3：标记自己的任务完成**
   ```python
   kanban_complete(summary="分解为 T1-T4: 2 个研究并行，1 个综合推荐")
   ```

#### 3. Worker Profile 的执行流程

当调度器发现一个 Ready 状态的 Card 时，它启动对应的 Profile，Worker 会：

1. **查看任务上下文** — `kanban_show(id)` 获取任务详情
2. **读取上游工件**（Artifacts）— 前任 Worker 产出的文件
3. **执行实际工作** — 用自己配置的 LLM 完成编码/研究/写作
4. **输出工件** — 写文件到工作目录
5. **心脏搏动**（Heartbeat）— 长任务定期报告进度
6. **完成任务或阻塞** — `kanban_complete()` 或 `kanban_block()`

Worker 的输出格式（Handoff 规范）：

```python
kanban_complete(
    summary="完成了限流器：令牌桶算法，14 个测试全部通过",
    metadata={
        "changed_files": ["rate_limiter.py", "tests/test_rate_limiter.py"],
        "tests_run": 14,
        "tests_passed": 14,
        "decisions": ["先用 user_id，未登录用 IP 回退"],
    },
)
```

#### 4. Codex/Claude Code 的角色（可选）

**重要：Codex/Claude Code 不是默认的 Worker！**

它们的角色是**可选的输入通道**（Input Lane）：

```
Hermes Worker (Profile) ──→ 可选地启动 Codex CLI ──→ Codex 产出 diff
     │                                                     │
     └──────────── 审查 diff，跑测试，接受/拒绝 ←───────────┘
```

五个所有权规则：
1. **生命周期归 Hermes** — Codex 不能调 `kanban_complete` 等 Hermes API
2. **验收归 Hermes** — Codex 的 diff 要经过 Hermes 审查
3. **测试归 Hermes** — Codex 跑的测试是参考性的，Hermes 会重跑
4. **安全归 Hermes** — Codex 改了安全边界要拒绝
5. **清理归 Hermes** — 杀掉卡住的 Codex，删除临时 worktree

要启用 Codex 通道，Profile 需要有 `kanban-codex-lane` skill。

### 架构对比

| 特性 | Hermes 自带多 Agent | + Codex/Claude Code |
|------|-------------------|-------------------|
| **Worker 是什么** | Hermes Profile（Agent 进程） | Codex CLI（外部编码工具） |
| **需要什么配置** | LLM API Key + Profile 配置 | 额外需要 Codex/Claude Code 安装 |
| **默认启用** | ✅ 是 | ❌ 需要 skill 加持 |
| **适用场景** | 研究、分析、写作、审查、简单编码 | 复杂编码/重构/迁移 |
| **成本** | 低（Hermes 自己的 LLM 调用） | 高（Codex 也调用 LLM） |
| **隔离性** | Hermes 进程内 | 隔离的 Git Worktree |

### 你的配置现状

#### ❌ 缺少的部分
- 没有 `~/.hermes/config.yaml`（配置文件）
- 没有 `~/.hermes/.env`（API Key 文件）
- 没有任何 LLM API Key 在环境中

#### ✅ 已有的部分
- `kanban.db` 已初始化
- Gateway 已安装为 Windows 计划任务，正在运行（PID 22204）
- `default` profile 已创建，注入了 80+ skill
- Dashboard 可访问 http://localhost:9119
- Web UI 的 "Keys" 页面已找到（可配置环境变量）

#### 下一步配置步骤
```powershell
# 1. 运行交互式配置向导
hermes config wizard

# 2. 或手动创建 .env 文件
notepad $env:USERPROFILE\.hermes\.env
# 填入你的 API Key，例如：
# OPENROUTER_API_KEY=sk-or-v1-你的key

# 3. 运行模型配置
hermes model

# 4. 验证连接
hermes -z "你好，用中文回复一句话"
```

### 总结

**Hermes 的架构是**：**Profiles（Agent）** 通过 **Kanban Cards** 协调，由 **Gateway（调度器）** 自动分配任务。每个 Profile 是一个独立的 Hermes Agent 实例，有自己的 LLM 模型和工具集。

**Codex/Claude Code** 是 Hermes Worker 在需要时**可选调用的外部工具**，需要：
1. Worker Profile 加载了 `kanban-codex-lane` skill
2. Codex/Claude Code 已在本机安装
3. Worker 认为任务适合交给 Codex（有明确验收标准、可隔离的 worktree）

对于你现在的情况，核心是要**先配好 API Key**，让 Hermes 自带的 Agent（Profile）能跑起来。之后如果想用 Codex 辅助编码，再装 Codex 并配置 skill。

---

## 实操问答：Profile 与多 Agent 的真实机制

> 以下基于对 `hermes_cli/profiles.py`、`devops/kanban-orchestrator/skill`、`devops/kanban-worker/skill` 和 `cli.py` 的源码分析

### 核心答案

#### Q1：只要配好 DeepSeek 的 API 就行了吗？

**是的，但 DeepSeek 需要作为 custom provider 配置：**

Hermes 没有内置的 "deepseek" 提供商，但因为 DeepSeek 兼容 OpenAI 的 API 格式，可以用两种方式接入：

**方式一：OpenRouter（推荐 — 一个 Key 用几十种模型）**
```env
OPENROUTER_API_KEY=sk-or-v1-你的key
```
然后 `hermes model` 选 `openrouter/deepseek/deepseek-chat`，不需要配 base_url。

**方式二：直接 DeepSeek（custom provider）**
```env
OPENAI_API_KEY=sk-你的deepseek-key
```
然后在配置里指定 base_url 为 `https://api.deepseek.com/v1`。

配好之后：
```powershell
hermes -z "你好，用中文回答"    # 测试连通性
hermes chat                      # 交互式对话
```

**你现在 Gateway 已经跑了，就差这步 API Key 配置了。**

---

#### Q2：Worker Profile 是多个的吗？

**❌ 不是。默认只有 1 个 Profile（名叫 `default`）。**

多 Agent = 多 Profile，但默认**不会自动创建**多个 Profile。需要你手动创建：

```powershell
# 查看当前有多少 Profile（默认只有 default）
hermes profile

# 创建第二个 Profile（叫 coder，克隆 default 的配置和 skills）
hermes profile create coder --clone

# 每个 Profile 可以独立配置模型
hermes -p coder model        # 给 coder 配不同的 LLM

# 创建第三个 Profile（叫 reviewer，全新不含 skills）
hermes profile create reviewer

# 查看看板上有哪些可用的 assignee
#（Profile 就是看板上的 assignee——只能 assign 给已存在的 Profile）
hermes profile list
```

每个 Profile 是一个**完全独立**的目录：
```
~/.hermes/                   ← default profile
~/.hermes/profiles/coder/    ← coder profile（自己的 config.yaml、.env、skills）
~/.hermes/profiles/reviewer/ ← reviewer profile
```

**核心约束：** Orchestrator（编排器）在分解任务时，**必须知道哪些 Profile 存在**。如果只有 `default`，那分解出来的卡片只能 assign 给 `default`，等于没有多 Agent 分工。

---

#### Q3：都用 DeepSeek 这个模型去做任务吗？

**每个 Profile 可以独立配置模型**，例如：

```powershell
# Profile A（研究方向）→ 用 DeepSeek
hermes -p researcher model
# 选 deepseek-chat

# Profile B（编码方向）→ 用 Claude
hermes -p coder model
# 选 claude-sonnet-4-20250514

# Profile C（审查方向）→ 用 GPT-4o
hermes -p reviewer model
# 选 gpt-4o
```

或者全部用同一个 DeepSeek 也行。**每一个 Profile 有自己的 `config.yaml` 和 `.env`**，模型配置完全独立。

---

### Profile 生命周期图

```
┌─────────────────────────────────────────────────┐
│  hermes profile create coder --clone              │
│    ↓                                               │
│  ~/.hermes/profiles/coder/                        │
│    ├── config.yaml  ← 独立模型配置（可改 DeepSeek）│
│    ├── .env          ← 独立 API Key（可不同）      │
│    ├── SOUL.md       ← 独立身份/System Prompt     │
│    ├── skills/       ← 独立技能集合                │
│    ├── memories/     ← 独立记忆                    │
│    ├── sessions/     ← 独立会话历史                │
│    └── logs/         ← 独立日志                    │
└─────────────────────────────────────────────────┘
```

### 真正的多 Agent 工作流示例

```powershell
# 1. 创建三个 Profile
hermes profile create coder --clone
hermes profile create reviewer --clone
hermes profile create writer --clone

# 2. 分别配模型（可相同可不同）
hermes -p coder model        # 选 deepseek-chat
hermes -p reviewer model      # 选 claude-sonnet-4
hermes -p writer model        # 选 deepseek-chat

# 3. 创建看板，任务自动分配给不同 Profile
hermes kanban create "写一篇关于 AI Agent 的文章" --body "..."
# Orchestrator 会自动分解任务，assign 给 writer（写作）→ reviewer（审查）
# Gateway 调度器自动派发
```

---

### 你现在的情况总结

| 项目 | 状态 | 需要做什么 |
|------|------|-----------|
| **API Key** | ❌ 未配置 | 配 OpenRouter 或 DeepSeek Key |
| **Profile** | ✅ 1 个 default | 想多 Agent 就 `hermes profile create` |
| **Gateway** | ✅ 已运行 (PID 22204) | 无需操作 |
| **Kanban DB** | ✅ 已初始化 | 随时可用 |
| **Dashboard** | ✅ http://localhost:9119 | 在 Keys 页面配环境变量 |

### 最简单最快的启动路径

你先在 Web Dashboard（http://localhost:9119 ）的 Keys 页面配一个 `OPENAI_API_KEY`（DeepSeek Key），然后：

```powershell
hermes -z "你好"          # 测试能不能用
hermes chat               # 开始聊天
```

等 Hermes 本身能跑之后，再考虑：
1. 想多 Agent？→ `hermes profile create <name>` 创建更多 Profile
2. 想用 Codex 辅助？→ 装 Codex + 确保 Profile 有 `kanban-codex-lane` skill
3. 想自动化？→ 在 Dashboard 看板上创建任务链

---

## 实操验证：DeepSeek 配置成功

### 连通测试结果

**✅ 2026-05-31 DeepSeek 连通成功！**

| 项目 | 状态 | 详情 |
|------|------|------|
| API Key | ✅ 已配置 | DeepSeek API Key 已写入 `.env` |
| 模型 | ✅ deepseek-chat | 写入 `config.yaml` |
| 响应时间 | ✅ ~7秒 | DeepSeek-chat 首次预热稍慢 |
| Gateway | ✅ 运行中 | 计划任务 `Hermes_Gateway` |

### 配置内容

**`~/.hermes/.env`**
```env
DEEPSEEK_API_KEY=sk-db7f40e357c74d93b0ae8536397feb40
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
```

**`~/.hermes/config.yaml`**
```yaml
model:
  default: deepseek-chat
```

Hermes 自动从 `.env` 检测到了 DeepSeek 的 Key 和 Base URL，`config.yaml` 只需要指定 `model.default` 即可。

### 验证命令

```powershell
# 使用全路径（PATH 可能未刷新）
C:\Users\T\AppData\Local\Programs\Python\Python313\Scripts\hermes.exe -z "你好"

# 或者用 py 启动
py -3.13 -m hermes_cli.main -z "你好"
```

### 当前状态总结

| 功能 | 状态 |
|------|------|
| Hermes CLI | ✅ 可用 |
| DeepSeek 模型 | ✅ 连通 |
| Gateway 调度器 | ✅ 后台运行 |
| Web Dashboard | ✅ http://localhost:9119 |
| 多 Agent Profile | ⏳ 可创建 |
| Kanban 看板 | ⏳ 可开始使用 |
