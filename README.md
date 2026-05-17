# globalshopper-apex

Tiny static site hosted on Render that lives at `https://globalshopper.in`
(without `www`). Its only jobs:

1. **Serve `/.well-known/assetlinks.json`** so Google can verify the
   Android app (`in.globalshopper.app`) for Android App Links on the
   apex domain. The main Express app at `https://www.globalshopper.in`
   serves the same file from its own route — Google requires both
   domains to serve the file directly (it does not follow redirects).

2. **Redirect human visitors** who type `globalshopper.in` to
   `www.globalshopper.in` where the full app lives.

The real Express app, catalog SQLite, admin panel etc. all live in the
`befach-sourcing` repo and run as a Render Web Service. This repo is
separate so the apex domain can be attached to its own Render Static
Site and serve content directly (Render's Web Service redirects every
non-primary custom domain to the primary, which would 301 the
assetlinks file and fail Google's verification).

## Files

| File | Purpose |
|------|---------|
| `.well-known/assetlinks.json` | Digital Asset Links for Android App Links |
| `index.html` | Redirect-to-www page for human visitors |

## Updating the SHA-256 fingerprints

If you re-sign the Android app (e.g. key rotation), update the
`sha256_cert_fingerprints` array in `assetlinks.json` and push. Render
auto-redeploys.

The two current fingerprints come from Play Console → Setup → App
integrity:
- App signing key certificate (top one)
- Upload key certificate (bottom one)

Keep both unless you're certain about which one Play uses.

