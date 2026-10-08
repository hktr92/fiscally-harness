# Frontend UI System

Use when changing shared UI components, app-local components, design tokens, icons or Tailwind/Radix/shadcn usage.

## Shared vs App-Local

- Prefer existing project UI primitives over creating near duplicates.
- Put reusable app-neutral primitives in an actual shared UI package, if one exists.
- Put feature-specific composition inside the app that owns it.
- Keep product data, copy and policy out of the generic UI layer.
- Do not invent `@project/ui` or any placeholder package alias as a real dependency.

## Component Patterns

- Follow configured shadcn/Radix idioms: typed props, `data-slot`, `cn` and `class-variance-authority` when installed and relevant.
- Support controlled and uncontrolled patterns only when the component truly needs both.
- Prefer small composable pieces over giant feature-specific primitives.
- Explicit accessible names, keyboard behavior, focus states, disabled states, pressed states and validation feedback.
- Avoid decorative nested cards that bury useful information.

## Styling

- Prefer Tailwind v4 semantic tokens when Tailwind v4 is actually installed.
- Prefer semantic theme classes (`bg-background`, `text-muted-foreground`, `border`) over raw colors in shared components.
- Use raw colors sparingly for intentional one-off visual work.
- Ensure labels inside controls fit or wrap predictably.
- Do not scale font size based solely on viewport width.

## Icons

- Use whichever icon library is installed; `lucide-react` is a reasonable default for UI/system icons.
- Use distinct brand icons only when needed and with correct licenses.
- Give icon-only buttons accessible labels and adequate hit targets.
