## Database — Prisma

- `prisma/schema.prisma` is the source of truth for the schema.
- Schema changes go through `npx prisma migrate dev -n <name>` — never edit an applied migration, never `db push` against production.
- **Prisma is only imported inside `*.repository.js` files.** Services and routes call the domain repository — the ORM never escapes the repository layer (see Repository Structure).
- PrismaClient is a singleton in `src/shared/clients/prisma.js` (single instance prevents pool exhaustion under hot-reload and serverless):

  ```js
  import { PrismaClient } from '@prisma/client';
  export const prisma = globalThis.__prisma ?? new PrismaClient();
  if (process.env.NODE_ENV !== 'production') globalThis.__prisma = prisma;
  ```

- Keep `postinstall: "prisma generate"` in `package.json`.
- Raw SQL only when Prisma can't express the query — wrap it in a service function, never inline in a route handler.
