## Backend — Fastify

- Entry point `server.js` (or `api/server.js` on Vercel projects) wires plugins and routes only — no business logic in it.
- Structure: `src/routes/{resource}.js` → `src/services/` (business logic) → `src/lib/` (clients, helpers). Shared Fastify plugins live in `src/plugins/` and use `fastify-plugin` (`fp()`) when they must decorate the root instance.
- Group routes by prefix with `app.register(routes, { prefix: '/v1/x' })`. Move a handler to its own file once it exceeds ~50 lines.
- Handlers are `async` and either `return` the payload or call `reply.send()` — never both.
- **Validate all input at the route boundary with zod.** `schema.parse(request.body)` before touching services; never trust raw `request.body`/`request.query`.
- Standard error shape: `{ success: false, error: { code, message } }` with the correct HTTP status. Register one central `app.setErrorHandler` instead of per-route ad-hoc error responses.
- `@fastify/cors` uses an explicit origin whitelist. Never `origin: true` outside local dev.
- `dotenv` is loaded once in the entry point. Required env vars are validated at boot — fail fast naming the missing variable.
- Log through `app.log`/`request.log` (pino). Never `console.log` (see Code Style).
