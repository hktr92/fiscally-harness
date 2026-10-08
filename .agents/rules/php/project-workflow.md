# PHP Project Workflow

Use this rule before broad PHP changes, when entering a new PHP project, or when changing dependencies, tests, static analysis, framework code, or PHP versions.

## Project Discovery

- Detect the active PHP version target before choosing syntax.
- Treat `composer.json` and `composer.lock` as the dependency truth.
- Inspect Composer scripts before inventing commands.
- Identify the framework from installed packages, not directory names alone.
- Identify test tools and static analysis/code style tools from installed packages and scripts.
- If multiple PHP projects exist, scope all dependency reads and commands to the affected project.
- Prefer `backend-validation.md` as the default Symfony validation checklist when the project has not selected another.

## Dependency Awareness

- Prefer exact installed versions from `composer.lock` when reasoning about package APIs.
- Use `composer.json` constraints for intended compatibility; don't assume dependencies are installed.
- When dependencies change, run or recommend the configured security audit.
- Don't run `composer update` without an explicit request for dependency resolution.
- When code needs a missing package, identify the package before making implementation assumptions.

## Tool Routing

- For Symfony work read `symfony-project.md` before generic snippets.
- For API features, use the project's API contract docs and `symfony-feature` / `api-contract-change` skills.
- For file imports read `backend-import-boundaries.md` and consider `file-import`.
- For Doctrine changes, use `doctrine-migration` and review generated SQL before applying.
- Prefer existing tests/tooling over installing more tools for an unrelated change.
- For new Symfony projects, Symfony PHPUnit Bridge, Psalm, PHP CS Fixer and Rector are reasonable QA options, not installed dependencies until configured.
- Report unavailable checks and keep new failures from being treated as completed work.

## PHP Upgrades

1. Inspect current and target versions.
2. Inventory compatibility, removed APIs, deprecations and modernizations.
3. Prioritize breaking issues before style changes.
4. Consider `match`, enums, readonly classes, asymmetric visibility and property hooks only when compatible with PHP/framework versions.
5. Request explicit authorization before sweeping mechanical codebase changes.
6. Apply narrow batches and verify each one.
7. Change Composer PHP targets only in a deliberate upgrade.

## Documentation

Consult official PHP, Symfony, Doctrine, Composer and installed tool docs for version-sensitive details when local metadata is not enough.
