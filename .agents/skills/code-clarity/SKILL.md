---
name: code-clarity
description: Review and improve code readability and coupling with optional semantic LSP tooling.
---

# code-clarity

Determine the caller's target and inspect code first. Prefer precise, semantics-preserving refactoring: clearer names, smaller functions, simpler control flow and explicit boundary types. Use agent-lsp for symbol/reference discovery **if available**, otherwise repository search and compiler/test tooling. Avoid mass formatting, style-only churn and speculative abstraction. Validate behavior before and after. State which checks ran.
