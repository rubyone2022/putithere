# Tasks

Project ID: `multi-endpoint-handoff`

## In progress

### HOF-001 Establish project-scoped repository layout

Status: in-progress
Owner: `chatgpt-web:gpt-5.6-thinking`

Acceptance criteria:

- 项目拥有独立状态文件；
- Agent 来源可登记；
- 当前交接和历史交接可区分；
- 两篇现有说明文档反映新架构。

### HOF-002 Define machine-readable handoff schema

Status: planned

Acceptance criteria:

- JSON Schema 校验必填身份与版本字段；
- CI 拒绝缺少 project ID、handoff ID 或 base commit 的写回包。

## Next

- HOF-003 编写 handoff ID 与 session ID 生成脚本；
- HOF-004 增加历史交接归档脚本；
- HOF-005 增加 Secret/PII 扫描；
- HOF-006 实现 Patch 导入与 Draft PR；
- HOF-007 为多项目建立索引页。

## Done

- 建立总体方案文档；
- 建立额度救急宣传文稿；
- 建立项目定义和当前状态；
- 确认 Git 为最终版本事实源。