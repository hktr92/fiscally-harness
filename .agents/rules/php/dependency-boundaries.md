# Dependency direction
`include/` depends on neither `src/` nor `lib/`; `lib/` may depend on `include/` but not `src/`; `src/` may depend on both. Do not pass Symfony Request/Response, EntityManager or Doctrine entities through boundary contracts; normalize to owned types.
