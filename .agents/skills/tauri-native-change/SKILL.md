---
name: tauri-native-change
description: Implement a Tauri Rust command/plugin/capability change with explicit security and compatibility validation.
---

# tauri-native-change


## Discovery

1. Locate `src-tauri/` and the owning frontend app.
2. Read `frontend/AGENTS.md` plus all relevant rules under `tauri/`.
3. Inspect Tauri and plugin versions, Cargo.toml, Cargo.lock, tauri.conf.json, JS packages and capability files.
4. Identify whether a normal WebView API already suffices; avoid needless native commands.

## Changes

5. Define narrow input/output DTOs for IPC; never leak raw internal errors to UI.
6. Keep OS-facing work in Rust and ordinary product workflows in their existing application owner.
7. Use configured Tauri plugins when suitable; maintain frontend package, Rust registration and capabilities together.
8. Grant least-privilege filesystem/HTTP/OS permissions and review CSP.
9. Preserve browser fallback for shared web code if required.
10. Keep credentials and personally sensitive data out of logs and native configs.
11. Don't edit generated Android project files to solve config bugs.

## Validation

12. Run frontend build and `cargo check`/tests using available toolchains.
13. If mobile builds are requested, check Android/iOS prerequisites and run configured scripts.
14. Report compilation errors separately from SDK/NDK, Gradle or signing blockers.
15. Never claim an emulator or real-device test unless actually performed.
