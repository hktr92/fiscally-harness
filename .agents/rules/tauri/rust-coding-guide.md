# Rust Coding Guide

Use for Rust under `src-tauri/` or related native crates.

## Style

- Respect edition and MSRV declared in Cargo.toml.
- Keep modules focused on native responsibilities; avoid unexplained package layers.
- Use `serde` DTOs at IPC, plugin and config boundaries.
- Prefer typed structs over arbitrary `serde_json::Value` except explicit passthrough diagnostics.
- Keep public functions narrow and testable.

## Errors

- Return `Result<T, E>` from fallible paths.
- Avoid `unwrap`, `expect` or panic paths in Tauri commands and OS-facing code.
- Map internal errors to typed, frontend-safe outcomes at IPC.
- Do not leak paths, tokens, secrets or sensitive data through errors or logs.

## Native Boundaries

- Use Rust for OS/native integration where Tauri is involved.
- Keep business logic in its deliberate owner (frontend/backend/native crate); do not move it across runtime boundaries opportunistically.
- If significant domain logic genuinely belongs on-device, propose a separate testable crate and explicit API instead of growing `src-tauri/src/lib.rs`.

## Dependencies

- Inspect installed crates before adding more.
- Introduce dependencies when they reduce complexity or risk.
- Commit Cargo.lock for applications.
- Keep native integration and data models typed and compatibility-aware.
