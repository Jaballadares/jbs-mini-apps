# Server functions

GitHub Pages cannot run server code. If a mini app needs a few real HTTP endpoints, put them here as Cloudflare Workers.

Typical split:

- `apps/` stays static and is deployed by GitHub Pages
- `workers/` handles proxies, form posts, or anything that needs a secret
- the app calls the Worker with `fetch()`

## Suggested layout

```
workers/
  wrangler.toml
  src/
    index.ts
```

A starting `wrangler.toml`:

```toml
name = "jbs-mini-apps"
main = "src/index.ts"
compatibility_date = "2026-08-23"
```

Do not commit API keys. Use Wrangler secrets or GitHub Actions secrets at deploy time.

No Worker is deployed from this scaffold. Add one only when an app actually needs an endpoint.
