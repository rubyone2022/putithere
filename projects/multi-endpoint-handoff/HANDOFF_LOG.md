# Handoff Log

Project ID: `multi-endpoint-handoff`

| Handoff ID | Direction | Source Agent | Base commit | Status | Current file |
|---|---|---|---|---|---|
| `HOF-20260710-001` | Desktop/authoring → Web | `chatgpt-web:gpt-5.6-thinking` | `454b48731fb871983e068e1918b58278624b2199` | current | `HANDOFF.md` |
| `HOF-20260710-WEB-001` | Web → Desktop | `chatgpt-web:gpt-5.6-thinking` | `bf6399187165d9544b0be0c07de611f30e33b69f` | current | `WEB_HANDOFF.md` |

## Rules

- 新交接产生后，将旧交接复制到 `handoffs/archive/<generated-at>-<handoff-id>.md`；
- 日志只追加，不复用 handoff ID；
- 被替代的交接标记为 `superseded`；
- 无法应用的交接标记为 `rejected` 并记录原因；
- Git commit 和 PR 仍是文件内容版本的最终事实源。