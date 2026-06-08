# kangsium-pages

Static GitHub Pages site hosting privacy policies + terms of service for every
Kangsium app — the Flutter/Play games (English) **and** the Apps in Toss web
mini-apps (Korean). Served at **https://policy.kangsium.com/**
(remote: `github.com/jasonkang14/privacy-policy`).

Plain HTML, no build step. Jekyll is disabled (`.nojekyll`) so files are served
exactly as written. Toss-app pages use `lang="ko"` and Korean headings.

## Layout

```
/CNAME                       policy.kangsium.com
/.nojekyll                   disable Jekyll (serve raw HTML)
/index.html                  landing page listing every app
/<app-slug>/privacy/index.html
/<app-slug>/terms/index.html
```

Each app gets a folder; folder + `index.html` gives clean trailing-slash URLs:

| File | URL |
|---|---|
| `bubble-pop/privacy/index.html` | https://policy.kangsium.com/bubble-pop/privacy/ |
| `bubble-pop/terms/index.html`   | https://policy.kangsium.com/bubble-pop/terms/ |

(Requests without the trailing slash 301-redirect to the slashed URL, which the
in-app WebView follows fine.)

## One-time GitHub setup

1. The repo is already wired to `github.com/jasonkang14/privacy-policy` — just push:
   ```bash
   git push -u origin main
   ```
2. Repo **Settings → Pages**: Source = *Deploy from a branch*, Branch = `main` / `/ (root)`.
3. **Settings → Pages → Custom domain**: enter `policy.kangsium.com` (this matches the
   committed `CNAME` file). Tick *Enforce HTTPS* once the cert is issued.
4. **DNS** (at the kangsium.com registrar): add one record —
   ```
   Type: CNAME   Host: policy   Value: <account>.github.io
   ```
   Propagation + cert issuance usually takes a few minutes to a couple of hours.

> If you ever want the apex `kangsium.com` instead of the `policy` subdomain, change the
> `CNAME` file + the DNS record (apex needs A/ALIAS records to GitHub's IPs, not a CNAME),
> and update each app's in-app privacy URL.

## Adding a new app

The `/prepare-app-for-store-release` skill generates `PRIVACY.html` + `TERMS.html` in the
game's project root. To publish them:

```bash
slug=sky-stack   # the app's URL slug
mkdir -p $slug/privacy $slug/terms
cp ../$slug/PRIVACY.html $slug/privacy/index.html
cp ../$slug/TERMS.html   $slug/terms/index.html
# add a card for it in index.html, then commit + push
```

Make sure the app's in-app privacy screen points at
`https://policy.kangsium.com/<slug>/privacy/`.
