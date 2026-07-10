# Multi-endpoint Handoff

本目录是“ChatGPT Web 与 Codex Desktop 多端接力”项目的独立状态空间。

仓库根目录保存跨项目通用规则；本目录保存该项目自己的目标、进度、决策、任务、交接记录、Agent 身份与产物索引。

## 读取顺序

1. `PROJECT.md`
2. `STATE.md`
3. `DECISIONS.md`
4. `TASKS.md`
5. `AGENT_REGISTRY.md`
6. 最新的 `HANDOFF.md` 或 `WEB_HANDOFF.md`
7. `HANDOFF_LOG.md`
8. `artifacts/manifest.md`

## 版本与身份

每份交接必须包含：

- `project_id`
- `handoff_id`
- `source_agent_id`
- `source_surface`
- `session_id`
- `base_commit`
- `target_branch`
- `generated_at`

这些字段用于区分不同来源 Agent、确认交接所基于的代码版本，并形成可审计的接力历史。
