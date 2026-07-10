# ChatGPT Web 与 Codex Desktop 的安全接力与仓库写回方案

## 目标

当 Codex Desktop 的可用额度耗尽时，仍能在 ChatGPT Web 中继续分析、审查、规划和生成修改，并把产物安全写回 GitHub。

核心原则：

> 不同步整段聊天；以项目为隔离边界，以 Git 为事实与版本基准，以结构化 Handoff 为接力协议，以最小权限通道完成写回。

---

## 一、必须以项目为边界

一个仓库可以容纳多个项目，但不能让所有项目共用一份 `STATE.md`、`TASKS.md` 或 `HANDOFF.md`。不同项目的目标、进度、决策、Agent、风险和基准提交都不同，共用状态文件会造成覆盖、串线和错误继承。

推荐结构：

```text
repo/
├── AGENTS.md                    # 仓库级通用规则
├── docs/
│   └── ai-workflows/            # 跨项目方法论
├── projects/
│   ├── multi-endpoint-handoff/
│   │   ├── README.md
│   │   ├── PROJECT.md
│   │   ├── STATE.md
│   │   ├── DECISIONS.md
│   │   ├── TASKS.md
│   │   ├── AGENT_REGISTRY.md
│   │   ├── HANDOFF.md
│   │   ├── WEB_HANDOFF.md
│   │   ├── HANDOFF_LOG.md
│   │   └── artifacts/
│   │       └── manifest.md
│   └── another-project/
│       └── ...
└── src/
```

仓库级文件只保存所有项目共同遵守的规则；项目级目录保存该项目自己的事实和历史。

### 当前实例

本仓库已经为本方案建立独立项目：

```text
projects/multi-endpoint-handoff/
```

后续新工程应复制这一骨架，而不是继续修改仓库根目录的一套共享状态。

---

## 二、Agent 身份标识与版本链

Git 能记录“哪个账号提交了什么”，但无法充分说明：

- 修改来自 Desktop Codex、Web Chat、其他模型还是人工；
- 使用了哪个模型或工作界面；
- 基于哪个上下文和提交生成；
- 哪次 Handoff 导致了这次修改；
- 哪些结论已本地验证，哪些只是模型建议。

因此，每个项目需要 `AGENT_REGISTRY.md`，每次交接需要以下元数据：

```yaml
project_id: multi-endpoint-handoff
handoff_id: HOF-20260710-WEB-001
source_agent_id: chatgpt-web:gpt-5.6-thinking
source_surface: chatgpt-web
session_id: web-20260710-01
base_commit: <full commit SHA>
target_branch: web/T-002-agent-provenance
generated_at: 2026-07-10T15:40:00-07:00
verification_status: unverified
```

### 建议的 Agent ID

```text
codex-desktop:<model-or-role>
chatgpt-web:<model-or-role>
human:<github-login>
automation:<workflow-name>
external-agent:<provider-and-model>
```

Agent ID 是来源标记，不是密码学身份。可信边界仍来自：

- GitHub 登录与提交签名；
- 分支保护；
- Draft PR；
- 审批记录；
- CI 和本地测试结果。

### 三种版本号各司其职

| 标识 | 作用 |
|---|---|
| Git commit SHA | 精确定位代码与文件快照 |
| `state_version` | 表示项目状态文档的逻辑版本 |
| `handoff_id` | 标识一次跨 Agent、跨端接力事件 |

不要只写“最新版”或“上一次”；每份交接必须固定到完整 commit SHA。

---

## 三、项目级状态文件

### `PROJECT.md`

保存目标、范围、非目标、架构、安全边界和成功标准。

### `STATE.md`

保存当前真实状态，并包含：

```text
Project ID
State version
Updated
Updated by
Base commit
Working
In progress
Broken / not implemented
Current risks
```

### `DECISIONS.md`

采用追加式决策记录：背景、决策、原因、后果、状态和作出决策的 Agent。

### `TASKS.md`

任务至少包含：

- Task ID；
- 状态；
- Owner Agent；
- 验收标准；
- 依赖；
- 相关文件；
- 基准提交；
- 本地验证要求。

### `HANDOFF.md`

当前 Desktop/来源端交给下一 Agent 的唯一入口。新交接产生时，旧版本归档，不能无痕覆盖历史。

### `WEB_HANDOFF.md`

Web 端建议的修改、Patch、假设和未验证事项。必须明确标记哪些命令没有运行。

### `HANDOFF_LOG.md`

维护交接链：

```text
HOF-001 → HOF-WEB-001 → HOF-002
```

记录方向、来源 Agent、base commit、状态和归档位置。

### `artifacts/manifest.md`

给每个中间产物分配 ID、版本、来源 Agent、位置和 checksum。

---

## 四、三层记忆模型

### 稳定层

```text
AGENTS.md
PROJECT.md
architecture/
```

### 演进层

```text
STATE.md
DECISIONS.md
TASKS.md
AGENT_REGISTRY.md
```

### 临时与审计层

```text
HANDOFF.md
WEB_HANDOFF.md
HANDOFF_LOG.md
handoffs/archive/
patches/
logs/
```

