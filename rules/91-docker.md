## Docker

- **`.dockerignore` is mandatory whenever a Dockerfile exists**: exclude `node_modules`, `.env`, `dist`, `.git`. Without it the build context ships secrets and junk into every build (and `COPY . .` can bake `.env` into the image).
- **Never `COPY .env` or credentials into the image** — secrets end up in image layers permanently. Configuration goes in at runtime: `env_file`/`environment` in compose, `-e` flags, or the platform's env config. (Real footgun: `Dockerfile.evolution` did this.)
- **Base image: latest Node LTS line only** — `node:lts-alpine` (auto-tracks the current LTS) or an explicit LTS pin (`node:22-alpine` today). Never `:latest` (the Current line, odd majors, breaking changes) and never EOL majors. `lts-alpine` floats between minors on rebuild — acceptable; the lockfile + `npm ci` is what actually pins the app.
- No `|| true` in `CMD`/entrypoint — a migration or setup failure must crash loudly, not boot a broken container.
- Compose files are for infra dependencies (postgres, redis, etc.) — give them `healthcheck` + named volumes + `restart: unless-stopped`.
- Credentials in `docker-compose.yml` come from `env_file`/`${VAR}` referencing `.env` — never a hardcoded password committed to VCS.
- Node production images: `npm ci` (not `npm install`), run as a non-root user (`USER node`).
- Don't commit empty Dockerfiles — either write the working recipe or remove the file.
