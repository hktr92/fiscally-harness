# Backend agent instructions

Apply in `backend/`. Read applicable rules from `../.agents/rules/php/` and `../.agents/rules/general/`.

- Symfony is the runtime; honor the `include/`, `lib/`, `src/` dependency direction when the project adopts this architecture.
- `include/` owns contracts/DTOs/enums/value objects; `lib/` owns extractable integrations; `src/` owns Symfony/Doctrine runtime.
- Keep controllers thin, avoid leaking Doctrine entities through APIs, type boundary contracts explicitly.
- Confirm PHP/Symfony/Doctrine versions and Composer dependencies before choosing syntax or attributes.
- Inspect Composer scripts and installed dev tools before quality checks; no invented commands.
- Review migrations before applying; never run destructive schema changes by default.
- Applicable skills: `symfony-feature`, `doctrine-migration`, `code-clarity`.
