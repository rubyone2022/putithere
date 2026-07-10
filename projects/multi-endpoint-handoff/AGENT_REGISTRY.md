# Agent Registry

Project ID: `multi-endpoint-handoff`

本文件登记参与项目的 Agent 身份标签。它提供可审计的来源标识，但不是密码学身份证明。

| Agent ID | Surface | Role | Trust level | Notes |
|---|---|---|---|---|
| `chatgpt-web:gpt-5.6-thinking` | ChatGPT Web | research, architecture, documentation, GitHub writeback | reviewed-write | 当前建立项目结构的来源 Agent |
| `codex-desktop:default` | Codex Desktop | local code editing, terminal, testing | local-executor | 实际实例应补充 session ID 和模型 |
| `human:ruby` | Human | approval, security review, merge decision | owner | 最终批准者 |

## Identity format

推荐格式：

```text
<surface>:<model-or-role>[:<instance>]
```

示例：

```text
chatgpt-web:gpt-5.6-thinking
codex-desktop:gpt-5.6-codex:macbook
human:ruby
n8n:writeback-gateway
```

## Required session fields

每次交接另行记录：

- `source_agent_id`
- `source_surface`
- `session_id`：无法取得真实会话 ID 时，生成本地 UUID；
- `model`：不可见时填 `unknown`，不得猜测；
- `capability_scope`：read / propose / write-branch / execute-local / approve；
- `base_commit`；
- `generated_at`。

## Trust rule

Agent 自报身份只用于追踪，不自动授予权限。真正的权限由 GitHub App、分支保护、路径白名单和人工审批控制。