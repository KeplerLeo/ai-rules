## Code Style (JavaScript)

- **ESM only**: `import`/`export`. Never `require`/`module.exports`. `package.json` has `"type": "module"`.
- **Plain JS by default.** If the project has a `tsconfig.json`, TypeScript is in use — keep `strict` on.
- Two-space indentation.
- Naming: `camelCase` for functions/variables/files; `PascalCase` for React components and their `.jsx` files; `kebab-case` for other files.
- Optional fields use the `maybe_` prefix: `maybeUserId`, `maybeSessionToken`.
- Booleans read as predicates: `isActive`, `hasAccess`, `shouldRetry`.
- **No `console.log` in committed code.** Backend logs via Fastify's logger (`app.log` / `request.log`, pino underneath). Frontend ships no logs to production.
- Errors: `try/catch` at boundaries (route handlers, scripts, top-level jobs). Never swallow an error silently — log it or rethrow. No empty `catch {}`.
- HTTP calls go through a shared axios instance — `src/services/api.js` on the frontend, `src/shared/clients/` on the backend — with `baseURL` from env. Never `axios.get("https://...")` with a hardcoded URL inline.

### File layout (top-to-bottom reading)

Organize each file so a reader sees the important things first:

1. Imports
2. Constants / config
3. Primary export (main function, class, or component)
4. Supporting helpers below their callers — follow the call chain downward
5. Tests (if colocated) at the bottom
