# Mobile and WebView Design Guidelines

A product-neutral adaptation of React + Tauri UX guidance for Android/iOS WebView surfaces. Apply these rules only if the application actually targets mobile. This document does not establish product branding, locale, navigation destinations or component package names.

## Platform philosophy

- Keep one coherent design language, with platform-aware navigation, gestures, sheets and native system integration.
- For Tauri mobile, build an app-like interaction model rather than a desktop webpage embedded in a WebView.
- Prefer native Tauri platform APIs as the source of truth for runtime capabilities; browser user-agent is only a development fallback.
- Centralize platform capabilities in runtime infrastructure instead of repeating `platform` props on individual screens.
- Permit test-level platform overrides for Storybook and visual QA only.

## Component ownership

- Reuse the actual project's shadcn/Radix primitives or an existing shared UI package.
- Do not create placeholder imports to nonexistent packages.
- Shared controls should be app-neutral, accessible and theme-token aware.
- Product-specific screens own their workflows, text and data.
- Keep meaningful variants small; avoid wrappers that only rename existing components.

## Layout

- Prefer `min-h-dvh` and explicit flex/scroll regions for mobile app shells.
- Use `min-h-0 flex-1 overflow-y-auto` inside flex screens with fixed headers and footers.
- Avoid hardcoded viewport pixel heights and stacked nested scroll containers.
- Main content scrolls; important actions and bottom navigation remain reachable.
- Give scroll regions enough bottom padding when a fixed surface overlays them.
- Use momentum scrolling where the WebView needs it.

## Insets and safe areas

- Apply `env(safe-area-inset-top)` at top shell boundaries.
- Apply `env(safe-area-inset-bottom)` at bottom navigation, sticky action areas and sheets.
- Protect controls from iOS home indicator, camera cutouts and Android navigation/gesture zones.
- Handle Android edge-to-edge explicitly; don't assume opaque system bars.
- Don't blindly add safe-area padding to every child.
- Test visible content and actions with gesture navigation and alternate Android navigation settings.

## Primary actions

- A focused screen should have a clear primary decision and action.
- For linear flows, keep the primary action visible without scrolling whenever practical.
- Use a sticky action region only when it improves a real workflow.
- Disabled, pending, success and error states must be obvious.
- Actions should name what they do; don't label consequential steps only `OK`.
- Never allow a fixed action to cover important scroll content or the on-screen keyboard.

## Navigation

- Top-level destinations may use bottom tabs, but don't invent extra tabs to fill a design pattern.
- Detail routes should have predictable return behavior.
- Android system back closes transient sheets/dialogs before leaving a route.
- Don't create history entries for every tiny filter or UI toggle.
- Confirm destructive exits and loss of unsaved changes where applicable.
- Keep route guards close to the routing layer and session assumptions explicit.

## iOS-specific behavior

- Keep navigation surfaces compact and stable, with home-indicator clearance.
- Avoid heavy elevations and Android-style floating actions when they conflict with iOS expectations.
- Focused inputs should scroll above the keyboard.
- Use at least 16px font size for iOS input text unless testing establishes a safer alternative.
- Preserve predictable sheet dismissal, momentum scrolling and keyboard focus.
- Avoid hover-only affordances.

## Android-specific behavior

- Respect modern edge-to-edge insets and system-bar contrast.
- Use comfortable touch targets: at least 44px, often 48px.
- Press feedback should be immediate and restrained.
- Make native back/gesture behavior consistent with sheets and routing.
- Avoid controls at gesture edges and ensure fixed bars do not obscure them.

## Forms

- Preserve user-entered values after recoverable validation errors.
- Validate on blur/submit when appropriate; avoid aggressive mid-typing resets.
- Use contextual loading labels in the application's chosen language.
- Don't surface raw HTTP payloads, stack traces or infrastructure errors as user copy.
- Choose appropriate `inputMode` and numeric parsing for locale-sensitive data.
- Keep the submit action available when the keyboard opens.

## Interaction and accessibility

- Every interactive control has accessible name, focus state, keyboard behavior and disabled/selected state as applicable.
- Selected state must not rely on color alone.
- Make selection cards fully interactive with correct semantics.
- Don't rely on hover to expose the only path to an action.
- Use haptics selectively through a runtime behavior abstraction.
- Screen-reader announcement, focus management and tab order must survive responsive layouts.

## Visual hierarchy

- Put user intent and content before decoration.
- Prefer semantic Tailwind tokens and coherent spacing/typography scales.
- Avoid nested decorative cards, heavy shadows, noisy gradients and excessive glass effects.
- Blur/translucency is acceptable only when text remains readable and rendering remains fast.
- Keep important numeric data readable; use tabular numbers where appropriate.
- Make light/dark mode contrast intentional.

## State handling

- Show clear loading, empty, success and error states.
- Give users a meaningful retry path when something is recoverable.
- Keep the layout stable when loading transitions to content.
- Avoid duplicate submission during pending async operations.
- Handle offline/degraded states if the target product requires them.

## WebView performance

- Avoid large blurred surfaces over long, constantly scrolling content.
- Prefer transform/opacity animations to layout-recalculating transitions.
- Virtualize long lists only when measurement justifies it.
- Keep animation short and purposeful.
- Profile on the actual WebView target rather than relying only on desktop browser speed.

## Final mobile UX checks

- [ ] Safe areas and fixed controls are correct.
- [ ] Inputs and actions remain accessible with the keyboard open.
- [ ] Android back closes transient UI correctly.
- [ ] iOS dismissal and home-indicator spacing are predictable.
- [ ] Touch targets, focus indicators and labels are accessible.
- [ ] Primary actions and error states remain obvious.
- [ ] Translucent surfaces preserve readability.
- [ ] Performance is acceptable on target devices, or unavailable device coverage is documented.

When auditing, report concrete defects, proposed scoped fixes, validation commands and what was or wasn't device-tested. Never claim visual/device validation without actually performing it.
