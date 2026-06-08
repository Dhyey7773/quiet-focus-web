# AGENTS.md

## Cursor Cloud specific instructions

### Product overview

**Quiet Focus** is a static web app (no build step, no `package.json`). It consists of:

| File | Purpose |
|------|---------|
| `index.html` | Marketing landing page + waitlist signup |
| `live-demo.html` | Full focus-timer app (auth-gated) with Supabase sync |

### Services

| Service | Required | How to start | Port |
|---------|----------|--------------|------|
| Static HTTP server | Yes | `python3 -m http.server 8080` from repo root | 8080 |
| Supabase (hosted) | Yes for waitlist/auth/sync | Cloud only — credentials are hardcoded in both HTML files | 443 |
| jsDelivr CDN | Yes at runtime | Loaded automatically by the pages | 443 |

**Do not** open pages via `file://` — use an HTTP server so OAuth redirects and browser APIs work correctly.

### Running the app

```bash
python3 -m http.server 8080
```

Then open:

- Landing: http://localhost:8080/index.html
- Demo app: http://localhost:8080/live-demo.html

Use a tmux session for the dev server so it stays running across agent turns.

### Lint / test / build

There are no lint, test, or build scripts in this repo. Verification is manual: serve the static files and exercise the pages in a browser.

### Supabase notes

- `SUPABASE_URL` and anon key are embedded in `index.html` and `live-demo.html` (no `.env` files).
- Waitlist (`index.html`) needs the `waitlist` table; the demo app needs Auth plus a `sessions` table.
- The code references `supabase-schema.sql`, but that file is not in the repo — schema must exist in the Supabase project dashboard.
- Full demo flow is behind auth; sign-in or account creation is required to reach the timer UI.

### Gotchas

- Hero CTA on `index.html` links to `Focus_state.html`, which does not exist — use `live-demo.html` for the working demo.
- `favicon.ico` is missing (harmless 404 in the browser console).
