# Agent Guide (AGENTS.md)

This guide is designed to orient future AI agents and developers to the **React Router Single Page Application (SPA) Template** repository, ensuring that modifications are safe, idiomatic, and compliant with the project's invariants.

## Overview

This repository is a preconfigured React Router v7 template optimized for client-side only (SPA) applications. By explicitly disabling Server-Side Rendering (SSR), the application compiles into purely static assets that can be served from any static host.

- **Primary Goal**: High-performance, client-side React Router web application.
- **Key Stack**: React Router v7, React 19, Tailwind CSS v4, Vite 8, and TypeScript.

---

## Documentation Index

- [README.md](./README.md) — Sourcing, installation, deployment, and conceptual details.

---

## Fast Orientation

Below are the high-signal files and directories:

- **`react-router.config.ts`**
  Contains core compilation options for React Router. Crucially, it sets `ssr: false` to force SPA mode.
- **`vite.config.ts`**
  Configures Vite 8 plugins: `@tailwindcss/vite` (Tailwind CSS v4 support) and `@react-router/dev/vite` (compiler integration).
- **`app/routes.ts`**
  The routing configuration ledger. Every page/route in the application must be registered here.
- **`app/root.tsx`**
  The main entry point, root layout (`Layout` component), and application-wide `ErrorBoundary`.
- **`app/routes/home.tsx`**
  The index/home page component which renders the default `<Welcome />` screen.
- **`app/welcome/`**
  Contains components, assets (logos), and styles specifically for the default welcome screen.
- **`app/app.css`**
  Application stylesheet containing `@import "tailwindcss";` for Tailwind CSS v4 integration.

---

## Local Development Commands

Always run these commands from the project root using npm:

| Action          | Command             | Purpose / Context                                                                       |
| --------------- | ------------------- | --------------------------------------------------------------------------------------- |
| **Development** | `npm run dev`       | Starts local Vite dev server with Hot Module Replacement (HMR).                         |
| **Type Check**  | `npm run typecheck` | Generates route types (`react-router typegen`) and runs TypeScript compilation (`tsc`). |
| **Build**       | `npm run build`     | Compiles the production-ready static assets into the `build/client/` directory.         |
| **Preview**     | `npm start`         | Previews the compiled production build locally using `@react-router/serve`.             |

---

## Architectural Notes & Data Flow

### 1. Single Page Application (SPA) / Client-Only Execution

- There is **no Node.js server side** in production. The build outputs purely static assets.
- Data fetching must happen entirely client-side (e.g., using `fetch` or client data-loaders). Do not rely on server loader functions.

### 2. Client-Side Routing and Code-Splitting

- Routes are declared in `app/routes.ts` using the `@react-router/dev/routes` utility (e.g., `index("routes/home.tsx")`).
- Automatic type generation maps types under `.react-router/types/` to support type-safe route-level properties (like `Route.MetaArgs` or `Route.ErrorBoundaryProps`).

### 3. Styling with Tailwind CSS v4

- The project leverages `@tailwindcss/vite` which compiles utility classes on-the-fly during development and build.
- Global styles, typography configurations, or custom directives should be placed in `app/app.css`.

---

## Safety Rules & Invariants

To avoid breaking the SPA compilation or deployment flow, future agents **must** respect the following rules:

1. **Keep SSR Disabled**
   Never set `ssr: true` or remove the `ssr: false` property in `react-router.config.ts`. Doing so will trigger server-side bundle expectations during builds, breaking static deployments.
2. **Handle Client-Side Navigation Fallbacks**
   Because this is an SPA, direct subpage requests on a static hosting provider will 404 unless the provider is configured to rewrite all routes back to `index.html`. Do not assume the server handles routing.
3. **Run Typecheck After Route Modifications**
   Whenever adding, deleting, or renaming routes in `app/routes.ts`, immediately execute `npm run typecheck`. This ensures the auto-generated types (`.react-router/types/`) are synchronized and error-free.
4. **No Server-Only Imports**
   Do not import Node-only or server-only dependencies inside React components or client-facing logic, as it will break compilation and runtime execution in browser environments.

---

## Required Context Before Refactors

Before refactoring routing or layout:

1. Review `app/routes.ts` to see how routes are configured.
2. Read the layout structure in `app/root.tsx` to understand the nesting of components and global CSS/Font linking.
