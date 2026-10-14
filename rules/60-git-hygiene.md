## Git Hygiene

- **One package manager per project: npm.** If a stray `pnpm-lock.yaml` or `yarn.lock` appears next to `package-lock.json`, remove it and run `npm install` to repair.
- Never commit: `node_modules/`, `.env`, `dist/`, `*.db`, `*.log`, credentials/certificates (`*.pem`, `cert_*`).
- Keep `.env.example` updated with variable **names only** (no values).
- Small commits, one purpose each. Imperative subject line describing the *why*; match the repo's existing message style.
- Never commit generated artifacts that the build recreates.
