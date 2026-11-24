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

## Demo logins
- Admin — ID `admin` · PIN `1234`
- Workers — `raju01`/`1111`, `suresh02`/`2222`, `amit03`/`3333`, `kiran04`/`4444`

Front-end demo only — data resets on refresh. No Firebase connection, so no Firestore rules apply yet.
