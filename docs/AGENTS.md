# Documentation and file-based issue workflow

`docs/` is the source of truth for decisions and the local issue queue. Don't turn document-only requests into application implementation without instruction.

## Issue states
- `issues/0-draft/`: open questions, exploration, unclear scope.
- `issues/1-new/`: ready, bounded issue with objective, constraints and verifiable acceptance criteria.
- `issues/2-in-progress/`: active work. Move the issue here **before code edits** when executing it.
- `issues/3-done/`: satisfied acceptance criteria, relevant checks run, status/handoff recorded.

Move **one file** between directories; do not duplicate active issues. On blocked work, keep it in progress and record the blocker; don't mark it done. The operator may choose a different workflow explicitly.

## Executing an issue
1. Read the complete issue and related instructions.
2. Inspect repository state and existing code; preserve user-owned changes.
3. Move to `2-in-progress`; implement within scope; update the issue with decisions and evidence.
4. Run affected checks; document exact commands and any limitations or inherited failures.
5. When acceptance criteria are met, move to `3-done`; commit task-owned code **and** issue movement in one coherent commit.
6. Report changed files, checks, remaining risks, and commit SHA.

Use `issues/TEMPLATE.md` to create new issues and `../.agents/skills/issue-execution/SKILL.md` for repeatable execution. Don't auto-run `0001-example-endpoint.md` before an application exists.

## Docs
- Keep architecture docs factual; mark proposals as proposals.
- Record decisions in `decisions/` using context, decision, consequences.
- Don't write fictional benchmark or test results.
