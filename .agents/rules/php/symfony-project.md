# Symfony Project Guide

Use this rule when orienting inside a Symfony backend, adding Symfony runtime code, selecting Symfony commands, or deciding which Symfony-specific tool should answer a question.

## Project Shape

- Treat Symfony as the runtime layer, not the whole application boundary.
- Follow the three-directory boundary rule:
  - `include/` contains DTOs, contracts, enums, domain exceptions, and boundary types.
  - `lib/` contains extractable integrations, adapters, parsers, wrappers, and non-app support logic.
  - `src/` contains Symfony runtime code: controllers, services, entities, repositories, commands, event subscribers, voters, fixtures, listeners, and app workflows.
- Do not copy default Symfony examples that place DTOs, contracts, or domain exceptions in `src/` when this pack's boundary rules apply.

## Runtime Directories

- `src/Controller/` contains DTO API controllers and one-route action classes.
- `src/Entity/` contains Doctrine ORM entities.
- `src/Repository/` contains Doctrine repositories.
- `src/Service/` contains concrete service implementations behind `include/Contract/` interfaces.
- `src/Command/` contains Symfony console commands.
- `src/EventSubscriber/` contains event subscribers.
- `src/Security/` contains voters and security runtime code.
- `src/DataFixtures/` contains Doctrine fixtures.
- `src/Twig/` contains Twig extension services.
- `config/` contains framework, bundle, routing, security, package, and environment configuration.
- `migrations/` contains Doctrine migrations.
- `templates/` contains Twig templates when the app renders server-side views.
- `tests/` contains PHPUnit Bridge tests.
- `var/` contains cache and logs and must not be treated as source.

## Service Container

- Prefer Symfony attributes for service definitions when available.
- Use `#[AsAlias]`, `#[AsCommand]`, `#[AsController]`, `#[AsEventListener]`, `#[AsMessage]`, `#[AsMessageHandler]`, `#[AsTwigFilter]`, `#[AsTwigFunction]`, `#[AsTwigTest]`, and `#[Autowire]` before YAML service wiring.
- Keep YAML/PHP config for framework-level configuration, bundle configuration, environment settings, transport config, and parameters.
- Controllers must inject service contracts, not concrete service implementations.
- Concrete services implementing contracts from `include/Contract/` must declare an explicit `#[AsAlias]` because the three-directory boundary can break default service discovery assumptions.

## Routing

- Prefer PHP route attributes on controllers and action classes.
- Keep controllers thin: deserialize DTO request, obtain the current user when needed, delegate to a service contract, and return a response DTO.
- If the project has a DTO API bundle, follow its configured attributes; otherwise use Symfony controllers and its existing serializer/validator conventions.
- For NDJSON or SSE endpoints, inspect the actual streaming conventions and document them before changing API contracts.
- Split large controllers into one-route action classes once they exceed 150 visual lines or accumulate many route actions.
- For end-to-end API feature work, use `the project's API contract documentation` and the
  `symfony-api-feature` skill before stitching together lower-level skills.

## Doctrine

- Keep entities and repositories in `src/`.
- Use Doctrine attributes for mapping.
- Generate migrations from a blank migration class with `bin/console doctrine:migrations:generate`, then write and review SQL deliberately.
- Do not apply generated migrations without reviewing destructive SQL risk.
- Prefer embeddables for repeated field groups such as timestamps.

## Console Commands

- Prefer `#[AsCommand]`.
- Keep commands as thin orchestration around services.
- Use `SymfonyStyle` for input/output.
- Return explicit success and failure status codes.

## Imports

- Keep external file parsing and row normalization in `lib/Import/` when the project has import workflows.
- Use `backend-import-boundaries.md` and the `file-import` skill for CSV
  and provider import handlers.
- Keep commands and application services as orchestration; do not put business policy, database writes, or application orchestration into import adapters.

## Testing

- Prefer Symfony PHPUnit Bridge.
- Use unit tests for pure logic that does not need the container.
- Use `KernelTestCase` for service/container integration.
- Use `WebTestCase` for HTTP request/response behavior.
- Keep tests aligned with existing project fixtures, database setup, and environment configuration.

## Debugging Commands

Use the project equivalent of these commands when relevant:

```bash
bin/console debug:router
bin/console debug:container
bin/console debug:config
bin/console doctrine:schema:validate
bin/console doctrine:migrations:generate
bin/console cache:clear
```

For Docker-based projects, run commands through the PHP container.

## Symfony AI Mate

- Prefer Symfony AI Mate as the Symfony-native MCP tooling layer for agent-assisted development.
- Treat it as a development dependency only.
- If missing and MCP tooling would help, suggest installing it with `composer require --dev symfony/ai-mate`.
- After installation, initialize with `vendor/bin/mate init`, refresh autoloading with `composer dump-autoload`, discover extensions with `vendor/bin/mate discover`, and start the MCP server with `vendor/bin/mate serve`.
- For Symfony-specific MCP tooling, suggest `symfony/ai-symfony-mate-extension` when container introspection or profiler access would materially improve the work.
- Use Mate commands such as `vendor/bin/mate debug:capabilities`, `vendor/bin/mate debug:extensions`, and `vendor/bin/mate mcp:tools:list` to understand available tools.
- Do not expose secrets, cookies, sessions, auth headers, or sensitive environment data through custom MCP tools.
- Keep custom Mate tools in the generated `mate/` area unless the project has a documented alternative.
