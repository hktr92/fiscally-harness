---
name: ui-review
description: Audit React UI ergonomics, accessibility, responsive layout and WebView-native behavior without changing product identity.
---

# ui-review


## Scope

Identify the affected screens and platform targets. Read existing tokens, UI primitives, styles and the optional `frontend/DESIGN_GUIDELINES.md`; do not redesign unrelated screens.

## Audit rubric

- Hierarchy: user can identify purpose, primary decision and primary action.
- Consistency: use existing tokens and shared primitives; avoid decorative card stacking.
- Input: controls stay reachable under the keyboard and have readable labels.
- Accessibility: semantic controls, screen-reader names, focus, keyboard, contrast, disabled/selected/error states.
- Mobile: touch targets >=44px, safe areas, dynamic viewport, scrolling and fixed-surface padding.
- Navigation: predictable back/close, clean modal dismissal, no history pollution from trivial toggles.
- Feedback: responsive interactions, progress where meaningful, stable loading and actionable errors.
- Performance: no indiscriminate blur/shadows over long scrolling regions, no expensive effects without reason.

## Output and implementation

1. Rank findings by severity and affected users.
2. Show concrete changed files and proposed minimal repairs.
3. Implement only in-scope changes, preserving design language.
4. Run configured frontend gates; perform actual visual/device review only when a browser/device is available.
5. Record what you inspected and what remains an assumption.

This skill does **not** include or automatically invoke snowe-ui-skill.
