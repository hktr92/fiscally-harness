# Dependency Boundaries

Use this rule to preserve dependency direction in the three-directory backend pattern.

## Direction

- `include/` is owned boundary code.
- `include/` must not depend on `src/`.
- `include/` must not depend on Symfony runtime, Doctrine ORM, HTTP requests/responses, controllers, entity managers, repositories, or service implementations.
- `lib/` must not depend on `src/`.
- `src/` may depend on `include/` and `lib/`.
- `src/` adapts Symfony, Doctrine, Messenger, Twig, Console, Security, and infrastructure concerns to owned contracts and DTOs.

## Include Ownership

- Everything in `include/` must be owned by the application or package.
- Put contracts in `include/Contract/`.
- Put DTOs in `include/Dto/`.
- Put enums in `include/Enum/`.
- Put domain exceptions in `include/Exception/`.
- Put shared boundary value objects in `include/Shared/`.

## Runtime Types

- Do not pass Symfony `Request`, `Response`, `HeaderBag`, `ParameterBag`, `UploadedFile`, Doctrine entities, `EntityManagerInterface`, or repositories through owned boundary contracts.
- If a runtime value is needed at a boundary, extract and normalize it into an owned DTO or value object first.
- For headers, prefer an owned value object such as `include/Shared/SomeHeader.php` instead of passing the request object.
- For authenticated users, pass an owned identity DTO/value object or a narrow app contract unless the service is explicitly runtime-facing.

## Contracts

- Contracts describe use cases in application language.
- Contracts accept request DTOs, value objects, enums, scalar identifiers, and safe context objects.
- Contracts return response DTOs, value objects, enums, or explicit result objects.
- Contracts must not leak persistence, HTTP, or framework implementation details.
