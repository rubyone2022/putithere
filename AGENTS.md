# Agent Instructions

## Purpose

This repository is used to preserve project context and hand work between ChatGPT Web and Codex/Desktop.

## Required reading order

Before starting work, read:

1. `docs/PROJECT.md`
2. `docs/STATE.md`
3. `docs/DECISIONS.md`
4. `docs/TASKS.md`
5. `docs/HANDOFF.md`
6. `docs/WEB_HANDOFF.md` when returning from Web Chat

## Working rules

- Treat repository files as the source of truth, not old chat history.
- Do not add secrets, credentials, personal data, production logs, or private customer data.
- Record durable decisions in `docs/DECISIONS.md`.
- Update `docs/STATE.md` when the actual project state changes.
- Update `docs/TASKS.md` when work starts, stops, becomes blocked, or finishes.
- Before switching clients, rewrite the appropriate handoff file.
- Prefer patches and Draft PRs over direct changes to protected branches.
- Clearly distinguish verified facts from assumptions requiring local validation.
