# Frontend agent instructions

Applies in `frontend/`. Start with root `AGENTS.md` and selectively read `../.agents/rules/frontend/`; for Tauri/Rust, also read relevant `../.agents/rules/tauri/`.

## Discovery

- Inspect the actual `package.json` and lockfile before assuming React/TanStack/Tailwind versions or runnable commands.
- A single application is fine; use `apps/` and `packages/` only when the workspace actually needs them.
- React, TypeScript, TanStack Router/Query/Form, Tailwind v4, shadcn/Radix and pnpm are the preferred examples, not fabricated dependencies.

## Implementation

- Keep routes thin; move nontrivial UI and state to feature modules.
- Treat API contracts as backend-owned. Use the existing typed transport/hook boundary.
- Prefer installed shared UI components; never import a placeholder or nonexistent internal package.
- Preserve accessibility, keyboard and platform-targeted ergonomics.
- On mobile/WebView, follow the detailed `mobile-webview.md` and `DESIGN_GUIDELINES.md` where applicable.
- On native work, inspect `src-tauri/Cargo.toml`, capabilities, Tauri config and both Rust and JavaScript plugin registration.

## Skills and checks

- Relevant workflows: `react-feature`, `ui-review`, `api-contract-change`, `tauri-native-change`, `tauri-android-release`, `code-clarity`.
- Inspect scripts before lint, typecheck, tests or build.
- Use scoped checks where possible. Separate new failures from inherited failures.
