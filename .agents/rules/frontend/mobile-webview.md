# Frontend Mobile WebView

Use for mobile screens, Tauri/WebView shell behavior, navigation, forms and sticky actions. On desktop-only apps apply the relevant accessibility and responsiveness rules, not mobile-specific layout constraints.

## Layout Foundation

- Prefer `h-dvh` or `min-h-dvh` over `h-screen`/`min-h-screen` for application shells.
- Use `flex flex-col`, `min-h-0 flex-1 overflow-y-auto` and `shrink-0` to give screens stable scrolling regions.
- Main content scrolls; key actions and primary navigation should not be buried at the end of the content.
- Use momentum scrolling in mobile WebViews where needed; avoid nested scroll containers.

## Safe Areas

- Respect `env(safe-area-inset-top)` at top bars and edge-to-edge surfaces.
- Respect `env(safe-area-inset-bottom)` for sticky CTAs, bottom navigation, sheets and footers.
- Never place important controls beneath iOS home indicator, camera cutouts or Android gesture/navigation bars.
- Fixed actions must have matching content padding and remain accessible with the keyboard open.

## Actions and Forms

- Prefer one clear primary action per focused screen/flow.
- The primary CTA should remain reachable without scrolling when it is the main decision.
- Disable or guard submission for invalid states and while saving.
- Provide contextual loading and error text in the application's actual target language.
- Prevent iOS input focus zoom (normally minimum 16px input text).
- Verify keyboard avoidance on real/emulated mobile platforms if available.

## Navigation

- Android system back closes transient UI before navigating away.
- Use native-like screen and sheet transitions without building a separate platform clone.
- Use sufficiently large tap targets (at least 44px), visible pressed/focus states and no hover-only affordances.

## Tauri/WebView Boundary

- Keep business logic in application layers unless native execution is justified.
- Prefer configured Tauri plugins/bridges for OS-specific behavior.
- Confirm network fetch behavior on device, not just browser localhost.
- Keep Vite dev URL/ports aligned with `tauri.conf.json`.
- Consult `../tauri/project-workflow.md` and `../tauri/security-capabilities.md` for native changes.
