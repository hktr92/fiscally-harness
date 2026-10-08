# Coding Boundaries

Use the three-directory boundary pattern for Symfony backend work.

## Directories

- `include/` contains application boundary code: DTOs, service contracts, enums, domain exceptions, and shared boundary value objects.
- `lib/` contains extractable code: integrations, adapters, parsers, provider clients, filesystem helpers, and other code that could become a standalone package.
- `src/` contains Symfony runtime code: controllers, services, Doctrine entities, repositories, commands, event listeners, fixtures, and app workflows.

## Dependency Flow

- `include/` must not depend on `src/` or `lib/`.
- `lib/` may depend on `include/`, but must not depend on `src/`.
- `src/` may depend on `include/` and `lib/`.

## Boundary Rules

- Contracts, request DTOs, response DTOs, enums, and domain exceptions belong in `include/`.
- Symfony controllers, concrete service implementations, Doctrine entities, repositories, commands, and listeners belong in `src/`.
- Extractable integrations and parsing code belong in `lib/`.
- Entities never leave the `src/` application boundary as API responses.
- Controllers receive DTOs, delegate to contracts or services, and return DTOs or framework responses.
- Services receive DTOs or typed values, not raw HTTP request data.
- Do not invent missing boundary contracts or DTOs silently when the project treats them as human-owned. Pause and ask for the missing contract shape.
