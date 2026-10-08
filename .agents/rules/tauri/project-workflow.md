# Tauri Project Workflow

Use before changing `src-tauri/`, native dependencies, configuration or mobile build behavior.

## Project Discovery

- Locate the real Tauri app. It may be `frontend/src-tauri/` or `frontend/apps/<app>/src-tauri/`.
- Inspect owning package.json, Vite config, Cargo.toml, Cargo.lock, tauri.conf.json and capabilities.
- Verify installed Tauri/Rust/plugin/JS versions before using version-specific APIs.
- Keep Vite dev host and port aligned with Tauri `build.devUrl` and scripts.
- Don't edit `src-tauri/gen/android/` unless explicitly required.

## Native Boundary

- Keep Tauri commands small and focused on OS-facing responsibilities.
- Prefer existing plugins/configuration over a custom Rust command for trivial OS integration.
- Preserve a browser fallback for web-targeted features when the project needs one.
- Do not introduce a native capability merely to expose code already available safely in the WebView.

## Change Discipline

- Update Rust dependencies, JS dependencies, plugin registration and capabilities together.
- Commit Cargo.lock for app crates.
- Avoid copied configuration between apps that don't need identical permissions.
- Preserve uncommitted generated/native changes made by others.

## Quality Gates

- Use actual configured frontend scripts plus relevant `cargo check`, `cargo test` and platform builds.
- Build the web frontend before diagnosing native packaging problems.
- Report missing Android SDK/NDK/Gradle/emulator/signing resources separately.
