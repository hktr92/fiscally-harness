# Symfony App Backend Validation

Use these checks after Symfony backend changes. Adapt command wrappers to the repository, such as Docker Compose, local PHP, or CI scripts.

## Standard Checks

- Inspect `composer.json` scripts and installed packages before choosing validation commands.
- Use `symfony-project.md` for Symfony project structure, debugging command selection, and Symfony AI Mate guidance.
- Run a Symfony container/cache check after service, controller, configuration, or dependency changes.
- Review mapping differences and only generate a Doctrine migration when migration tooling is configured and the change needs one.
- Run dependency audit checks when dependency lockfiles change.
- Run available tests for the changed behavior, preferring Symfony PHPUnit Bridge.
- Run configured static analysis, preferring Psalm.
- Run configured style checks, preferring PHP CS Fixer in dry-run mode.
- Run configured Rector checks in dry-run mode when refactoring, modernization, or repeatable pattern enforcement is relevant.
- If a check is not configured or cannot run locally, say that explicitly in the final handoff.
- Do not hand over Symfony backend code as complete while a configured relevant gate is failing on the changed code.

## Suggested Commands

Use the project equivalent of:

```bash
composer validate
bin/console cache:clear
bin/console doctrine:migrations:generate
vendor/bin/simple-phpunit
vendor/bin/psalm --show-info=false
vendor/bin/php-cs-fixer fix --dry-run --diff
vendor/bin/rector process --dry-run
vendor/bin/mate debug:capabilities
composer audit
```

For Docker-based projects, run the same commands through the PHP container.

## Validation Notes

- Do not run formatters that rewrite unrelated files unless formatting is part of the requested work.
- Do not run Rector rewrites across unrelated files unless refactoring or modernization is part of the requested work.
- Do not apply generated migrations without reviewing them.
- If migrations are generated, inspect for destructive SQL before committing or applying them.
- If dependency changes occur, include dependency audit results in the handoff.
- Do not run `composer update` unless the user explicitly asks for dependency resolution.
- When a Symfony component, Doctrine feature, Messenger transport, Twig feature, or DTO API attribute depends on a missing package, suggest the Composer install before assuming the API exists.
- If these tools are missing, explain which checks were skipped. Suggest adding a dev dependency only when it fits the project or the task expressly includes setting up quality gates.
- If Symfony AI Mate is installed and MCP tooling is relevant, use its debug/list commands to verify available capabilities before relying on them.
- If a configured gate fails on unrelated pre-existing code, summarize that separately from the current task and include the failing command.
- If a configured gate fails on code changed by the current task, fix it before final handoff unless the user explicitly asks to pause or accept the failure.
