# Tauri Android Release

Use for Android builds, app identification, signing, versioning and distribution.

## Scope and Identity

- Inspect the application's declared platform support and release target; do not assume Android-first.
- Keep `productName`, `version` and `identifier` in `tauri.conf.json` deliberate.
- Align package versions, tags, artifacts and native identifiers.
- Do not change an identifier casually; installed upgrade continuity depends on it.

## Signing and Secrets

- Never commit keystores, signing passwords, Play credentials or signing property files.
- Prefer environment variables or secure CI secret stores.
- Document secret variable names, never their values.
- Treat APK/AAB as artifacts, not source files.

## Build Workflow

- Use actual project scripts for `tauri android dev` / `tauri android build`.
- Validate the web frontend before debugging native package failures.
- Distinguish failures in frontend compilation, Rust, SDK/NDK, Gradle, packaging and signing.
- Don't hand-edit generated Android files to conceal configuration problems.

## Release Checks

- Verify launch on a real device/emulator when available; explicitly report when unavailable.
- Verify backend URL behavior from the device rather than assuming host localhost works.
- Check native logs for credential or user-data leaks.
- Review capabilities and Android permissions before distributing builds.
