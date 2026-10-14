## Security & Environment

- `.env` is never committed. Secrets live only in `.env` (local) or the platform's env config (Vercel/host).
- Validate required env vars at boot and fail fast with a clear message naming the missing variable.
- Webhooks: verify the provider's signature/token whenever it offers one; reject unsigned requests.
- Never log request bodies containing tokens, passwords, or PII. Redact before logging.
- Anything under `VITE_*` is public — it is bundled into frontend code. Secrets are backend-only.
- Don't disable TLS verification, CORS, or auth "temporarily" — those changes ship.
