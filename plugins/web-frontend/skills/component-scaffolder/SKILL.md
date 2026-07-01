---
name: component-scaffolder
description: Scaffolds a new frontend component (React by default) with a standard file layout — component file, styles, and optional test file. Use when the user asks to create/scaffold/generate a new component, page, or UI element from scratch.
---

# Component Scaffolder

Generate a new frontend component following the conventions already present in the project.

## Steps

1. Detect the project's stack before writing anything:
   - Check `package.json` for `react`, `vue`, `svelte`, etc.
   - Check for TypeScript (`tsconfig.json` or `.tsx` files nearby) vs plain JavaScript.
   - Look at an existing component in the same directory (if any) to match naming, file extension, export style (default vs named), and styling approach (CSS module, styled-components, Tailwind, plain CSS).

2. Ask the user (if not already specified) for:
   - Component name (use PascalCase for the component, matching the project's file naming convention).
   - Target directory (default to the project's existing components folder, e.g. `src/components/`).

3. Create the component directory `ComponentName/` containing:
   - `ComponentName.tsx` (or `.jsx` if the project isn't TypeScript) — a minimal functional component that accepts a typed `props` interface (if TS) and renders a simple placeholder.
   - A stylesheet matching the project's existing styling approach (e.g. `ComponentName.module.css`), only if the project already uses per-component stylesheets.
   - `ComponentName.test.tsx` — a minimal render test, only if the project already has a test setup (e.g. Jest, Vitest, React Testing Library present in `package.json`).
   - An `index.ts`/`index.js` barrel file only if that pattern already exists elsewhere in the project.

4. Do not invent conventions that aren't already present in the codebase — mirror what's there. If this is a brand-new project with no existing components, use plain functional-component + CSS module as a sensible default and say so.

5. After scaffolding, print the created file tree and briefly summarize what was generated.
