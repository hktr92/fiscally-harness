# Rust coding guide
Honor edition/MSRV in Cargo.toml. Use Result for fallible operations, typed serde data at IPC boundaries, and safe errors. Avoid unwrap/expect/panic in production Tauri commands. Minimize dependencies, keep Cargo.lock for app crates and keep OS integration distinct from application workflows.
