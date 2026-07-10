# Tasks

## In progress

### T-001 Establish repository handoff skeleton

Status: in progress

Acceptance criteria:

- `AGENTS.md` exists.
- Project, state, decision, task, and handoff documents exist.
- `artifacts/manifest.md` exists.
- `src/` is represented in Git.

## Next

### T-002 Add automated context export

- Create an allowlist-based export script.
- Add secret scanning before publishing an AI mirror.
- Generate branch, commit, changed-file, and test metadata.

### T-003 Add Web writeback import

- Accept a standard patch and `WEB_HANDOFF.md`.
- Validate the expected base commit.
- Apply changes on a temporary branch.
- Run local checks before creating a Draft PR.

### T-004 Add CI safety checks

- Detect secrets.
- Reject forbidden paths.
- Validate handoff metadata.

## Done

- Initial workflow architecture documented.
- Promotional explanation and quota-emergency use case documented.
