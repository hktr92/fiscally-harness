# Repository agent instructions

This is a generic Symfony + React + Tauri **engineering harness**, not an initialized application. Do not assume business domain, package names or previously generated project code.

## Instruction routing

- Root rules apply everywhere. Use nearest nested `AGENTS.md` in `frontend/`, `backend/`, or `docs/` when working there.
- `.agents/rules/` contains **contextual policy**, not automatically loaded Codex instructions. Read the smallest relevant subset for the task.
- `.agents/skills/*/SKILL.md` describes executable workflows; use an applicable skill rather than improvising rituals.
- Inspect repository reality (status, manifests, scripts, lockfiles, API contracts and package versions) before proposing changes.

## Execution discipline

- Solve small tasks directly: investigate → change → validate → handoff. Plan larger/multi-boundary tasks before editing.
- Avoid mandatory subagents, excessive planning, speculative abstractions or new dependencies without demonstrated need.
- Keep diffs small and reviewable. Don't format unrelated code, edit generated files casually or change cross-app architecture silently.
- User-owned uncommitted files must be preserved. Never use destructive Git cleanup/reset/force push without explicit permission.
- For tracked issues, follow `docs/AGENTS.md` and the `issue-execution` skill, including claim, integration QA, completion naming and commits.
- `code-clarity` is strictly a **read-only source audit**. It may write audit/issue documentation, but must not modify application code, tests or runtime configuration.

## Quality and handoff

- Run **configured** checks appropriate to the changed surface, narrow checks before broad checks.
- Distinguish checks run/passed, skipped because unavailable, inherited failures and newly introduced failures.
- Report touched areas, remaining risks and blockers. When the task requests issue execution, commit only issue-owned changes and report SHA.
- Never fabricate test outcomes, device validation, CI status or information absent from local source.
- Keep secrets, tokens, signing credentials and real personal data out of Git.
