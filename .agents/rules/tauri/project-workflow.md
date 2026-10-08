# Tauri project workflow
Verify app scripts, Cargo.toml, tauri.conf.json and capability files before edits. Keep Vite dev URL/port aligned with Tauri config. Prefer existing plugins for native integration. Avoid editing generated Android files except for tasks explicitly requiring it. When plugins change, update Rust registration, JS packages and capabilities together.
