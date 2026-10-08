---
name: file-import
description: Add a deterministic file import parser/adapter without coupling it to Symfony persistence or business policy.
---

# file-import


## Discovery

1. Read `backend/AGENTS.md` and `php/backend-import-boundaries.md`.
2. Inspect the actual accepted file format and representative **synthetic** examples.
3. Locate current import registry/contracts, adapters, streams and app orchestration.
4. Identify charset, locale, separators, multi-line fields, repeated headers and error policy.

## Implementation

5. Write pure, typed format detection and parsing functions where practical.
6. Normalize dates, decimals, encodings and optional values at the input boundary.
7. Prefer row-level diagnostics and deterministic output for malformed but recoverable records.
8. Keep filesystem, extraction, business matching, persistence and user confirmation separate.
9. Register a new handler only through the project's actual conventions.
10. Avoid inventing a generic import framework when there is one source format.

## QA

11. Test valid, truncated, empty, repeated-header, escaped, multiline and malformed records relevant to the format.
12. Keep fixtures synthetic and tiny. Do not commit private exports or credentials.
13. Run configured PHP tests and static checks; report unsupported cases.
