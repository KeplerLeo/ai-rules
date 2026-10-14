## Frontend — React + Vite

- Stack: React, Vite, Tailwind CSS, Zustand, axios, lucide-react. Plain `.jsx` (JS unless the project has `tsconfig.json`).
- `src/` layout grouped by domain: `components/` (reusable, grouped into `components/{domain}/` past ~5 files; `components/ui/` for primitives), `pages/` (route screens), `store/` (Zustand), `services/` (`api.js` axios instance + external clients like supabase), `hooks/`, `utils/`.
- Components: `function Foo() {}` + named export; props destructured in the signature; one component per file, `PascalCase.jsx`.
- **Zustand**: one store per domain in `src/store/use{Domain}Store.js`. Actions live inside the store; components call actions, never mutate `state.x =` directly. Select with `useStore(s => s.field)` — don't subscribe to the whole store.
- **Tailwind**: utility classes first. Custom CSS only in `index.css` for design tokens/globals. No `style={{}}` except truly dynamic values (computed positions, dimensions).
- Data fetching goes through the axios instance in `services/api.js` (`baseURL` from `import.meta.env.VITE_API_URL`) — no inline `fetch`/`axios` calls with hardcoded URLs in components.
- Env vars exposed to the browser use the `VITE_*` prefix. **Never put secrets in `VITE_*`** — they ship in the bundle.
- Lint before finishing: `npm run lint` (oxlint — do not add ESLint alongside it).
