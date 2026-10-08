# PHP Coding Guide

Use this guide for PHP code style decisions across Symfony backend work.

## PHP Version

- Detect the project PHP target from `composer.json`, runtime configuration, or project docs before choosing syntax.
- Check `config.platform.php` in `composer.json` because it can override the effective PHP version used for dependency resolution.
- For a new project targeting modern PHP, PHP 8.4 / 8.5 can be a sensible baseline; do not assume either without checking the configured target.
- Prefer the latest stable PHP language features available to the detected project target.
- Do not write legacy-compatible syntax for older PHP versions unless the project explicitly targets them.
- Chain method calls from `new` directly; do not wrap the `new` expression in unnecessary parentheses.

```php
return new DateTimeImmutable()->format(
    format: 'Y-m-d',
);
```

## Standards Baseline

- Use PSR-12 as the baseline coding style for this pack.
- Keep PHP CS Fixer configured with PSR-12 in mind unless the project explicitly chooses a stricter local variant.
- Use 4-space indentation.
- Use Unix LF line endings.
- Keep `declare(strict_types=1);` as the first statement after `<?php`.
- Use one import per line, grouped by class, function, and constant imports.
- Treat 120 characters as a soft line-length target, not an excuse for unreadable wrapping.

## Type Style

- Use `declare(strict_types=1);` in every PHP file.
- Prefer explicit native types for parameters, properties, and return values.
- Prefer union nullables such as `null|Uuid`, `null|string`, and `null|SomeEntity`.
- Do not use shorthand nullable types such as `?Uuid`, `?string`, or `?SomeEntity`.
- Prefer `null|T` ordering for nullable unions.
- Import PHP functions with `use function ...;` instead of relying on global function discovery.
- If there are multiple related integer or string constants, prefer a PHP enum.
- Prefer `match` for expression-oriented branching over long `switch` blocks.
- Use `never` only for functions that always throw, exit, or otherwise cannot return.
- Use `#[\SensitiveParameter]` for parameters that may contain API keys, passwords, tokens, auth headers, secrets, or sensitive PII.

## Attribute Style

- Attribute calls with arguments must be multi-line.
- Use named arguments in attributes.
- Put one attribute argument per line.
- Use a trailing comma in multi-line attributes.
- Attribute calls without arguments may stay single-line, such as `#[AsController]`.

```php
#[Route(
    path: '/api/some-resources',
    name: 'api.some_resource.create',
    methods: [Request::METHOD_POST],
)]
```

## Call Style

- Prefer named arguments when calling a function, method, constructor, or parent constructor with more than one argument.
- Multi-line calls that pass more than one argument, with one argument per line and a trailing comma.
- Calls with zero or one argument may stay positional and single-line when that is clearer.

```php
return $this->someService->create(
    user: $user,
    request: $request,
);
```

## Object Style

- Prefer constructor property promotion for required object state.
- Prefer `final readonly class` by default.
- Do not make Doctrine entities `final readonly class`; Doctrine proxying and hydration make entities the main exception.
- Do not use property hooks in `readonly` classes or readonly properties.
- Use property hooks sparingly for non-entity classes when they materially reduce boilerplate or enforce a local invariant without hiding work.
- If `final readonly class` is not feasible because the class extends a framework base class, prefer `final class` and make constructor-promoted properties `readonly`.
- Prefer API-level readonly behavior for entities: getters by default, no setters or mutators unless a use case needs them.
- Keep mutation methods explicit and domain-named when mutation is required.
- Prefer composition over inheritance. Extend framework base classes only when the framework contract requires it.
- Use early returns to flatten conditional logic. Avoid deeply nested `if` / `else` blocks.

## Symfony Style

- Controllers must inject service contracts, not concrete service implementations.
- Concrete services that implement contracts from `include/Contract/` must use `#[AsAlias]` with the explicit interface id.
- When a DTO API controller grows beyond 150 visual lines or accumulates many route actions, split it into one-route action classes with `__invoke()`.
- Place one-route action classes in a feature directory such as `src/Controller/Some/SomeCreateAction.php`, `SomeUpdateAction.php`, and `SomeDeleteAction.php`.
- Before adding Symfony code that depends on a component or bundle, confirm the dependency exists in Composer metadata.

## Doctrine Style

- Prefer Doctrine embeddables when duplicate field groups appear across entities.
- Reuse or introduce embeddables for repeated timestamp groups such as `createdAt`, `updatedAt`, `deletedAt`, or similar audit fields.
- Doctrine ORM 3.4+ supports PHP property hooks for backed, non-virtual mapped properties.
- Do not map virtual property hooks with Doctrine columns.
- Do not use property hooks in Doctrine entities by default in this pack; prefer constructor-promoted private state plus getters unless the project explicitly adopts a public-property Doctrine style.
- If property hooks are used in a Doctrine entity, remember that DQL and repository criteria compare against the raw stored value, not the transformed hook value.

## Anti-Patterns

- Do not use `@` error suppression.
- Do not use `eval()`.
- Do not use untyped arrays for structured data when a DTO, enum, typed collection, or value object would make the shape explicit.
- Do not use `global`; inject dependencies.
- Do not use `extract()`.
- Do not suppress exceptions with empty `catch` blocks.
- Do not add `#[AllowDynamicProperties]` except for explicit interoperability with legacy APIs.

## Boundaries

- Keep framework types out of boundary contracts and DTOs unless the contract is explicitly framework-facing.
- Keep Doctrine entities inside the application runtime boundary.
- Prefer DTOs and small value objects at application boundaries.
- If a framework runtime value needs to cross into `include/`, convert it into an owned DTO or value object first.

## Project Tooling

- Inspect configured Composer scripts before inventing validation, test, static analysis, or formatting commands.
- Prefer the project's existing tooling over adding a new tool for an unrelated change.
- If a code style, static analysis, or test runner is not configured, note that explicitly instead of implying it passed.
