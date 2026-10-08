# Frontend Quality Gates

Run checks relevant to the touched surface. Discover commands from package scripts first.

## Candidate checks (if configured)

```bash
pnpm lint
pnpm typecheck
pnpm test
pnpm build
# In a configured workspace only:
pnpm --filter <actual-package-name> build
```

Do not infer that every script exists. Inspect `package.json` and package scripts.

## Missing or Partial Gates

- A project may not have a root test runner or `format:check`.
- A write-format script such as `pnpm format` is not a check.
- Some workspaces expose typecheck only in selected packages.
- For native Tauri validation use the relevant Cargo/build scripts separately.

## Handoff Rules

- Run narrow checks first; expand only when shared behavior/dependencies change.
- For lockfile updates run relevant install/audit/build checks, or explain blockers.
- Note regenerated routes/types or native code explicitly.
- Include failing command and diagnostic context for any inherited failures, distinguished from the current change.
