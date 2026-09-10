# live-camera-demo — Status

**Last verified:** 2026-09-10
**Branch:** `main`   **Behind origin:** up to date (`git fetch` then `git log --oneline HEAD..origin/main` → empty output, 2026-09-10)

**Working tree (2026-09-10):** `git status --short` showed one untracked entry,
`.DS_Store` (a macOS Finder artifact, not project content). There is no
`.gitignore` in this repo. Plus the four documentation files added on 2026-09-10,
left uncommitted by request.

**History:** one commit, ever. `1a76a24` "Deploy LIVE demo (public front-end for
testers)", authored 2026-07-20 23:02 -0500 by conflenti <oconflenti@gmail.com>,
`+922` lines in `index.html`. The GitHub repo was created 2026-07-21T04:02:47Z
and `pushed_at` is 2026-07-21T04:02:50Z — **nothing has been pushed to it since.**

## Live and working

- **The public demo URL serves.** `curl` on 2026-09-10 returned **HTTP 200**,
  47,296 bytes, `<title>LIVE — capture-only camera</title>` from
  https://conflenti.github.io/live-camera-demo/.
- **GitHub Pages is configured and built.** `gh api repos/conflenti/live-camera-demo/pages`
  on 2026-09-10 reported `status: built`, `source: {branch: main, path: /}`,
  `public: true`, `https_enforced: true`, no custom domain. The latest Pages
  build is dated 2026-07-21T04:04:37Z, `status: built`.
- **What is served is current, not stale.** `./index.html` is byte-identical to
  `../social-cam-platform/www/index.html` (`diff -q`, 2026-09-10). The demo
  reflects the newest app code that exists.

That is the complete list of what I can confirm. Note carefully what is *not*
claimed: I verified that a page is served, not that the app works. See below.

## Built but never exercised

- **I have not seen the LIVE app run.** I read all 922 lines; I did not open it
  on a device. Whether the camera initialises, whether the both-cameras PiP
  composite records, whether the segmented video stitching produces a playable
  file, whether the editor exports, and whether the SHA-256 receipt matches
  anything — all unverified by me as of 2026-09-10. The code is coherent and
  handles the obvious failure paths (iOS dual-stream fallback, a non-https
  warning, `MediaRecorder.isTypeSupported` mime probing). Coherent is not tested.
- **There is no evidence any tester ever opened this URL.** The repo has never
  been pushed to since the day it was created; the commit message says "for
  testers" but nothing records that it was actually sent to anyone.
- **No analytics of any kind are wired — this is the standing gap.** Grepped
  `index.html` on 2026-09-10 for gtag / GTM / Plausible / PostHog / Umami /
  Mixpanel / Fathom / Matomo / Sentry, and for `fetch(` and any `http://` or
  `https://` request: **zero hits. The file makes no network calls at all.**
  The demo has been live for 51 days (2026-07-21 → 2026-09-10) and we have no
  idea whether anyone has ever loaded it. Under the standing "wire analytics
  from day one" rule, low or unknown traffic is the reason to instrument, not a
  reason to defer. This is listed under **Next** and should not be argued away.
- **`deploy-demo.sh` has never been run against this repo.** It was committed to
  `social-cam-platform` as `9f70a8e`, dated after this repo's only commit, and
  this repo has zero commits matching the script's `Redeploy <timestamp>` message
  format. The one commit here was made by hand. So **the deploy path itself is
  untested** — plausible, and never exercised.

## In progress

Nothing. No branches other than `main` (local and `remotes/origin/main` only),
no open PRs, no CI (there is no `.github/` directory in this repo, and the
client-side rule of thumb across `~/Developer` — that GitHub Pages sites run no
CI on PRs — applies here trivially: there are no PRs).

## Parked

The **app** is parked; this mirror is parked as a consequence. Last app-code
change in the upstream repo was 2026-07-05; this mirror's only push was
2026-07-21. See `../social-cam-platform/STATUS.md` for the reasoning on why the
project stopped — the short version is that it stopped where it needed a real
backend, accounts and money, not at a bug.

**This folder itself needs no restart trigger.** It is not work in progress. It
resumes automatically the moment someone runs `../social-cam-platform/deploy-demo.sh`.

## Next

The whole roadmap belongs to `../social-cam-platform/STATUS.md`. Only two items
are properly this folder's:

1. **Instrument the demo page.** It has been publicly reachable since
   2026-07-21 with zero measurement. The change has to be made in
   `../social-cam-platform/www/index.html` (never edited here — it would be
   overwritten) and shipped through `deploy-demo.sh`. A privacy-respecting
   counter is the right shape given the ethos in
   `../social-cam-platform/SCALING.md`; a surveillance-ad tag is not.
2. **Exercise `deploy-demo.sh` once, deliberately,** so that the publishing path
   is known-good before anyone relies on it under time pressure. Right now it is
   a script that has never run end to end against a repo that has never received
   a second commit.

Do not add features, docs or config here. Anything that is not the deployed
`index.html` is clutter in a folder whose entire value is being a clean mirror.
