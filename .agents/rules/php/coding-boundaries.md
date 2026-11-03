# Coding Boundaries

The backend separates `include/`, `lib/` and `src/`. This is an established requirement of the application. Use this rule to restore and enforce those boundaries, not to reorganise working code.

## Mandatory rules

### Directory responsibilities

- `include/` holds human-owned definitions that give the application its contracts and vocabulary: service contracts and interfaces, request and response DTOs, enums and constants, domain exceptions, and shared boundary value objects.
- `lib/` holds self-contained implementations that could reasonably be extracted or reused outside the Symfony application: integrations and provider clients, importers, parsers and format adapters, encoding and filesystem utilities, reusable algorithms, and infrastructure utilities that don't depend on the application runtime.
- `src/` holds Symfony runtime code and application behaviour: controllers, concrete services, Doctrine entities and repositories, commands, listeners and subscribers, authorization, workflows, business policy, orchestration, dependency injection and framework integrations.

### Dependency direction

| Origin     | `include/` | `lib/`    | `src/`    |
|------------|------------|-----------|-----------|
| `include/` | Allowed    | Forbidden | Forbidden |
| `lib/`     | Allowed    | Allowed   | Forbidden |
| `src/`     | Allowed    | Allowed   | Allowed   |

These directions are permitted. They don't require anyone to create a dependency.

### Boundary rules

- `include/` contains no runtime dependencies, infrastructure implementations, persistence objects or framework service behaviour. Validator and Serializer metadata attributes are allowed where needed.
- Every contract in `include/Contract/` is runtime-independent. Contracts use application-owned types and never expose Symfony Security, HTTP, Doctrine or other runtime implementation objects (`Request`, `Response`, `HeaderBag`, `ParameterBag`, `UploadedFile`, security users or tokens, entities, `EntityManagerInterface`, repositories).
- Runtime-facing internal services belong in `src/`. They may accept framework objects where appropriate, and they don't automatically need an interface in `include/`. This exception covers internal `src/` implementations, never `include/` contracts.
- `lib/` depends on neither the application runtime nor concrete `src/` implementations. It may use `include/` types directly.
- `lib/` contains no application workflows, authorization policy or business decisions, even when their implementation is framework-independent.
- `lib/` must not own application persistence workflows, Doctrine entities, ORM repositories, or business-specific data mutations. Reusable storage clients and filesystem adapters are permitted.
- Entities never escape through public API response contracts.
- Don't add adapters, duplicate DTOs, forwarding interfaces or mapping layers whose only purpose is to avoid a permitted dependency.

### Tenant isolation

The application is multi-tenant. Tenant isolation is a mandatory architectural and security invariant.

- All tenant-owned data stays scoped to the correct tenant.
- Every tenant-scoped operation has an explicit, authoritative tenant context.
- Tenant context is an application-owned immutable `TenantId` value object in `include/Value/`.
- A contract carries the tenant context either as a direct argument or inside an owned request or context object. Don't repeat `TenantId` across several arguments without need.
- A client-supplied `TenantId` is never proof of authorization. Resolve or validate tenant access against the authenticated actor before any tenant-scoped operation.
- Tenant-scoped resource access must verify that the requested resource belongs to the authoritative tenant context. Knowing or authorizing a `TenantId` alone is insufficient. Data-access queries must enforce the same scope, including indirect relationships and bulk operations.
- Isolation applies to reads, writes, updates, deletes, imports, exports, background jobs and asynchronous handlers.
- Enforce isolation in application authorization and data-access boundaries, not only through DTO signatures.
- System-wide operations and global reference data may be unscoped when their purpose requires it, and that must be explicit.
- SysAdmin cross-tenant access is an explicitly authorized exception, never an implicit bypass. The SysAdmin role is not permission to disable tenant isolation globally.

### Contract ownership

- Controllers must depend on application service contracts for business use cases. Framework services and infrastructure utilities may be injected directly where appropriate. Do not introduce interfaces solely to satisfy this rule.
- Contracts and DTOs are human-owned. Reuse existing definitions before proposing new ones.
- Never silently redefine an existing contract to fit an implementation.
- When a genuinely new public boundary is needed, propose its shape before implementing it.

### Reviewing existing code

- Report violations with concrete evidence: file path, relevant code, and the violated architectural rule. Evidence may include forbidden dependencies, misplaced responsibilities, or runtime coupling. Do not classify code as a violation based solely on its location or name.
- Keep violations separate from style preferences and optional improvements.
- Fix with the smallest change that restores the boundary. Preserve public contracts and behaviour unless a change is approved.
- This rule does not authorise reorganising the codebase.

## Recommended practices

- Decide placement by responsibility and coupling, not by class name, complexity, or whether the class imports Symfony. Pure PHP doesn't automatically belong in `lib/`.
  - A reusable CSV parser goes in `lib/`. Its result may use existing `include/` types.
  - The service that validates ownership, selects accounts and persists the imported records goes in `src/`. Its contract and its request and response types go in `include/`.
  - A reusable numerical calculation can go in `lib/`. The application rules that decide how to apply it go in `src/`.
- Judge each third-party package in `include/` by whether it compromises the boundary's independence. A package that brings runtime, HTTP or persistence behaviour does.
- Preserve established subdirectories within `include/`. `Contract/`, `Dto/`, `Enum/`, `Exception/`, `Shared/` and `Value/` are valid organizational boundaries. Do not rename or consolidate existing directories solely for stylistic consistency.
  - `include/Value/` holds immutable domain value objects and domain identities, including `TenantId`.
  - `include/Shared/` holds deliberately shared boundary definitions and generic cross-module types.
- Convert runtime values into an owned DTO or value object before they cross a contract. Headers become an owned type in `include/Shared/`.
- When authenticated identity crosses a runtime-independent contract, pass the minimal required application-owned identity or context type. Runtime-facing services may use framework identity objects where appropriate.
- Controllers receive DTOs, delegate, and return DTOs or framework responses. Services receive DTOs or typed values, never raw HTTP request data.
- A concrete service that implements an `include/Contract/` interface declares `#[AsAlias]` with that interface's id.
- Don't build a generic tenant-management framework unless the existing application requires one.

## Optional design choices

- An interface for a `src/` service that isn't a business use case. Internal implementation details and infrastructure utilities don't need one by default.
- A shared import contract. Add one only when several formats need it.
- Sub-paths such as `lib/Import/`, `lib/Import/<Provider>/` and `lib/Fs/`. These are examples, not required directories.
