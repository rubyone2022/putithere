# Codex 额度耗尽了？先别急着下班 ☺️

Codex 正写到关键位置，额度突然用完；本地线程里塞满上下文，Web Chat 却像刚入职一样一脸茫然。

于是我整理了一套略显得意、但确实能救急的办法：

> 把 Git 当长期记忆，把项目级 `HANDOFF.md` 当接力棒，把 ChatGPT Web 当 Codex 暂时离场后的替补队员。

它不增加额度，不绕过限制，也不是官方无限续杯。它只是努力把“被迫停工”变成“换个工位继续干活”。

## 为什么 Codex 用尽后，ChatGPT Web 还能干活

ChatGPT 订阅并不是只有一个万能额度桶。Codex、Work 等 Agent 功能与标准 Web Chat 的模型使用可能采用不同 allowance 和重置机制。

因此可能出现：

```text
Codex：agentic usage 已耗尽

ChatGPT Web：
✓ 仍能聊天
✓ 仍能读 HANDOFF、Diff 和源码片段
✓ 仍能分析报错、设计方案和审查代码
✓ 仍可能继续使用 GPT-5.6 Sol
✓ 仍能生成 Patch、测试计划和下一轮任务单
```

所以，Codex 下班了，不等于整个 ChatGPT 都下班了。只要 Web 模型选择器里相关模型仍可用，把上下文接过去，依然可能美美使用 GPT-5.6 Sol 接着干活 ☺️

具体可用模型和次数仍以账号当时显示的套餐、allowance、额度提示和重置时间为准。

## 不同套餐大概意味着什么

| 套餐 | 大致定位 |
|---|---|
| Plus | 高级 ChatGPT 能力，加一定量 Codex/Agent 使用；适合偶尔做集中编程任务 |
| 100 美元 Pro | 核心高级能力较完整，使用 allowance 高于 Plus，但低于最高档 |
| 200 美元 Pro | 面向高频、长时间、高算力使用，仍不等于所有功能永久绝对无限 |

较高档位意味着多个能力池通常更宽裕，而不是获得一个永远不会耗尽的总额度。Codex 任务的实际消耗还受仓库大小、会话长度、任务复杂度、模型和工具调用影响。

关键结论是：

> Codex 的额度耗尽，不等于订阅完全不能用；标准 Web Chat、特定高级模型、文件或其他能力可能仍有自己的可用额度。

## 真正关键的新升级：按项目分割

最初看起来，只要在仓库根目录放一份 `STATE.md` 和 `HANDOFF.md` 就行。

后来发现不对。

不同项目有不同进度、不同目标、不同决策和不同基准提交。把所有项目塞进一套共享状态文件，迟早会发生：

- A 项目的状态覆盖 B 项目；
- Web Agent 读错当前任务；
- 旧 Handoff 被误认为最新版本；
- 一个项目的安全规则被错误套用到另一个项目；
- 不知道某段修改到底来自哪个 Agent。

所以正确结构应该是：

```text
repo/
├── AGENTS.md                  # 仓库级通用规则
├── projects/
│   ├── multi-endpoint-handoff/
│   │   ├── PROJECT.md
│   │   ├── STATE.md
│   │   ├── DECISIONS.md
│   │   ├── TASKS.md
│   │   ├── AGENT_REGISTRY.md
│   │   ├── HANDOFF.md
│   │   ├── WEB_HANDOFF.md
│   │   ├── HANDOFF_LOG.md
│   │   └── artifacts/manifest.md
│   └── another-project/
│       └── ...
└── docs/                     # 跨项目方法论
```

仓库可以共享，当前状态不能共享。

## 还要知道“是谁干的”

只有 Git commit 还不够。Git 能告诉你哪个账号提交了文件，却不一定能告诉你：

- 是 Desktop Codex、Web Chat、人工还是自动化产生的；
- 使用了哪个模型或工作界面；
- 基于哪个 commit 和哪次 Handoff；
- 是否真实运行过测试；
- 哪些内容只是建议，哪些已经本地验证。

因此每次交接都应该带：

```yaml
project_id: multi-endpoint-handoff
handoff_id: HOF-20260710-WEB-001
source_agent_id: chatgpt-web:gpt-5.6-thinking
source_surface: chatgpt-web
session_id: web-20260710-01
base_commit: <full commit SHA>
target_branch: web/T-002-agent-provenance
verification_status: unverified
```

于是版本管理不再只有一条线，而是三条线互相咬合：

```text
Git commit SHA   → 文件与代码快照
state_version    → 项目状态版本
handoff_id       → 跨端、跨 Agent 接力事件
```

这套设计比较得意的地方就在这里 ☺️：

> 模型可以换、客户端可以换、额度池可以换，但每一次接力都知道来自谁、基于什么版本、做了什么、验证到哪一步。

## Web 与 Codex 最理想的分工

```text
Codex 有额度时：
执行、修改、跑终端、测试、验证

Codex 没额度时：
Web 分析、审查、生成 Patch、补文档、整理决策与下一轮任务

Codex 恢复后：
核对 base commit、本地应用 Patch、运行测试、更新验证状态
```

Web 不是假装成另一个本地 Codex，而是接管那些仍然可以完成、又不依赖真实本地环境的工作。

## 推荐救急组合

```text
项目级状态目录
+ Agent 身份登记
+ Git commit / state version / handoff ID
+ AI 脱敏镜像仓库
+ GitHub 只读连接
+ Web 生成 Patch 或 Draft PR
+ Desktop 本地验证后合并
```

读取时只给 Web 当前项目所需的最小信息；写回时只允许隔离分支或 Draft PR，不让模型直接合并主分支。

## 当前示例项目

这套方法已经不只是写在说明里，而是以独立项目落地在：

```text
projects/multi-endpoint-handoff/
```

里面包含项目定义、当前状态、决策、任务、Agent Registry、双向 Handoff、交接日志和 Artifact Manifest，可以直接作为其他项目的模板。

## 先说好：它只是救急方案

这套方法不保证效果。

不同模型对大型仓库、复杂 Patch 和长期上下文的理解能力并不一致；Web Chat 也不能完整替代本地文件系统、终端、依赖安装、真实测试和系统级调试。

它也不保证能用多久。

ChatGPT、Codex、GitHub Connector、MCP、Actions、额度规则、模型名称和产品权限都可能调整。今天可行的路径，未来可能被限制、改名、迁移，或者被更好的官方功能取代。

所以请把它理解为：

> 在当前产品边界下，为 Codex 额度耗尽准备的一条临时逃生通道，而不是永久漏洞，更不是无限额度方案。

能多救回来一轮思路、多完成一次审查、少重新解释一次项目背景，就已经赚到了 ☺️

## 完整工程方案

详细结构、安全边界和写回方案参见同目录：

`chatgpt-web-codex-handoff-and-secure-writeback.md`

项目实例参见：

`../../projects/multi-endpoint-handoff/`
