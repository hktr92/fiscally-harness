# 0001: Example endpoint (documentation only)

Status: new

## Context
This file demonstrates an actionable issue. The repository does **not** ship an initialized Symfony application.

## Objective
Once an actual backend exists, add a small health/status endpoint matching that backend's real routing and API conventions.

## Scope
- In scope: one read-only endpoint and matching tests if the project has a test runner.
- Out of scope: authentication flows, databases, monitoring stacks, frontend integration.

## Constraints
Inspect installed Symfony version and existing routing first. Do not introduce a third-party response bundle solely for this example.

## Acceptance criteria
- [ ] Endpoint responds with a stable status payload using current project conventions.
- [ ] Any configured route/container checks pass.
- [ ] Test coverage uses the project's existing tooling, if available.

## Validation
Discover actual commands from `composer.json` and Symfony configuration. Document executed checks.

## Handoff
Not implemented. This is a sample issue; replace/delete when bootstrapping a real app.
