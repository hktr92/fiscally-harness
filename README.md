# Fiscally Harness

An MIT-licensed, **product-neutral Codex starter harness** for projects using Symfony/PHP, React/TanStack/Tailwind/shadcn, and Tauri/Rust.

This repository contains **instructions, rules, skills, and an issue workflow**, not an initialized application. It is inspired by the engineering workflow developed while building Fiscally, but **does not import any Fiscally application code, branding rules, data, or internal packages**.

## Quick start

1. Use this repository as a template or clone it and change the project name.
2. Read [AGENTS.md](AGENTS.md), [frontend/AGENTS.md](frontend/AGENTS.md), [backend/AGENTS.md](backend/AGENTS.md), and [docs/AGENTS.md](docs/AGENTS.md).
3. Initialize your own Symfony app under `backend/` and React workspace under `frontend/`. Put Tauri under the relevant frontend app's `src-tauri/`.
4. Adjust project-specific decisions (package manager, PHP target, naming, platform targets) in local AGENTS documents. **Do not invent configured commands or dependencies**.
5. Write work items in `docs/issues/0-draft/`; move ready, bounded work to `1-new/`.
6. Open Codex from the repository root and ask it to implement one ready issue using the [issue-execution skill](.agents/skills/issue-execution/SKILL.md).

Example prompt:

```text
Read AGENTS.md and docs/AGENTS.md. Execute docs/issues/1-new/0001-example-endpoint.md.
Inspect existing code before editing. Move the issue through the documented lifecycle,
run relevant available checks, commit only your own work, and report the commit SHA.
```

The included example is a **template**, not a task to run before an application exists.

## Repository map

| Path | Responsibility |
| --- | --- |
| `AGENTS.md` | Root routing, repository-wide discipline |
| `.agents/rules/` | Contextual technical constraints (read via AGENTS routing; not implicitly auto-loaded) |
| `.agents/skills/*/SKILL.md` | Repeatable workflows that Codex can discover |
| `frontend/AGENTS.md` | React/TanStack/UI and Tauri development |
| `backend/AGENTS.md` | Symfony/PHP/Doctrine conventions |
| `docs/AGENTS.md` | Docs authority and issue lifecycle |
| `docs/issues/` | Draft → ready → in progress → done |
| `docs/architecture/` | Architecture decisions and diagrams |
| `docs/decisions/` | Short ADRs |

**Rules vs skills:** a rule states a constraint (e.g. dependency direction); a skill explains *how* to perform a task (e.g. change an API contract). Read only the rules and skills relevant to current work.

## Issue lifecycle

`0-draft` for exploration; `1-new` for actionable issues; `2-in-progress` for active implementation; `3-done` only after acceptance criteria and relevant checks are satisfied. See [docs/AGENTS.md](docs/AGENTS.md) and [issue template](docs/issues/TEMPLATE.md). The workflow is file-based; it does not automatically synchronize GitHub Issues.

## Stack assumptions

React, TypeScript, TanStack Router/Query/Form, Tailwind CSS v4, shadcn/Radix and pnpm are **preferred examples**, not unconditional dependencies. Symfony, Doctrine, PHP tooling and Tauri/Rust conventions are likewise conditional on what the project actually installs. Check manifests and lockfiles first. A single React app is fine; an `apps/` + `packages/` workspace is optional.

## Optional Codex integrations

The harness runs without any external Codex plugin:

| Integration | Policy |
| --- | --- |
| `agent-lsp` | Recommended optional semantic navigation/refactor tool, when available |
| `unslop` | Optional cleanup pass; never a replacement for tests or review |
| `code-clarity` | **Included skill**, works without agent-lsp (uses it only if present) |
| `ponytail` | Optional personal preference; not a dependency |
| [snowe-ui-skill](https://github.com/What0ff/snowe-ui-skill) | Worth reviewing for UI audit ideas; **not bundled or installed** |
| `superpowers` | **Excluded**. No mandatory subagent-driven development or verbose ritual workflow |

Do not assume these integrations are available; verify them in the running Codex environment. Install instructions and upstream compatibility must come from each project's official repository.

## Design principles

- Minimum ceremony for small tasks; plan only when work warrants it.
- No forced subagents, gratuitous dependency layers, or new packages without justification.
- No fabricated validation results or claims that a check ran when it did not.
- Keep changes scoped; preserve uncommitted user-owned changes.
- Keep credentials, private datasets, keys, signing material, and secrets outside Git.
- Commit one coherent issue at a time, with a useful handoff.

## License

[MIT](LICENSE) © 2026 hktr92.
