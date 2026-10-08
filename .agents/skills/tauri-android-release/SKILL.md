---
name: tauri-android-release
description: Audit and produce a Tauri Android release using installed tooling while protecting signing credentials.
---

# tauri-android-release


## Preconditions

1. Read all applicable Tauri rules and the owning app's manifests.
2. Confirm app identifier, target version, Android target, dev/prod API URL and update policy.
3. Check Rust, Android SDK/NDK, Java, Gradle and signing resources without printing secrets.
4. Verify the current Git state and intended release commit/tag.

## Build and QA

5. Run configured frontend build before native packaging.
6. Run the configured Tauri Android build; identify Rust vs SDK vs Gradle vs signing failures.
7. Never modify generated Android files merely to silence a config error.
8. Verify permissions/capabilities/CSP and ensure secrets are not shipped.
9. Smoke-test on emulator/device if available, including routing, keyboard/back and backend networking.
10. Review output artifacts and version alignment.

## Handoff

- Report artifact type/location, hashes if available, actual device coverage and blockers.
- Never commit keystores, build secrets or release binaries unless an explicit distribution policy allows it.
