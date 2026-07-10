# Project Definition

Project ID: `multi-endpoint-handoff`
Status: active

## Goal

建立一套在 Codex Desktop、ChatGPT Web 及其他 Agent 之间安全迁移工程上下文和中间产物的协议，使 Codex agentic usage 暂时耗尽时，项目仍可在 Web 端继续分析、审查、生成 Patch 与准备下一轮执行。

## Scope

- 项目级上下文隔离；
- Desktop → Web → Desktop 双向交接；
- 来源 Agent 身份标识；
- 基于 Git commit、branch、handoff ID 的版本追踪；
- AI 镜像仓库和最小信息暴露；
- Patch、Draft PR 或受控 Gateway 写回；
- Secret、PII 与 Prompt Injection 防护。

## Non-goals

- 绕过或增加 Codex 额度；
- 让 Web 完全代替本地终端与真实测试环境；
- 让 Agent 自动合并主分支；
- 在交接文件中保存密钥或真实客户数据。

## Success criteria

- 任一新 Agent 可在读取本目录后复述当前状态与下一步；
- 每次交接可以追溯到来源 Agent、会话和 base commit；
- 不同项目的状态不会互相覆盖；
- Web 产物可通过隔离分支或 Patch 安全回流；
- 旧交接可以归档，而当前状态始终唯一明确。
