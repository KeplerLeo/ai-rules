## Git Hygiene

- **One package manager per project: npm.** If a stray `pnpm-lock.yaml` or `yarn.lock` appears next to `package-lock.json`, remove it and run `npm install` to repair.
- Never commit: `node_modules/`, `.env`, `dist/`, `*.db`, `*.log`, credentials/certificates (`*.pem`, `cert_*`).
- Keep `.env.example` updated with variable **names only** (no values).
- Small commits, one purpose each. Imperative subject line describing the *why*; match the repo's existing message style.
- Never commit generated artifacts that the build recreates.

### Branches

- Trunk-based: branch short-lived off `main`, merge via PR, delete after merge.
- Always `git pull` (fetch + fast-forward) `main` **before** creating a new branch — never branch off a stale local `main`.
- Naming: `<type>/<scope>-<short-desc>` — e.g. `feat/auth-refresh`, `fix/cart-total`, `chore/deps-bump`. Types: `feat`, `fix`, `chore`, `refactor`, `docs`, `test`, `hotfix`.
- One branch = one concern. Never mix refactor + feature in the same branch.
- Rebase onto `main` before opening the PR; resolve conflicts locally, never with merge commits into `main` (unless the repo uses merge-flow).
- `main` is always deployable. Direct pushes to `main` are forbidden — PRs only.
- `hotfix/*` branches may target `main` directly for production incidents, but must be back-ported to any release/dev branch.

### Atomic Commits

- Each commit is a self-contained unit: it builds, passes lint/tests, and does exactly one thing.
- If reverting the commit would leave the repo broken or remove unrelated changes, it's not atomic.
- Use `git add -p` (hunk staging) to split a messy working tree into separate commits — never `git add -A` blindly.
- Order matters: refactor first, then feature, then docs/tests — each in its own commit so history reads as a story.
- Squash "fixup"/"typo"/"wip" commits before merge (interactive rebase or squash-merge); do not merge WIP history.
- If the repo uses Conventional Commits, format: `<type>(<scope>): <imperative why>` — otherwise match existing style.
- Never `git commit --amend` or force-push commits already shared/reviewed on a PR without team agreement.
