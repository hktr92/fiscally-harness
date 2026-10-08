---
name: code-clarity
description: Improve code comprehensibility and refactoring safety using agent-lsp if available, otherwise ordinary symbol searches and tests.
---

# code-clarity


## Purpose

Find the least invasive changes that clarify intent while preserving semantics. A readability review is not permission for mass rewrite.

## Investigation

1. Establish exact scope and known pain point.
2. Read neighboring code, contracts and tests; follow existing naming conventions.
3. Inspect call sites before renaming public symbols or changing signatures.
4. Use `agent-lsp` for references/type navigation **only if installed and available**.
5. Fall back to repository search, language server, compiler, static analyzer and test runner when agent-lsp is absent.

## Changes

- Prefer explicit names, straightforward control flow and typed boundary objects.
- Remove redundant branching or duplications only after understanding behavior.
- Avoid speculative abstractions, new services/packages or broad formatting churn.
- Keep API/backward compatibility intentional; capture migration risks.
- Treat generated code and external/public interfaces carefully.

## Verification

6. Run configured checks before and after when feasible.
7. For structural changes, test the changed behavior, not just compilation.
8. Report before/after responsibility, actual validation and any remaining uncertainty.
