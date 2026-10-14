## Deploy — Vercel

- `vercel.json` lives at the repo root. Serverless API functions go under `api/` when applicable.
- Pin the runtime: set `engines.node` in `package.json`.
- Frontend builds run through the `build` npm script (`vite build` inside the frontend dir).
- A change is not "deployed" until verified in production — never report a merged/pushed change as live without checking.
- Env vars are configured in the Vercel dashboard/CLI, not committed files.
