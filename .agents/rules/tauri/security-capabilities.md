# Tauri capabilities and security
Use least privilege for filesystem, HTTP, OS integrations and capabilities. Review CSP and allowed origins before expanding access. Keep secrets/keystores/signing files out of Git; don't log tokens or personal data. Return frontend-safe errors from native commands; avoid blanket wildcards.
