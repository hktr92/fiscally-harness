# Frontend API Client Boundaries

Use this rule when changing HTTP clients, DTO types, typed hooks or application API calls.

## Layered Responsibilities

A small app may have a single typed client module. For a larger monorepo, a three-layer model is an optional architecture:

1. **Core transport**: HTTP factory, base URL, serialization, shared errors, cancellation and auth-safe request mechanics.
2. **Domain client**: typed HTTP operations and backend DTO mirrors. This layer must not depend on React.
3. **React bridge**: TanStack Query hooks, stable query keys, cache mutations/invalidation and auth integration.

Do not create package tiers merely to replicate a diagram. Existing naming and package boundaries take precedence. Authentication may need a deliberately separate module.

## Dependency Direction

- UI should normally call React hooks, not assemble transport headers or requests inline.
- React query hooks may call domain clients; domain clients may call shared transport.
- Domain clients must not import React or TanStack Query.
- Auth token injection belongs in one carefully owned layer.
- Keep internal bridge hooks scoped, avoiding overly broad public exports.

## DTO and Query Contracts

- Backend contracts are the source of truth for types. Keep nullable/optional semantics accurate.
- Never invent missing DTO fields when the contract is unclear.
- If NDJSON/SSE is used, centralize framing and heartbeat handling instead of duplicating parsers.
- Query keys must be stable arrays that include all parameters influencing the result.
- Queries requiring an identifier should not fire with missing/empty identifiers.
- Explicitly invalidate or update the affected keys on mutations.
- Use safe error parsing; never reveal tokens, request headers or personal data to the UI or logs.

## Handoff

When updating the API contract, inspect corresponding backend DTOs/endpoints and cover breaking changes with tests where available.
