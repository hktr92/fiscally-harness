---
name: tauri-native-change
description: Make a safe Tauri/Rust native integration or capability change.
---

# tauri-native-change

Read `frontend/AGENTS.md` and Tauri rules. Inspect actual Tauri/plugin versions and platform targets. Identify needed native boundary, Rust command/plugin, JS bridge, capabilities and CSP. Grant minimal permissions. Avoid secrets in logs and configs; protect browser fallback if needed. Run configured frontend build and cargo checks; perform mobile build only when toolchain exists.
