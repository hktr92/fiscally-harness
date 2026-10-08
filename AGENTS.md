# Repository agent instructions

This repository is a reusable coding harness, not an application with an established product domain.

## Orientation
- Read this file first, then the nearest nested AGENTS.md for files being changed: `frontend/AGENTS.md`, `backend/AGENTS.md`, or `docs/AGENTS.md`.
- Read **only relevant** documents in `.agents/rules/`. These are conventional project documents; do not assume Codex automatically loads them.
- Use an applicable skill from `.agents/skills/` when it provides a concrete workflow. Skills must not override explicit user requests or repository constraints.
- Inspect actual files and versions before assuming code, scripts, tools, frameworks, or package names exist.

## Execution
- Keep modifications focused. Do not redesign untouched areas or add speculative infrastructure.
- For small tasks: investigate, fix, validate, and hand off directly. Plan larger or risky tasks explicitly.
- Never mandate subagents. Use parallel work only for truly independent tasks with measurable benefit.
- Preserve uncommitted/user-owned changes. Never run destructive Git commands or force-push without explicit authorization.
- If executing an issue, follow `docs/AGENTS.md` and the `issue-execution` skill.
- Prefer minimal, inspectable diffs; don't mass-format unrelated files.

## Handoff
- Run relevant **configured** quality gates. Report exact commands and outcomes; if a tool is missing, say so.
- Distinguish newly introduced failures from pre-existing failures.
- When asked to complete an issue, commit only task-owned changes and report commit SHA, changed areas and outstanding blockers. Do not commit secrets.
- Never claim a release/device test or CI result without running it.
