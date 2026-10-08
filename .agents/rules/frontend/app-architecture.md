# React app architecture
Keep TanStack Router route files thin and delegate nontrivial UI to feature components. Prefer TanStack Query for server state, form tooling configured in the project, and local state for ephemeral UI. Reuse shared components when shared components actually exist; do not invent package aliases.
