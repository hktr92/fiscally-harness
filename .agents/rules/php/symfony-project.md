# Symfony conventions
Symfony runs in `src/` (controllers, services, repositories, events, commands). Use framework attributes when available; inject explicit service interfaces where adopted. Controllers parse/validate input and delegate to services; use DTOs at API boundaries. Check installed bundles before using bundle-specific attributes. Keep Doctrine migrations deliberate and review SQL.
