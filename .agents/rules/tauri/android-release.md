# Android release discipline
Verify version, app identifier, signing requirements and installed toolchain before building. Keep keystores, signing passwords and Play credentials out of Git. Check frontend build first, then Rust/Gradle/SDK steps; distinguish errors by layer. Do not claim emulator or device testing without executing it. Review capabilities before release.
