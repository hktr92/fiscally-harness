---
name: project-bootstrap
description: Initialize a real Symfony and React/Tauri project underneath the harness without assuming packages are installed.
---

# project-bootstrap


## Determine intent

1. Read the root and relevant nested AGENTS files; confirm which runtime(s) are requested.
2. Inspect working tree for existing code and user changes.
3. Check installed tools and version constraints before generating project skeletons.
4. Choose smallest app layout: one React app before a monorepo, unless multiple apps/packages are already justified.
5. Keep backend Symfony separate from React, and Tauri `src-tauri/` under the intended app.

## Configure deliberately

6. Use official initializers compatible with installed versions; don't blindly pin latest.
7. Establish scripts for dev, lint, typecheck, tests and build only if configured and useful.
8. Apply existing architecture rules selectively rather than generating empty service layers.
9. Ensure environment examples contain fake placeholders, never secrets.
10. Update README with *actual* commands and prerequisites; do not leave invented examples.

## Verify

11. Run the minimum boot/build checks supported by the environment.
12. Report what was generated, what couldn't be tested, and next real feature entrypoints.
