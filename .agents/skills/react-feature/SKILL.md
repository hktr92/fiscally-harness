---
name: react-feature
description: Implement a React/TanStack feature using the existing project's routing, API and UI conventions.
---

# react-feature


## Discovery

1. Read `frontend/AGENTS.md` and relevant frontend/Tauri rules.
2. Inspect actual package scripts, installed React/TanStack/Tailwind versions and router/feature patterns.
3. Find existing UI components, hooks, query key factories and types before introducing new modules.
4. Determine platform: browser, desktop Tauri, Android/iOS WebView, or combination.

## Build

5. Keep route module small, delegating nontrivial JSX or domain state to feature components.
6. Use typed backend DTOs and the established transport/query bridge. Don't copy HTTP calls into components.
7. Treat server state, form state and local ephemeral state as separate concerns.
8. Provide meaningful loading, empty, error and retry states.
9. Ensure keyboard access, accessible names, focus visibility, contrast and stable tab order.
10. For mobile layouts, respect safe area, keyboard avoidance, touch targets and native back behavior.
11. Reuse existing shared UI primitives; don't create a hypothetical `@app/ui` package.

## Validate

12. Add the narrowest meaningful test for behavior and regressions.
13. Inspect scripts, then run scoped lint/typecheck/test/build. For Tauri changes follow native skill.
14. Check generated route artifacts only if router tooling has to update them.
15. Document decisions, actual checks and anything not device-tested.
