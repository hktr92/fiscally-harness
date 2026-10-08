# Backend agent instructions

Applies in `backend/`. Start with root `AGENTS.md`; selectively read `../.agents/rules/php/`.

## Architecture

- Symfony is the runtime, not the only application boundary.
- This kit favors `include/` (DTOs/contracts/enums), `lib/` (extractable adapters/parsers) and `src/` (Symfony/Doctrine runtime), but use the established project layout if different.
- `include/` must not depend on `src/` or `lib/`; `lib/` must not depend on `src/`.
- Use owned DTOs/contracts at boundaries. Never leak Doctrine entities as API responses.
- Keep controllers narrow; place app workflows in services/contracts and integrations in adapters.

## Project discovery and safety

- Verify PHP target, Symfony/Doctrine versions, installed bundles and Composer scripts before writing code.
- Don't invent missing bundles, attribute APIs or quality gate scripts.
- Review migrations and potentially destructive SQL before applying.
- Preserve existing uncommitted work and secrets.

## Skills

`symfony-feature`, `api-contract-change`, `doctrine-migration`, `file-import`, `code-clarity`.

Use available tests/static analysis/style checks. Report exact results, failures and environmental blockers.
