---
name: doctrine-migration
description: Plan and safely review Doctrine ORM mapping and SQL migrations.
---

# doctrine-migration


## Preparation

1. Read `backend/AGENTS.md`, `php/dependency-boundaries.md` and `php/symfony-project.md`.
2. Inspect Doctrine ORM/DBAL versions, mappings, database platform and migration configuration.
3. Review previous migrations and entities for conventions and backward compatibility.
4. Identify migration risk: dropped columns, nullability, renames, type conversion, locks and backfills.

## Implementation

5. Make mapping changes in the owning runtime layer.
6. Reuse embeddables for repeated field groups when consistent with existing code.
7. If a migration is needed and tooling exists, generate a blank migration or follow project workflow.
8. Write deliberate upgrade SQL and applicable rollback/downgrade behavior. Do not blindly trust diff generation.
9. Confirm indexes, default values, constraints and data migration order.
10. Prefer expand/migrate/contract for changes that require zero-downtime deployments.

## Validation and handoff

- Review SQL line-by-line before applying.
- Run schema validation and migrations against a disposable database when available.
- Do **not** apply migrations against an unknown, shared or production database automatically.
- Document irreversible operations, execution prerequisites, tests and version compatibility.
