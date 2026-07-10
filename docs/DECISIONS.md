# Decision Log

## D-001: Use Git as the durable project-state layer

Date: 2026-07-10
Status: accepted

### Decision

Store durable context, handoffs, patches, and reviewable artifacts in Git rather than relying on client-specific chat history.

### Reason

ChatGPT Web and Codex/Desktop do not reliably share complete thread context, local files, execution state, or artifacts.

## D-002: Separate reading from writing

Date: 2026-07-10
Status: accepted

### Decision

Use a read-only repository connection for analysis and a controlled patch or Draft PR path for writeback.

### Reason

This reduces the impact of accidental edits, prompt injection, and excessive repository permissions.

## D-003: Use a sanitized AI mirror for sensitive projects

Date: 2026-07-10
Status: accepted

### Decision

Do not connect a full sensitive primary repository when an allowlisted, secret-scanned AI mirror can provide sufficient context.

### Consequence

The mirror may lag behind the primary repository, so every handoff must include a branch and commit reference.
