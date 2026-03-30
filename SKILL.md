---
name: astro-cloudflare-worker-setup
description: Scaffolds a comprehensive Astro JS project for Cloudflare Workers, including Durable Objects, Elysia API, D1 databases, and Domain-Driven Design (DDD) architecture. Use this skill/workflow when the user wants to start a new Astro project on Cloudflare.
---

# Astro Cloudflare Worker Setup

This skill scaffolds a comprehensive Astro JS project for Cloudflare Workers. 

## The Core Workflow

When this workflow is activated, you must follow these steps:

1. **Plan Mode:** Always convert to plan mode at the very beginning to lay out the steps.
2. **Initialization:** Run `npm create astro@latest`. Tell the user what to do to proceed.
3. **README:** Create a README.md outlining what the setup provides.
4. **Dependencies:**
   - Install adapters: `npm install @astrojs/cloudflare @astrojs/check`
   - Utilities: `npm install lodash p-queue jose` (Note: `jose` is for crypto as native crypto fails on Cloudflare).
   - Tailwind: `npm install tailwindcss @tailwindcss/vite`
   - Vitest: `npm install vitest`
   - SEO: `npm install @astrojs/sitemap`
   - *Optional (Ask user):* React (`npx astro add react`, then install `ahooks`).
5. **Astro Configuration (`astro.config.mjs`)**:
   - Add the Cloudflare adapter with `imageService: "compile"` and `workerEntryPoint: { path: "src/cloudflare/worker.ts" }`.
   - Add Tailwind plugin to Vite.
   - If React is installed, add Vite resolve alias:
     ```javascript
     export default defineConfig({
         vite: {
             resolve: {
                 //@ts-ignore
                 alias: import.meta.env.PROD && {
                     "react-dom/server": "react-dom/server.edge",
                 }
             }
         }
     })
     ```
6. **Worker Entry Point (`src/cloudflare/worker.ts`)**:
   - Must use this exact code:
     ```typescript
     import type { SSRManifest } from "astro";
     import { App } from "astro/app";
     import { handle } from "@astrojs/cloudflare/handler";

     export function createExports(manifest: SSRManifest) {
         const app = new App(manifest);
         return {
             default: {
                 async fetch(request, env, ctx) {
                     return handle(manifest, app, request as any, env as Cloudflare.Env as any, ctx);
                 },
                 async queue(batch, _env) {
                     let messages = JSON.stringify(batch.messages);
                     console.log(`consumed from our queue: ${messages}`);
                 },
                 async scheduled(event, env, ctx){
                     // Do some time logic heere
                 },
             } satisfies ExportedHandler<Cloudflare.Env>,
         };
     }
     ```

## References
Consult the following reference files for specific steps:
- `references/wrangler-config.md`: Wrangler configuration and database setup.
- `references/durable-objects.md`: Durable Objects and WebSockets setup.
- `references/ddd-architecture.md`: Domain-Driven Design architecture and folder structure.
- `references/elysia-api.md`: Elysia API setup with CORS and OpenAPI.
- `references/layouts.md`: Layouts and theming setup.