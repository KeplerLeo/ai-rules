## Frontend — React + Vite

- Stack: React, Vite, Tailwind CSS, Zustand, axios, lucide-react. Plain `.jsx` (JS unless the project has `tsconfig.json`).
- `src/` layout: `components/` (reusable), `pages/` (route screens), `store/` (Zustand), `lib/` (`api.js` axios instance, utils), `hooks/` (shared hooks).
- Components: `function Foo() {}` + named export; props destructured in the signature; one component per file, `PascalCase.jsx`.
- **Zustand**: one store per domain in `src/store/use{Domain}Store.js`. Actions live inside the store; components call actions, never mutate `state.x =` directly. Select with `useStore(s => s.field)` — don't subscribe to the whole store.
- **Tailwind**: utility classes first. Custom CSS only in `index.css` for design tokens/globals. No `style={{}}` except truly dynamic values (computed positions, dimensions).
- Data fetching goes through the axios instance in `lib/api.js` (`baseURL` from `import.meta.env.VITE_API_URL`) — no inline `fetch`/`axios` calls with hardcoded URLs in components.
- Env vars exposed to the browser use the `VITE_*` prefix. **Never put secrets in `VITE_*`** — they ship in the bundle.
- Lint before finishing: `npm run lint` (oxlint — do not add ESLint alongside it).
