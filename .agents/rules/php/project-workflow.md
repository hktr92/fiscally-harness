# PHP Project Workflow

Use this rule before making broad PHP changes, when entering a new PHP project, or when changing dependencies, tests, static analysis, framework code, or PHP versions.

## Project Discovery

- Detect the active PHP version target before choosing syntax.
- Treat `composer.json` and `composer.lock` as the dependency truth.
- Inspect Composer scripts before inventing commands.
- Identify the framework from installed packages, not from directory names alone.
- Identify test tools from installed packages and scripts before adding or running tests.
- Identify static analysis and code style tools from installed packages and scripts before proposing checks.
- If the repository contains multiple PHP projects, scope all commands and dependency reads to the affected project.
- Prefer the quality-gate stack from `.agents/rules/php/backend-validation.md` when the project has not chosen another toolchain.

## Dependency Awareness

- Prefer exact installed versions from `composer.lock` when reasoning about package APIs.
- Use `composer.json` constraints to understand intended compatibility, but do not assume a package is installed unless it appears in the lockfile or vendor metadata.
- When dependency files change, run or recommend the project dependency audit command.
- Do not run `composer update` unless the user explicitly asks for dependency resolution.
- When adding code that requires a missing package, state the Composer package to install before continuing with implementation assumptions.

## Tool Routing

- For Symfony projects, follow Symfony-specific skills and rules before generic PHP guidance.
- For Symfony project orientation, use `symfony-project-guide.md` before default Symfony examples.
- For backend API feature work, use `the project's API contract documentation` and
  `symfony-feature` before editing multiple boundaries.
- For CSV or bank statement import work, use `backend-import-boundaries.md` and
  `file-import`.
- For Doctrine mapping changes, use the Doctrine migration workflow and review generated SQL before applying it.
- For tests, prefer existing project test commands and configuration.
- For static analysis and formatting, use configured project tools only; do not introduce new tools as part of an unrelated change.
- For new Symfony backend projects, prefer Symfony PHPUnit Bridge, Psalm, PHP CS Fixer, and Rector as the default quality-gate stack.
- If a useful check is unavailable, say so explicitly in the handoff instead of pretending it passed.
- If a configured quality gate fails on the changed code, do not hand off as complete until the failure is fixed.
- If a configured quality gate fails for unrelated existing code, separate that from the current change and report the failing command clearly.

## PHP Upgrade Work

- Treat PHP version upgrades and deprecation cleanup as a staged workflow.
- First, detect the current and target PHP versions.
- Next, scan and inventory compatibility issues before editing.
- Categorize issues by risk: breaking changes, removed deprecations, warned deprecations, and style modernizations.
- For PHP 8.4+ modernization, consider named arguments, `match`, enums, readonly classes, typed constants, asymmetric visibility, direct `new` chaining, and property hooks only when compatible with the target framework.
- Ask for confirmation before applying broad or mechanical upgrade fixes.
- Apply fixes in priority order, verify each batch, and keep unrelated style churn out of the upgrade.
- Update Composer PHP constraints only as part of an explicit version upgrade task.

## Documentation Lookup

- Prefer official PHP, Symfony, Symfony AI Mate, Doctrine, Composer, Symfony PHPUnit Bridge, PHPUnit, Psalm, PHP CS Fixer, and Rector documentation when local project files are not enough.
- For API details that vary by package version, verify against the installed version or current official docs.
