# live-camera-demo — Decisions

Newest first. Never delete an entry; supersede it.

**No decisions are recorded for this project.** There are none to record: this
repo has a single commit, no discussion, no issues, no PRs, and no prior
documentation of any kind (checked 2026-09-10). Nothing was invented to fill
this file.

**Where to look instead:** every real decision about the LIVE app — the
capture-only premise, the audience/visibility model, the scaling and cost
guardrails, the iOS/TestFlight path — was made in the upstream project and is
written down there. Read **[`../social-cam-platform/DECISIONS.md`](../social-cam-platform/DECISIONS.md)**,
and `PRODUCT.md` / `SCALING.md` in the same folder for the two areas that carry
their own open questions.

---

## Observed, not recorded

One structural choice is visible in the artifacts but was never written down as
a decision, so it is logged here as an observation rather than as a dated entry:

- **This repo is a public mirror, deliberately separate from the app repo.**
  `../social-cam-platform/deploy-demo.sh` describes its destination in its own
  comments as "the public mirror repo" and copies a single file into it. The
  practical reason is legible from the file listing — `social-cam-platform`
  contains `node_modules`, an iOS Capacitor shell, a lockfile and internal
  planning docs, none of which should be published, and GitHub Pages serves a
  whole repo. Whether that reasoning is what was actually in mind on 2026-07-20
  is not recorded anywhere. If it is confirmed, it belongs above as a proper
  dated entry.
