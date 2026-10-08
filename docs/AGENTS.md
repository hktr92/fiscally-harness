# Documentation and issue workflow

This directory owns decisions, engineering context and a file-based, Git-versioned issue lifecycle. Read root `AGENTS.md` first. See `../.agents/skills/issue-execution/SKILL.md` for the full implementation procedure.

## States and ownership

- `issues/0-draft/`: human-owned discovery space. Codex may edit drafts only when explicitly asked; never auto-execute them.
- `issues/1-new/`: accepted, actionable and bounded. Each ready issue has clear scope and verifiable acceptance criteria.
- `issues/2-in-progress/`: a claimed issue being investigated, implemented and reviewed.
- `issues/3-done/`: completed artifacts and validation evidence. Use `YYYYMMDD-<issue-id>-<slug>.md` when closing.

Do not maintain competing active copies. Move the same file through states. Do not confuse these folders with GitHub Issues; synchronization is not automatic.

## Claim / plan / build / verify / close

1. Inspect Git status and current task ownership; never overwrite unrelated changes.
2. Read the whole issue and its referenced docs before implementation.
3. Move a ready issue from `1-new/` to `2-in-progress/`. A dedicated claim commit is preferred for auditability when the workflow is being followed end-to-end; commit only the move.
4. Investigate existing behavior. For material work, record a bounded implementation plan. Avoid verbose plans for trivial changes.
5. Implement, validate and review against *every* acceptance criterion.
6. Run relevant configured quality gates, then any integration gate needed for cross-boundary work.
7. Record actual commands/results, changes, any inherited failures, risks and follow-ups.
8. If done, date-prefix and move the issue under `3-done/`, then commit the task-owned code and closing move. Report SHA.
9. If blocked, keep in `2-in-progress/` with an actionable blocker. Never mark done merely because code was written.

## Issue hygiene

- Preserve the issue identifier across moves and commits.
- Split oversized work into separate issues with explicit dependencies.
- Keep active issues concise but executable without mind-reading.
- Mark acceptance checkboxes only when evidence exists.
- Use `issues/TEMPLATE.md` and `issues/EXAMPLE.md` as the format reference.
- Do not move work out of `0-draft/` automatically or treat `3-done/` as a todo queue.

## Architecture and decisions

- `architecture/` documents existing mechanisms, distinguishing proposed from shipped behavior.
- `decisions/` holds concise ADRs with context, decision and consequences.
- Never invent tests, metrics, external system state or executed commands.
