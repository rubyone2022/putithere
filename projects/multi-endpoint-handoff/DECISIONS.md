# Decision Log

Project ID: `multi-endpoint-handoff`

## D-001: Use project-level state boundaries

Status: accepted

每个项目使用独立目录 `projects/<project-id>/` 保存自己的 PROJECT、STATE、DECISIONS、TASKS、HANDOFF、Agent 注册表和产物索引。仓库根目录只保存跨项目通用规范。

原因：不同项目进度、风险、参与 Agent 和交接节奏不同，共用一份状态文件会产生覆盖和串线。

## D-002: Treat Agent identity as version metadata

Status: accepted

每次交接必须记录 `source_agent_id`、`session_id`、`handoff_id`、`base_commit`、`target_branch` 和时间戳。

原因：仅知道“由 ChatGPT 生成”不足以追踪来源，也无法判断产物基于哪个版本。

## D-003: Git remains the source of truth

Status: accepted

Agent 的对话记忆和身份标签不替代 Git commit、branch、diff 与 PR。状态文件描述语义状态，Git 提供内容版本与变更历史。

## D-004: Agent identity does not grant authority

Status: accepted

身份登记只用于可追溯性。写入、执行、审批和合并权限必须由连接器权限、分支保护和人工审批决定。

## D-005: Keep current handoff canonical and archive old handoffs

Status: accepted

`HANDOFF.md` 与 `WEB_HANDOFF.md` 只表示当前有效交接；历史版本进入 `handoffs/archive/`，并登记在 `HANDOFF_LOG.md`。