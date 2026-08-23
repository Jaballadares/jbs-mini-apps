# jbs-mini-apps

A shared repo for small web apps, deployed as static sites on GitHub Pages.

Live site (after Pages is enabled): https://jaballadares.github.io/jbs-mini-apps/

## One-time setup

GitHub Pages has to be turned on in the UI. It takes about 30 seconds:

1. Open [Settings → Pages](https://github.com/Jaballadares/jbs-mini-apps/settings/pages)
2. **Source:** Deploy from a branch
3. **Branch:** `main` / `/ (root)`
4. Save

The first publish usually takes 1–2 minutes. After that, every push to `main` updates the live site.

## Layout

```
.
├── index.html              Landing page. Reads apps.json.
├── apps.json               Manifest of published apps.
├── assets/                 Shared styles for the landing page.
├── apps/                   One folder per mini app.
│   └── _template/          Copy this to start a new app.
├── data/                   Optional JSON written by scheduled Actions.
├── workers/                Optional Cloudflare Workers (real HTTP endpoints).
└── .github/workflows/      Example scheduled-data workflow (not enabled).
```

Each app is just static files. After Pages is on, `apps/foo/` is live at:

`https://jaballadares.github.io/jbs-mini-apps/apps/foo/`

## Add a mini app

1. Copy `apps/_template` to `apps/your-app-name`.
2. Edit that folder. Keep an `index.html` at the app root.
3. Add an entry to `apps.json` so it shows up on the landing page:

```json
{
  "apps": [
    {
      "id": "your-app-name",
      "name": "Your App Name",
      "description": "One-line description.",
      "path": "apps/your-app-name/"
    }
  ]
}
```

4. Push to `main`.

Do not put a backend in `apps/`. Pages only serves static files.

Folders that start with `_` are ignored by the landing page.

## Optional: scheduled data (GitHub Actions)

Use this when an app needs data that refreshes on a timer, not per request.

Pattern:

1. A workflow fetches or computes something on a schedule.
2. It commits the result to `data/something.json`.
3. The static app reads that file with `fetch('../data/something.json')`.

There is a disabled example at [`.github/workflows/refresh-data.example.yml`](.github/workflows/refresh-data.example.yml). To turn it on, copy it to `.github/workflows/refresh-data.yml` and replace the placeholder step with real work.

This is **not** a request handler. Actions cannot answer live `fetch()` calls from the browser.

## Optional: a few server functions (Cloudflare Workers)

Use `workers/` when an app needs a real HTTP endpoint: a proxy, a form handler, a secret kept off the client, or anything that must run per request.

The static app on Pages calls the Worker with `fetch()`. Worker code stays in this repo and can be deployed from GitHub Actions later.

See [`workers/README.md`](workers/README.md).

## Local preview

Any static server from the repo root works. Relative paths are used on purpose.

```bash
python3 -m http.server 8080
```

Then open http://localhost:8080
