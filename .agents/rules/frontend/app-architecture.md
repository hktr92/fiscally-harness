# Frontend Application Architecture

Use this rule for routes, feature folders, app-local components, shells or cross-app work.

## Architecture Discovery

- Inspect the actual app's routing, state, styling and workspace arrangement first.
- The preferred example stack is React + TypeScript with TanStack Router, Query and Form, but do not assume those libraries are installed.
- Avoid importing conventions from unrelated prototype apps or demos.

## Routes and Features

- Keep route modules thin: route definition, loader/guard, then delegation to a feature component.
- Once UI becomes nontrivial (about 30 lines of JSX, entity cards, nested components, charts, tables or complex state), move it to an explicit feature component.
- Move data shaping to typed helpers rather than large render functions.
- One-app components stay app-local. Move UI into a shared package only when genuinely reused or foundational.

## State and Data

- Prefer TanStack Query for server state when configured.
- Prefer the project's chosen form tooling for form state; do not duplicate it in custom state containers.
- Use shared client stores sparingly for cross-feature UI state.
- Use React local state for ephemeral view state.
- Keep transport/integration code out of presentation components; see `api-client-boundaries.md`.

## Navigation and Shells

- Respect the project's web/desktop/mobile targets.
- For mobile/WebView screens, handle safe areas, native back, keyboard and fixed action regions.
- Keep guards at the router boundary when the router supports them.
- Make auth/session assumptions explicit and central.
