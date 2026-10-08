# Frontend agent instructions

Apply in `frontend/`. Before changes, read the relevant rules from `../.agents/rules/frontend/`, `../.agents/rules/tauri/`, and general rules.

- Discover whether this is a single React app or pnpm workspace from package manifests. Do not impose `apps/` and `packages/` unnecessarily.
- Prefer TypeScript, reusable components, TanStack ecosystem, Tailwind/shadcn conventions where already chosen.
- Keep route definitions thin; place nontrivial screens and data logic in feature modules.
- Server state belongs in TanStack Query when present; don't duplicate it in ad hoc global state.
- Prefer the project's configured component system. Never import nonexistent `@project/ui` or any other hypothetical package.
- Use accessible labels, focus states, safe areas and mobile/WebView behavior where relevant.
- Check actual package scripts before running lint, typecheck, tests or build.
- For Tauri work inspect Cargo.toml, tauri.conf.json, capability files and frontend plugin dependencies together.
- Applicable skills: `react-feature`, `ui-review`, `tauri-native-change`, `code-clarity`.
