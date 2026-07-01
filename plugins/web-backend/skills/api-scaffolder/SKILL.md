---
name: api-scaffolder
description: Scaffolds a new backend API endpoint/route (handler, request/response types, and route registration) following the project's existing framework and conventions. Use when the user asks to create/scaffold/generate a new API endpoint, route, or controller.
---

# API Scaffolder

Generate a new backend API endpoint following the conventions already present in the project.

## Steps

1. Detect the project's stack before writing anything:
   - Check `package.json`/`requirements.txt`/`go.mod`/etc. for the web framework in use (e.g. Express, Fastify, NestJS, FastAPI, Django REST Framework, Gin).
   - Look at an existing route/controller/handler to match file layout, naming, request validation approach, and error-handling style.
   - Check whether routes are registered centrally (a router file) or via decorators/auto-discovery.

2. Ask the user (if not already specified) for:
   - HTTP method and path (e.g. `POST /api/users`).
   - Request/response shape, or infer a minimal one from context if not given.

3. Create the endpoint following the existing pattern:
   - Handler function/class in the location and naming style matching existing endpoints.
   - Request validation using whatever the project already uses (e.g. Zod, Joi, Pydantic, class-validator) — don't introduce a new validation library.
   - Response typing/shape consistent with sibling endpoints.
   - Route registration wired the same way existing routes are registered.

4. Do not invent conventions that aren't already present in the codebase. If this is a brand-new project with no existing endpoints, use a minimal, framework-idiomatic default and say so explicitly.

5. After scaffolding, print the created/modified files and briefly summarize what was generated, including the route path and method.
