# Alive Engineering — Production Control (prototype)

Static single-page app (Three.js inlined). No real build step.

## Repo layout
```
public/
  index.html      <- the app
  404.html
wrangler.toml     <- serves ./public as static assets
package.json      <- safety net: makes any npm build command a no-op
README.md
```

## Deployment flow: GitHub → Cloudflare

### 1) Push to GitHub
Commit everything above to the repo root and push to `main`.

### 2) Cloudflare auto-builds from the repo
In Cloudflare: **alive-engineering → Settings → Build**
- **Build command:** leave EMPTY  (best).  If it won't stay empty, use `npm run build` — now a safe no-op.
- **Deploy command:** `npx wrangler deploy`
- **Root directory:** `/`
Save → **Retry build**.  It deploys `./public` as static assets. Done.

Why it can't error anymore:
- Build command empty → skipped.  `npm run build` / `npm run dev` → echo → exit 0.
- Only `./public` is served, so node_modules is never uploaded.

## Simpler alternative — Cloudflare Pages
Create → **Pages** → connect the repo → Build command EMPTY, Output directory `public`.
No wrangler.toml / package.json needed there.

## Using the prototype
Opens straight into the **owner console** — no login (this is a demo prototype, not the full app).
The **switch-view button** (bottom-left of the sidebar, and the exit icon in the worker app) flips between the owner console and the worker app so you can show both roles.

Demo data only — everything resets on refresh. The floor sees party **codes** (P-07…), never party names. No Firebase connection, so no Firestore rules apply yet.