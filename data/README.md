# Scheduled data

Put JSON (or other static files) here when a GitHub Action should refresh data on a timer.

Apps on GitHub Pages can then load it as a normal static file:

```js
const response = await fetch("../../data/example.json");
const data = await response.json();
```

Good fit:

- snapshots
- feeds
- anything that can be a few minutes stale

Not a fit:

- per-request APIs
- anything that needs secrets in the browser
- anything that must respond to a user action immediately

See `.github/workflows/refresh-data.example.yml` for a disabled starting point.
