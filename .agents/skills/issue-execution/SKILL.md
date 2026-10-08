---
name: issue-execution
description: Implement one ready docs/issues task through the repository's issue lifecycle, validations and commits.
---

# issue-execution


## When to use

Use when asked to execute a file under `docs/issues/1-new/` or continue work in `2-in-progress/`. Read `AGENTS.md` and `docs/AGENTS.md` first. Do not run examples or unapproved drafts.

## Claiming work

1. Inspect `git status --short --branch` and find any user-owned changes before touching files.
2. Read the complete issue, including blockers, dependencies, acceptance criteria and expected checks.
3. Inspect adjacent code and configured tools. If scope is ambiguous, record the smallest defensible interpretation; don't create speculative dependencies.
4. Move the issue from `1-new/` to `2-in-progress/`; preserve the stable issue identifier.
5. Make a small claim commit when the documented workflow expects an explicit claim, without staging unrelated files. Report/record its SHA.

## Implementation

6. Read only relevant `.agents/rules/` and local `AGENTS.md`.
7. Choose the smallest change that meets all criteria. Avoid broad refactoring, subagent rituals and new packages without need.
8. Add regression coverage for logic/bug changes. Keep fixtures synthetic; avoid private data.
9. For cross-boundary changes, verify the API/DTO contract on both sides.
10. Update the issue as discoveries change the plan. Do not silently broaden scope.

## Verification and closing

11. Run actual configured quality gates, narrow first. Record commands and output. Distinguish inherited failures from introduced failures.
12. Check each acceptance criterion individually; mark only verified items.
13. If blocked or checks fail because of new code, leave the issue in `2-in-progress/` and write the blocker.
14. If complete, add concise implementation notes, tests, limitations and outcome, then move to `3-done/YYYYMMDD-<id>-<slug>.md` using the completion date.
15. Make a focused completion commit (only owned code/docs/issue path), then report resulting SHA, changed areas and unverified assumptions.

## Boundaries

- Never stage `git add -A` across a dirty multi-project workspace without review.
- Never force-push, reset, drop other developers' changes or invent passing tests.
- Do not re-run finished issues unless expressly requested.
