# Symfony coding boundaries
When adopting the three-root pattern: `include/` contains application-owned DTOs, contracts, enums, exceptions and boundary value objects; `lib/` contains extractable adapters and integrations; `src/` contains Symfony runtime (controllers, services, Doctrine, commands). Keep entities internal to runtime.
