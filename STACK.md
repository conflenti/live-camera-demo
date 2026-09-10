# live-camera-demo — Stack

**Last verified:** 2026-09-10

## Runtime

**None to install.** The entire repo is one static file:

| File | Size | Lines |
|---|---|---|
| `index.html` | 47,296 bytes | 922 (≈571 of them the app `<script>`) |

There is **no `package.json`, no lockfile, no `node_modules`, no build step, no
bundler, no framework and no dependency of any kind** (directory listing and
grep, 2026-09-10). It is hand-written HTML, CSS and vanilla ES2020 JavaScript in
a single inline `<script>` block. Nothing is transpiled; the browser runs the
file exactly as committed.

Browser APIs it relies on (these, not npm packages, are its real dependencies):

- `navigator.mediaDevices.getUserMedia` — camera + mic. **Requires https or
  `localhost`**; the app detects this and shows an explicit warning otherwise.
  This constraint is the whole reason this hosted mirror exists.
- `MediaRecorder` with `MediaRecorder.isTypeSupported` probing, in order:
  `video/mp4`, `video/webm;codecs=vp9,opus`, `video/webm;codecs=vp8,opus`, `video/webm`.
- `HTMLCanvasElement.captureStream(30)` — used to record the front+back composite.
- `CanvasRenderingContext2D.roundRect` — relatively recent; no polyfill is present.
- `crypto.subtle.digest('SHA-256', …)` — the capture "receipt" hash. **Secure
  contexts only**, another https requirement.
- `indexedDB` — local post store, database `live-cam`, object store `posts`, keyPath `id`.
- `navigator.geolocation.getCurrentPosition` — optional, off by default, behind a toggle.

Known device caveat handled in code: iOS Safari and some Android devices refuse
two simultaneous camera streams, so "both" mode falls back to back-camera-only
with a visible message.

## Hosting & deploy

**GitHub Pages, legacy branch build.** Verified via
`gh api repos/conflenti/live-camera-demo/pages` on 2026-09-10:

- `status: built`, `build_type: legacy`
- `source: { branch: "main", path: "/" }`
- `public: true`, `https_enforced: true`, `cname: null` (no custom domain)
- `html_url: https://conflenti.github.io/live-camera-demo/`
- latest build: 2026-07-21T04:04:37Z

**Does merging deploy it? There is nothing to merge — pushing to `main` deploys
it.** Any commit on `main` triggers a Pages rebuild, live in roughly 30–60
seconds. There is no CI, no `.github/` directory, and no gate of any kind
between a push and the public URL.

**Do not edit `index.html` in this folder.** The supported deploy path is to
change the source and run the upstream script:

```bash
# edit ../social-cam-platform/www/index.html, then:
/Users/conflenti/Developer/social-cam-platform/deploy-demo.sh
```

That script `cp`s `social-cam-platform/www/index.html` over `./index.html`,
`git add -A`, commits as `Redeploy <UTC timestamp>`, and pushes. It hard-codes
this folder's absolute path (`/Users/conflenti/Developer/live-camera-demo`) and
exits if `.git` is missing there — so **this local clone must stay where it is,
under that exact name.** As of 2026-09-10 the script has never actually been run
against this repo (no `Redeploy …` commit exists).

## Data

**No server-side data. No database, no bucket, no external store.**

Everything a user creates stays in their own browser: IndexedDB database
`live-cam`, object store `posts`, records holding `{ id, type, blob, caption,
tags, receipt, draft, audience, location, originalBlob }`. Media blobs are
stored in full, including the pre-edit original when the photo editor was used.

Clearing site data destroys it. There is no sync, no account, no shared feed and
no export. The upstream `SCALING.md` specifies Cloudflare R2 for media if a real
backend is ever built; none of that exists today.

## External services

**None.** Grepped 2026-09-10 for `fetch(`, XHR, WebSocket, any `http://` or
`https://` request, and every common analytics vendor (gtag, GTM, Plausible,
PostHog, Umami, Mixpanel, Fathom, Matomo, Sentry): **zero hits.** The page loads
no fonts, no CDN scripts and no images from anywhere. It is fully self-contained
and works offline once loaded.

GitHub Pages is the only third party involved, and only as the file host.

## Environment variables

**None.** There is no server, no build step and no configuration file, so there
is nothing to configure and no secret is involved anywhere in this repo. If
analytics are added later (see `STATUS.md` → Next), prefer a script-tag key
that is public by design; nothing here should ever grow a `.env`.

## How to run locally

`getUserMedia` and `crypto.subtle` both require a secure context, so opening the
file with `file://` will show the permission warning and nothing will work. Serve
it:

```bash
cd /Users/conflenti/Developer/live-camera-demo
python3 -m http.server 8000
# then open http://localhost:8000/  (localhost counts as a secure context)
```

`http://localhost` is fine on a desktop browser. **Testing on a real phone over
the LAN will not work** — a plain-http LAN address is not a secure context, so
iOS Safari will refuse the camera. That is exactly what the public https URL is
for: use https://conflenti.github.io/live-camera-demo/ on the device, or an
https tunnel (`cloudflared tunnel --url http://localhost:8000`, `ngrok http 8000`)
if you need to test an unpublished change.

To work on the app rather than just view it, edit
`../social-cam-platform/www/index.html` and serve that folder instead — see that
project's `STACK.md`.
