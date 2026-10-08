# Tauri Security and Capabilities

Use when changing capabilities, plugins, CSP, HTTP access, filesystem access or OS-facing permissions.

## Capability Scope

- Grant the minimum permission needed for the feature.
- Scope HTTP access to explicitly known hosts and paths where possible.
- Prefer separate capability entries for independently reviewable native access surfaces.
- Remove unused permissions when removing a plugin or feature.
- Do not copy capabilities from a privileged app into a demo or less trusted app.

## Sensitive Data

- Never log tokens, credentials, request bodies, imported files or sensitive user content from native commands.
- Keep secrets out of `tauri.conf.json`, Rust source, capability JSON, generated Android sources and committed Gradle files.
- Keep keystores, signing passwords and private fixtures out of the repository.
- Treat native debug logs as possible support artifacts.

## Plugin and Command Safety

- Add a native plugin only when the WebView/browser API is inadequate or OS integration is needed.
- Register plugins in Rust and pair them with the matching frontend package/API.
- Keep custom commands small, typed and OS-facing.
- Return safe, typed errors to frontend code.
- Avoid panic paths.

## Review Points

- Review `app.security.csp` whenever broadening remote content or script behavior.
- Review `capabilities/*.json` whenever enabling native plugin functions.
- Review Android/iOS permissions implied by the plugin before release.
- Prefer deny-by-default.
