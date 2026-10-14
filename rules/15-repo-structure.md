## Repository Structure

Canonical layout — domain-driven modular monolith. New projects MUST follow it;
existing projects converge gradually: apply the structure to code you touch, no
bulk renames.

```text
project/
├── backend/                    # Node API. `api/` ONLY for Vercel serverless.
│   ├── src/
│   │   ├── server.js           # entry: wiring only — plugins, routes, listen.
│   │   ├── config/             # env loading + validation (fail fast)
│   │   ├── modules/            # ONE DIRECTORY PER DOMAIN — the organizing unit
│   │   │   ├── tickets/
│   │   │   │   ├── tickets.routes.js      # HTTP layer: zod in, thin handlers
│   │   │   │   ├── tickets.service.js     # use cases / business rules
│   │   │   │   ├── tickets.repository.js  # ALL prisma/supabase access
│   │   │   │   └── tickets.schemas.js     # zod schemas (request/response DTOs)
│   │   │   ├── auth/ users/ webhooks/ ...
│   │   └── shared/             # domain-agnostic cross-cutting only
│   │       ├── plugins/        # fastify plugins (auth, supabase, cors setup)
│   │       ├── clients/        # prisma singleton, supabase admin, ext APIs
│   │       ├── errors/         # error types + central error handler
│   │       └── utils/
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/         # grouped by domain (components/tickets/…)
│   │   ├── pages/ hooks/ store/ utils/
│   │   ├── services/           # api.js axios instance + external clients
│   │   ├── App.jsx  main.jsx  index.css
│   └── package.json
├── database/                   # SQL schemas/migrations not owned by an ORM
├── docs/                       # only when needed (see Documentation Policy)
├── .env.example  .gitignore  README.md  AGENTS.md  CLAUDE.md
└── package.json                # orchestration scripts only
```

### Domain rules

- The organizing unit is the **domain** (`modules/tickets/`), not the technical
  layer. A new domain = a new directory with up to the 4 files above (skip ones
  genuinely empty for that domain, e.g. `webhooks/` may have no repository).
- **One-way dependency: `routes → service → repository → ORM`.** Repositories
  are the ONLY files that import prisma/supabase — services never see the ORM.
- Routes never import repositories; services never build HTTP responses.
- Cross-domain calls go through the other domain's **service** (or `shared/`) —
  never reach into another module's repository or internals.
- `shared/` holds only domain-agnostic code. If it references a domain concept,
  it lives in that domain's module.
- Domain dirs are lowercase, named by business concept (`tickets`, `users`,
  `auth`, `webhooks`) — not by technical role (`helpers`, `misc`, `common`).
- `backend/` is the canonical backend dir name; `server/` and root `src/` are
  legacy variants — converge when restructuring anyway. `api/` is reserved for
  Vercel serverless functions, not a regular Fastify app.
- Projects already on MVC (`controllers/`/`services/`/`models/`) or clean
  architecture keep their convention — but handlers/controllers stay thin and
  delegate to a service layer. When in doubt, `controller ≈ routes`,
  `service ≈ service`, `model/dao ≈ repository`.

### Frontend mirrors domains

- `components/` and `pages/` group by domain (`components/tickets/`,
  `components/dashboard/`); `components/ui/` holds primitives only.
- One Zustand store per domain (`store/useTicketsStore.js`).
- `services/api.js` is the single HTTP boundary — no other file talks to the
  backend.
