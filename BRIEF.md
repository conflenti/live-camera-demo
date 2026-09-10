# live-camera-demo

**One line:** The public GitHub Pages mirror that serves the LIVE capture-only camera app to testers — it is a deploy target, not a project with its own source.

**Status:** Live (as a deploy target) — the app it serves is Parked
**Repo:** https://github.com/conflenti/live-camera-demo
**Live at:** https://conflenti.github.io/live-camera-demo/ (HTTP 200, verified 2026-09-10)

## What it is

This folder is a **one-file publishing mirror**. It contains exactly one tracked
file, `index.html` (47,296 bytes, 922 lines, verified 2026-09-10), and nothing
else — no `package.json`, no build step, no config, no subfolders, no CI. The
whole repo is a single commit: `1a76a24` "Deploy LIVE demo (public front-end for
testers)", 2026-07-20.

That `index.html` is **not written here.** It is a byte-identical copy of
`social-cam-platform/www/index.html` (`diff -q` returned no difference,
2026-09-10). It gets here by being copied in by
`../social-cam-platform/deploy-demo.sh`, which hard-codes this folder's absolute
path, does `cp`, `git add -A`, `git commit`, `git push`, and relies on GitHub
Pages (source: `main` / root, HTTPS enforced) to serve the result. **This folder
exists so that a `git push` publishes a URL.** Editing `index.html` here directly
would be a mistake — the next run of `deploy-demo.sh` overwrites it.

The app it serves — "LIVE" — is a single-page, vanilla-JS, no-dependency camera
whose entire premise is that **there is no file input anywhere in it.** Media can
only enter the feed through the live camera. It does photo and press-and-hold
segmented video (60-second hard ceiling), front/back/both cameras with a
picture-in-picture composite drawn on a canvas, a photo editor (adjust presets,
crop ratios, text overlay), a caption/audience/location review screen, and a
"receipt" — a SHA-256 hash of the original camera frame plus capture time,
device, camera and edit list — surfaced in a per-post info sheet. Posts are
stored locally in IndexedDB (`live-cam` / `posts`). There is no server: grepped
2026-09-10, the file makes **zero `fetch` or XHR calls of any kind.**

Be honest about the scale of what is in *this* folder: it is one HTML file, of
which roughly 571 lines are the app script. Everything about the product — the
audience model, the scaling plan, the iOS shell, the roadmap — lives in
`social-cam-platform`, not here.

## Who it's for

Two audiences, one step removed from each other:

- **The owner (Owen), operationally.** This is the thing that turns a local file
  into a link he can text someone. That is its only job.
- **Testers of the LIVE app**, who open the URL on a phone. The app requires
  `https` or `localhost` for `getUserMedia`, which is precisely why the demo is
  hosted at all rather than opened from disk.

It is **not** for developers. Nobody should clone this to work on the app —
that is `social-cam-platform`. It is also not a client deliverable and not a
pitch asset for any Conflenti Media client; nothing in this repo or in the
loose files at `~/Developer` ties it to a client engagement.

## Why it exists

`getUserMedia` will not run over `file://`, and iOS Safari will not run it over
plain http. To put the camera app in front of anyone with a phone, it has to be
served over https from a public URL. GitHub Pages does that for free, but Pages
serves a whole repo — and `social-cam-platform` is a private-ish working repo
carrying an iOS Capacitor shell, `node_modules`, planning docs and a
`package-lock.json`. So a separate, deliberately empty public repo was created
to hold only the one file that should be public. That is the entire rationale.

It replaced nothing. Before 2026-07-20 there was no public URL for the app.

## Relationship to other projects

**Downstream of [`social-cam-platform`](../social-cam-platform).** That is the
source of truth for every line in this folder. Specifically:

- `social-cam-platform/www/index.html` → copied verbatim to `./index.html`
- `social-cam-platform/deploy-demo.sh` → the only supported way to update this repo
- `social-cam-platform/BRIEF.md`, `STATUS.md`, `PRODUCT.md`, `SCALING.md`,
  `TESTFLIGHT.md` → all product, status and roadmap context

If you are reading this file because you want to change what the demo does,
stop here and go to `../social-cam-platform`.
