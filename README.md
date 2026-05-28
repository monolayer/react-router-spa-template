# React Router Single Page Application Template

Get started with the React Router Single Page Application (SPA) template to build high-performance, client-side web applications.

## About the Template

This template provides a preconfigured development environment for building client-side only web applications using React Router. By disabling Server-Side Rendering (SSR), your application compiles into static assets that you can host on any static web hosting provider.

### Key Features

- **React Router v7**: Offers modern client-side routing, code-splitting, and client data loading patterns without server-side overhead.
- **Tailwind CSS v4**: Provides rapid, utility-first styling with the high-performance `@tailwindcss/vite` compiler plugin.
- **Vite 8 and React 19**: Delivers fast build speeds and modern state management improvements.
- **TypeScript**: Enables end-to-end type safety, including automatic route type generation.

## Project Structure

This template contains a streamlined folder structure to keep your development focused and organized.

- `app/root.tsx`: The primary entry point and main layout component for your application.
- `app/routes.ts`: The routing configuration file where you declare the routes for your application.
- `app/welcome/`: A folder containing components and assets for the default welcome screen.
- `react-router.config.ts`: The build configuration file that controls React Router compiler options.
- `vite.config.ts`: The build tool configuration where Vite plugins and bundler settings are declared.

Listing 1-1: Disabling Server-Side Rendering in `react-router.config.ts`

```ts
import type { Config } from "@react-router/dev/config";

export default {
  ssr: false, // Disables server-side rendering to generate a static client-side application
} satisfies Config;
```

Listing 1-2: Configuring Vite Plugins in `vite.config.ts`

```ts
import { reactRouter } from "@react-router/dev/vite";
import tailwindcss from "@tailwindcss/vite";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [
    tailwindcss(), // Enables Tailwind CSS v4 support
    reactRouter(), // Enables React Router compiler and dev server integration
  ],
  resolve: {
    tsconfigPaths: true,
  },
});
```

## Configuring and Running Your Project

### Concept/Goal

Start up the local development server and compile your application for production.

### Prerequisites

Ensure you have Node.js 20 or later installed on your system.

### Numbered Steps

1.  **Install** the project dependencies using your terminal.
    ```bash
    npm install
    ```
2.  **Start** the local development server to test your changes.
    ```bash
    npm run dev
    ```
3.  **Build** the production static files into the `build/client/` directory.
    ```bash
    npm run build
    ```
4.  **Preview** the production build locally using the built-in React Router server.
    ```bash
    npm start
    ```

### Expected Result

Your terminal displays a local address (typically `http://localhost:5173`) where you can view your application in a web browser. When you run the build step, you should see a `build/client/` directory containing `index.html` and your compiled assets.

## Deploying Your Application

Because this template is configured with SSR disabled, the build process generates static web assets. You can deploy the contents of the `build/client/` directory directly to any static hosting service.

These services include Vercel, Netlify, Cloudflare Pages, GitHub Pages, or Amazon S3. No Node.js server is required in production to host your application.

**IMPORTANT:** When deploying to a static host, you must configure the platform to redirect all requests to `index.html`. This ensures that client-side routing functions correctly when a visitor accesses subpages directly.
