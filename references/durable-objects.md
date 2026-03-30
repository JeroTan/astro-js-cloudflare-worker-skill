# Durable Objects & WebSockets

If the user needs persistent storage or WebSockets:

1. Add `namedExports: ["ExampleDurableObject"]` to `workerEntryPoint` in `astro.config.mjs`.
2. Create classes extending `DurableObject` in `src/cloudflare/durable-objects/`:
   ```typescript
   import { DurableObject } from "cloudflare:workers";
   export class ExampleDurableObject extends DurableObject {
       constructor(ctx: DurableObjectState, env: Env) {
           super(ctx, env);
       }
       async fetch(request: Request): Promise<Response> {
           // Return websocket or response here
       }
   }
   ```
3. Export these classes in `src/cloudflare/worker.ts` AFTER the default export block.
   ```typescript
   export function createExports(manifest: SSRManifest) {
       // ...
       return {
           default: { ... },
           ExampleDurableObject: ExampleDurableObject
       }
   }
   ```
4. Create `src/features/websocket/durable-object-guide.md` explaining `this.ctx.storage.get/put/delete` and `this.ctx.blockConcurrencyWhile`.