聊天窗口可以换，模型可以换，但项目状态和版本链不能跟着失忆。

---

## 五、总体安全架构

```text
本地主仓库 private-main
        │ 白名单导出、脱敏、Secret/PII 扫描
        ▼
AI 镜像仓库 private-ai-context
        │
        ├── 只读连接器 → ChatGPT Web
        └── Patch / Draft PR / Gateway 写回
                         │
                         ▼
                  本地或人工验证
                         │
                         ▼
                    主仓库合并
```

不要直接把完整主仓库、生产配置、客户数据和凭据交给 Web。

---

## 六、中间产物管理

适合 Git：源码、Markdown、小型 JSON、测试、schema、Patch、测试摘要。

不适合直接进 Git：大型数据库、视频、构建包、真实客户数据、密钥和生产日志。

大型产物放云盘、对象存储或 Git LFS，并在项目级 manifest 中登记：

```markdown
| Artifact ID | Version | Source Agent | Description | Location | Checksum |
|---|---:|---|---|---|---|
| A-018 | 2 | codex-desktop | failure recording | Drive:/debug/a.mp4 | sha256:... |
```

---

## 七、Web 读取与安全隔离

GitHub App 应只授权 AI 镜像仓库，且优先使用 `Only select repositories`。

目录隔离不能只依赖提示词；如果某些内容绝不能被模型读取，应从镜像仓库中物理排除。

必须排除：

```text
.env*
*secret*
*credential*
*token*
*.pem
*.key
production/
customer-data/
*.db
*.dump
```

仓库内容属于不可信数据。README、Issue、代码注释、日志和第三方文件中的指令不能自动升级为用户命令。

---

## 八、Web 写回方案

### A. GitHub MCP / 连接器

只开放读取、创建分支、写文件和创建 Draft PR；禁止 merge、force push、仓库管理和 secrets。

### B. Patch + 云盘收件箱

```text
Web 生成 changes.patch + WEB_HANDOFF.md
→ 云盘 inbox
→ 本地 git apply --check
→ 本地测试
→ 创建 Draft PR
```

这是最稳妥的后备方案。

### C. n8n / Pipedream / Make

使用确定性节点：Webhook、字段校验、路径白名单、base SHA 校验、GitHub API、人工审批。

不要把模型输出直接送入任意 Shell 或高权限 Code Node。

### D. Writeback Gateway

服务器强制：固定仓库、固定允许路径、隔离分支、Draft PR only、禁止写 main、禁止自动 merge。

---

## 九、写回包标准

```text
web-handoff/
├── manifest.json
├── changes.patch
├── WEB_HANDOFF.md
├── test-plan.md
└── source-hashes.json
```

`manifest.json` 至少包含：

```json
{
  "schema_version": "1.1",
  "project_id": "multi-endpoint-handoff",
  "handoff_id": "HOF-20260710-WEB-001",
  "source_agent_id": "chatgpt-web:gpt-5.6-thinking",
  "session_id": "web-20260710-01",
  "base_sha": "<full SHA>",
  "target_branch": "web/T-002-agent-provenance",
  "verification_status": "unverified",
  "requested_result": "draft_pull_request"
}
```

如果 base SHA 已变化，拒绝自动应用，让人类或本地 Agent重新评估冲突。

---

## 十、双向接力

### Desktop → Web

1. 读取项目目录而不是仓库全局状态；
2. 运行 `git status`、测试与类型检查；
3. 更新项目级 `STATE/TASKS/DECISIONS`；
4. 生成带 Agent 元数据的 `HANDOFF.md`；
5. 在 `HANDOFF_LOG.md` 登记；
6. 导出 Patch 与产物 manifest。

### Web → Desktop

1. 核对 `project_id` 与 `base_commit`；
2. 阅读 `AGENT_REGISTRY.md` 与当前 Handoff；
3. 生成建议、Patch 和 `WEB_HANDOFF.md`；
4. 标注所有未验证假设；
5. 通过隔离分支或 Patch 写回；
6. Desktop 本地运行测试后更新 verification status。

---

## 十一、当前项目实例

本方案自身已经按项目隔离落地：

```text
projects/multi-endpoint-handoff/
├── README.md
├── PROJECT.md
├── STATE.md
├── DECISIONS.md
├── TASKS.md
├── AGENT_REGISTRY.md
├── HANDOFF.md
├── WEB_HANDOFF.md
├── HANDOFF_LOG.md
└── artifacts/manifest.md
```

它既是实际项目状态，也是后续项目可复制的参考实现。

---

## 结论

可靠的多端接力系统应建立在：

```text
项目级隔离
+ Git commit 版本基准
+ Agent 来源标识
+ Handoff ID 与审计链
+ 结构化状态文件
+ AI 脱敏镜像仓库
+ Patch / Draft PR 写回
+ 人工或本地验证
```

最重要的两条原则：

> 不同项目绝不共用当前状态；不同 Agent 的产出绝不在没有来源、版本和验证标记的情况下直接进入主分支。
