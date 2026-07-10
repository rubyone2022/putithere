# Desktop / Codex Handoff

Updated: 2026-07-10
Source: repository setup
Branch: main
Commit: update before each handoff

## Current objective

Establish the repository structure described by the Web–Codex handoff documentation.

## Completed

- Added the main agent instructions.
- Added project overview, state, decisions, and task tracking.
- Existing workflow and promotional documents remain under `docs/ai-workflows/`.

## Current status

This repository currently documents and demonstrates the handoff system. It does not yet contain a production application or automated writeback pipeline.

## Recommended next action

Implement an allowlist-based AI context export script and a local patch-import script.

## Local verification required

- Update the commit field before a real client switch.
- Record actual commands and test results when source code is added.
- Replace this setup handoff with the active task context.

## Do not do

- Do not add secrets or private production data.
- Do not claim that Web-generated code has been tested locally.
- Do not write directly to a protected primary branch without review.
