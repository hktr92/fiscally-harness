# Frontend Project Workflow

Use this rule before changing frontend dependencies, touching multiple packages, or adjusting build tooling.

## Project Discovery

- Run commands from the actual frontend workspace root, normally `frontend/`.
- Treat `package.json`, package-local manifests, lockfiles, `pnpm-workspace.yaml`, and `turbo.json` (where present) as script and dependency truth.
- Verify exact library versions before using TanStack, Tailwind, Vite, React, shadcn or Tauri version-specific APIs.
- Scope commands with `pnpm --filter <package>` when the project defines a matching workspace package.
- Inspect scripts before invoking lint, build, typecheck, test, or formatting.
- Do not assume a root test runner exists.

## App and Package Awareness

- An app can live directly in `frontend/` or in `frontend/apps/<app>/`.
- Reusable components may live in `packages/ui/` if the workspace actually has a shared UI package.
- Common config and typed API clients may live in packages as complexity grows, but creating additional package tiers is not a default requirement.
- Native Tauri code commonly lives under the owning app's `src-tauri/`.
- If prototypes or demo apps exist, don't treat them as canonical without explicit project guidance.

## Change Discipline

- Keep edits scoped to the affected app or package.
- Do not use write-formatting commands as verification checks.
- Preserve generated route trees unless the route/tooling update requires regeneration.
- Preserve generated Android sources unless native generation is explicitly requested.
- Treat all pre-existing uncommitted changes as user-owned.

## Quality Gates

- Prefer configured scripts over invented commands.
- Run narrow relevant checks, then broader checks if shared packages changed.
- Distinguish missing checks, blocked checks, inherited failures and new failures.
