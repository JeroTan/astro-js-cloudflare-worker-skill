# Architecture & Folder Structure

- Scaffold a bulletproof/DDD architecture in `src/`:
  - `components`, `assets`, `features`, `hooks`, `lib`, `stores`, `styles`, `test`, `types`, `utils`
  - Astro folders: `actions`, `i18`, `layouts`, `middleware`, `pages`
  - DDD folders: `domain`, `adapter/application`, `adapter/infrastructure` (services go here)
  - Backend/Elysia: `api/routes`, `api/config`, `controller`, `container`
  - Cloudflare: `cloudflare`
- Add a `_readme.md` in each folder explaining its purpose.
- **IMPORTANT**: If the user provides reference files for `durableObjects.ts`, `elysia.ts`, `formatter.ts`, `crypto/**`, `i18/**`, or `query.ts`, instruct the AI to copy them to the new project structure to retain helper patterns.