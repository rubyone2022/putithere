# Project Overview

Updated: 2026-07-10

## Goal

Build a durable, auditable handoff system between ChatGPT Web and Codex/Desktop so work can continue when one client or allowance becomes unavailable.

## Core approach

- Git stores durable context and reviewable artifacts.
- Structured Markdown files hold project state, decisions, tasks, and handoffs.
- ChatGPT Web handles analysis, review, planning, and patch generation.
- Codex/Desktop handles local repository access, command execution, testing, and debugging.
- Sensitive source material should be exposed only through a sanitized AI mirror repository when necessary.

## Main documentation

- `docs/ai-workflows/chatgpt-web-codex-handoff-and-secure-writeback.md`
- `docs/ai-workflows/codex-quota-emergency-web-handoff-promo.md`

## Security constraints

- Never commit secrets or production credentials.
- Prefer allowlisted exports over copying a complete private repository.
- Web-originated changes should normally return through a patch or Draft PR.
- Human or local validation is required before merging changes into a primary codebase.
