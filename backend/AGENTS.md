# Backend agent instructions

Applies in `backend/`. Start with root `AGENTS.md`; selectively read `../.agents/rules/php/`.

## Architecture

- Symfony is the runtime, not the only application boundary.
- The `include/` / `lib/` / `src/` boundary is mandatory, and so is tenant isolation. Read `../.agents/rules/php/coding-boundaries.md` before backend changes; it is the single source for directory roles, dependency direction, contract ownership and tenant rules.

## Project discovery and safety

- Verify PHP target, Symfony/Doctrine versions, installed bundles and Composer scripts before writing code.
- Don't invent missing bundles, attribute APIs or quality gate scripts.
- Review migrations and potentially destructive SQL before applying.
- Preserve existing uncommitted work and secrets.

## Skills

`symfony-feature`, `api-contract-change`, `doctrine-migration`, `file-import`, `code-clarity`.

Use available tests/static analysis/style checks. Report exact results, failures and environmental blockers.
