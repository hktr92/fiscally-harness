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

- Follow `php/coding-boundaries.md`: the mandatory `include/` / `lib/` / `src/` boundary, contract ownership and tenant isolation.
- Controllers validate/map input, call a service/contract and return typed output.
- Keep authorization at the correct boundary and failures explicit.

## Implementation

5. Reuse existing DTOs/contracts first. When the feature needs a new or changed public boundary, propose its shape before implementing it.
6. Implement the smallest service/runtime change and wire dependencies using existing conventions.
7. Add actions/routes only after confirming installed route/serializer/bundle APIs.
8. Add unit tests for pure logic; kernel/http tests for actual container and endpoint behavior.
9. Review potential migration effects separately with `doctrine-migration`.

## Validation

- Inspect available Composer scripts and installed QA tools; do not invent commands.
- Run relevant container, static analysis, style, test and route checks if configured.
- Report exact checks/results, unresolved behavior and whether contracts changed.
