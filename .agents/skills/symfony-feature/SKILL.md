---
name: symfony-feature
description: Implement a Symfony service, controller or backend use case while preserving contracts, dependency direction and tests.
---

# symfony-feature


## Discovery

1. Read `backend/AGENTS.md` and rules in `php/` relevant to the change.
2. Inspect `composer.json`, `composer.lock`, PHP target, Symfony version and existing feature conventions.
3. Search for the nearest similar feature before inventing a new pattern.
4. Identify which public contract, runtime implementation and integration layers are affected.

## Design

- Follow `include/` → `lib/` → `src/` direction when used by the project.
- `include/Dto/` defines request/response shapes; `include/Contract/` defines use cases.
- `src/` owns Symfony controllers, concrete services, Doctrine repositories, security and commands.
- `lib/` is for extractable integrations and parsers, not persistence or application decisions.
- Controllers validate/map input, call a service/contract and return typed output.
- Don't leak Doctrine entities, Symfony Request/Response or EntityManager through owned DTO contracts.
- Keep authorization at the correct boundary and failures explicit.

## Implementation

5. Add or amend DTO/contract first when the feature crosses an API boundary.
6. Implement the smallest service/runtime change and wire dependencies using existing conventions.
7. Add actions/routes only after confirming installed route/serializer/bundle APIs.
8. Add unit tests for pure logic; kernel/http tests for actual container and endpoint behavior.
9. Review potential migration effects separately with `doctrine-migration`.

## Validation

- Inspect available Composer scripts and installed QA tools; do not invent commands.
- Run relevant container, static analysis, style, test and route checks if configured.
- Report exact checks/results, unresolved behavior and whether contracts changed.
