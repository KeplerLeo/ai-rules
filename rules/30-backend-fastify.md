## Backend — Fastify

- `server.js` (or `api/server.js` on Vercel) wires plugins and routes only — no business logic in it. See Repository Structure for the `modules/` + `shared/` layout.
- Each domain lives in `src/modules/{domain}/` with `{domain}.routes.js` + `{domain}.service.js` + `{domain}.repository.js` + `{domain}.schemas.js`. Cross-cutting code goes in `src/shared/` (`plugins/`, `clients/`, `errors/`, `utils/`).
- Register each domain's routes by prefix: `app.register(ticketsRoutes, { prefix: '/v1/tickets' })` from `server.js` or a `routes/index.js`.
- Shared Fastify plugins use `fastify-plugin` (`fp()`) when they must decorate the root instance.
- Handlers are `async`, thin, and either `return` the payload or call `reply.send()` — never both. Business logic belongs in the domain's `*.service.js`.
- **Validate all input at the route boundary with zod** — schemas live in `{domain}.schemas.js`; `schema.parse(request.body)` before touching services. Never trust raw `request.body`/`request.query`.
- Standard error shape: `{ success: false, error: { code, message } }` with the correct HTTP status. One central `app.setErrorHandler` in `shared/errors/`, not per-route ad-hoc responses.
- `@fastify/cors` uses an explicit origin whitelist. Never `origin: true` outside local dev.
- `dotenv` is loaded once in the entry point (`config/`). Required env vars are validated at boot — fail fast naming the missing variable.
- Log through `app.log`/`request.log` (pino). Never `console.log` (see Code Style).
