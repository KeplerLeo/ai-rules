## Database — Supabase

- One client per context:
  - Backend: `src/shared/clients/supabase.js` creates the admin client with `SUPABASE_SERVICE_ROLE_KEY`. **Supabase is only called from `*.repository.js` files** — services never touch the client directly.
  - Frontend: `src/services/supabase.js` creates the public client with `VITE_SUPABASE_URL` + `VITE_SUPABASE_ANON_KEY`.
- **`SUPABASE_SERVICE_ROLE_KEY` never reaches the frontend** — never in a `VITE_*` variable, never in a response, never in committed code.
- Frontend queries assume Row Level Security is ON; design tables/policies accordingly instead of filtering in the client.
- Schema/migration SQL is versioned under `database/` or `supabase/migrations/` — no untracked console edits.
- Prefer PostgREST calls (`.from('table')`) over RPC; use RPC/`rpc()` only when the query needs transactionality or joins PostgREST can't express.
