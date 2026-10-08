---
name: api-contract-change
description: Coordinate backend DTO/endpoint changes with typed frontend clients and React hooks.
---

# api-contract-change


## Scope

Use when a task changes request/response DTOs, auth handling, error formats, streaming data or endpoint routes across Symfony and React.

## Procedure

1. Read `backend/AGENTS.md`, `frontend/AGENTS.md` and both API/dependency rules.
2. Identify the backend source of truth and every typed frontend consumer.
3. Determine compatibility: additive, rename, removal, nullable change, shape change or pagination/streaming change.
4. Update explicit backend contracts and validation first; keep ORM entities private.
5. Update transport/domain clients (where they exist) and query hooks. Preserve auth injection at the configured boundary.
6. Align nullable/optional semantics, error normalization and stable query keys.
7. Update cache invalidation and loading/error states for changed mutations.
8. For SSE/NDJSON, cover framing, empty/heartbeat frames and interruption behavior.

## Validation

9. Run configured backend and frontend tests/typechecking.
10. Verify at least one realistic end-to-end request where a test environment exists.
11. Report breaking changes, required migration/coordinated deploy and tests actually run.
