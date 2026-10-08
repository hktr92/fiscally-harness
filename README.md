# Fiscally Harness

A product-neutral Codex engineering harness for **Symfony/PHP + React/TanStack/Tailwind/shadcn + optional Tauri/Rust**.

This repository contains agent instructions, detailed technical rules, skills and a file-based issue workflow. It is not an initialized application. Despite the repository name, nothing in the harness requires any Fiscally application or internal package.

## Getting started

```bash
git clone https://github.com/hktr92/fiscally-harness.git
cd fiscally-harness
codex
```

Read the root [AGENTS.md](AGENTS.md), then the nearest `frontend/AGENTS.md`, `backend/AGENTS.md` or `docs/AGENTS.md` for your task. Initialize the application in `backend/` and `frontend/` when needed. Tauri typically lives under the owning frontend app's `src-tauri/`. Adapt dependencies and scripts based on the real project.

## Included

| Location | Purpose |
| --- | --- |
| `AGENTS.md` and nested `AGENTS.md` | Router and local project instructions |
| `.agents/rules/general/` | Discovery, changes and quality checks |
| `.agents/rules/php/` | PHP, Symfony, DTO contracts, Doctrine, import boundaries and validation |
| `.agents/rules/frontend/` | React, API clients, Tailwind/shadcn, WebView and QA |
| `.agents/rules/tauri/` | Rust, Tauri security, native workflow and Android release |
| `.agents/skills/*/SKILL.md` | Detailed, reusable Codex workflows |
| `docs/issues/` | Local Git-versioned issue lifecycle |
| [`frontend/DESIGN_GUIDELINES.md`](frontend/DESIGN_GUIDELINES.md) | Generic iOS/Android WebView UX guidance |
| `docs/architecture/`, `docs/decisions/` | Architecture and ADR documentation |

Rules are context-specific documents. `.agents/rules/` **is not automatically loaded in full**: the relevant AGENTS file tells the agent which rules to read. Skills are discoverable through their SKILL.md metadata.

## Issues

```text
0-draft/      Human-owned investigation and planning
1-new/        Ready with acceptance criteria
2-in-progress/ Claimed, implemented and validated
3-done/       Completed, date-prefixed YYYYMMDD-<id>-<slug>.md
```

The flow uses a claim commit where appropriate, implementation and integration checks, then a completion commit. Leave blocked work in progress. See [docs/AGENTS.md](docs/AGENTS.md), [template](docs/issues/TEMPLATE.md), [example](docs/issues/EXAMPLE.md) and [issue-execution skill](.agents/skills/issue-execution/SKILL.md).

## Skills

`issue-execution`, `symfony-feature`, `doctrine-migration`, `react-feature`, `ui-review`, `tauri-native-change`, `code-clarity`, `file-import`, `api-contract-change`, `tauri-android-release`, `project-bootstrap`.

No mandatory subagents. Small changes should not require lengthy planning or new dependencies. Always discover versions and configured scripts before using tooling.

## Optional Codex integrations

The harness works without external plugins.

| Integration | Status |
| --- | --- |
| `agent-lsp` | Optional, recommended for semantic navigation and refactoring |
| `unslop` | Optional cleanup, not a test substitute |
| `code-clarity` | Included **read-only audit skill**; uses agent-lsp when available and never edits application source |
| `ponytail` | Optional and never required |
| [snowe-ui-skill](https://github.com/What0ff/snowe-ui-skill) | Reference only. Not bundled or installed |
| `superpowers` | Excluded. No mandatory subagent-driven development |

Check each external project's official documentation before installing it.

## Porting status and provenance

The technology rules were restored in detail from the available engineering rule documents, with internal package names, product-specific constraints and domain assumptions generalized. The original [code-clarity](.agents/skills/code-clarity/SKILL.md) skill **was recovered from an earlier source file** and ported with its evidence-driven, read-only audit workflow intact. The Fiscally-specific architecture overlay and domain examples were replaced with generic Symfony/React/Tauri guidance; agent-lsp was made optional for portability. Other skills and the AGENTS files remain **reconstructions**, not verbatim copies: their original source files have not yet been recovered. The original product-specific design guide was adapted into a smaller generic mobile/WebView guide; brand- and finance-specific sections were intentionally omitted.

## License

[MIT](LICENSE), © 2026 hktr92.
