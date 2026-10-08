# Backend File Import Boundaries

Use this rule for external CSV, spreadsheet, image, document, or provider data imports when a project includes them. These are **optional patterns**, not requirements to build an import framework.

## Directory Roles

- `lib/Import/` contains extractable parsers, format detectors, and provider adapters.
- `lib/Fs/` may contain reusable filesystem, path, and stream helpers.
- `src/Command/` and `src/Service/` orchestrate importing and persistence in Symfony.
- `include/` contains owned contracts or DTOs when import results cross an application boundary.

## Handlers and Detection

- Define a project-owned import contract when multiple formats require one; do not invent it for a single small parser.
- Keep provider-specific parsing under `lib/Import/<Provider>/` or `lib/Import/Handler/`.
- Handler support detection should be deterministic, based on headers, signatures, or structured layout.
- Tolerate ordinary noise only when the external format has documented irregularities.
- Symfony tags or discovery mechanisms must match the installed framework and the project's actual service configuration.

## Parsing Rules

- Make parser helpers pure and deterministic where practical.
- Normalize dates, decimal separators, locale, encodings and optional fields at the boundary.
- Use explicit numeric/decimal types appropriate for the application; do not use floating point for exact-value domains such as currency.
- Reject truly uninterpretable files with useful typed errors; skip malformed rows only by documented policy.
- Do not hide business categorization, entity lookups or persistence inside format adapters.

## Validation

- Add unit tests with representative synthetic fixtures.
- Include support-detection tests and malformed-row cases.
- Never commit real customer exports, private records, or credentials as fixtures.
