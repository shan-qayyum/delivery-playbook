# Release checklist

**Why:** a release that depends on someone remembering the steps will
eventually skip one. A checklist makes "ready" something you can check.

The question every release has to answer before it ships: **if this goes
wrong, how do we go back?** If the answer is "we'd figure it out", the release
isn't ready.

## Template

```markdown
# Release [version or tag] — [YYYY-MM-DD]

Release owner: [name]    Approver: [name]    Window: [time + timezone]

## 1. Pre-release
- [ ] Scope is listed (issues/PRs) and nothing else is in the build
- [ ] Code in version control matches what is on production now (no drift)
- [ ] Release is tagged: [tag]
- [ ] Test gate passed (see acceptance/test-gate-policy.md): [link to run]
- [ ] Backup taken of code, database and uploaded files: [location]
- [ ] Backup restore was tested in the last [N] days
- [ ] Rollback runbook filled in for this release: [link]
- [ ] Rollback triggers agreed (section 4)
- [ ] Config / environment changes listed; secrets are not in the repo
- [ ] Database migrations reviewed; reversible or a restore plan exists
- [ ] People affected have been told the window

## 2. Deploy
- [ ] Announce start in [channel]
- [ ] Maintenance mode on (if needed)
- [ ] Deploy tag [tag] using [method]
- [ ] Run migrations
- [ ] Clear caches that hold old pages, forms or config
- [ ] Maintenance mode off

## 3. Verify (on production, not staging)
- [ ] Home page and main flows load without errors
- [ ] Each critical flow tested end to end: [list, e.g. checkout, contact form]
- [ ] Something that should arrive actually arrives (email, webhook, order)
- [ ] Error logs checked for new errors in the first [N] minutes
- [ ] Monitoring and alerts are green
- [ ] Each issue in scope re-checked and "Verified by" filled in

## 4. Rollback triggers (decide before deploying)
Roll back without debate if any of these happen:
- [ ] A critical flow fails verification and can't be fixed in [N] minutes
- [ ] Error rate or log errors above [threshold]
- [ ] Data is being written wrongly or lost
- [ ] Anything that was in the "do not break" list breaks

Who can call the rollback: [name(s)]

## 5. Close
- [ ] Announce done (or rolled back) in [channel]
- [ ] Release notes published
- [ ] Backup kept until [date]
```

## Example (fictional)

> **Fictional.** Invented release, shown partly filled in.

```markdown
# Release v1.8.0 — 2026-03-10

Release owner: Bilal    Approver: Ayesha    Window: 22:00–22:30 PKT

## 1. Pre-release
- [x] Scope: ISS-003 (sale items 500), ISS-004 (mobile images)
- [x] Production matches main at v1.7.2 (diff empty)
- [x] Tagged: v1.8.0
- [x] Test gate passed: CI run #212
- [x] Backup: db + files to storage bucket /backups/2026-03-10
- [x] Restore tested on staging 2026-03-08
- [x] Rollback runbook: release/rollbacks/v1.8.0.md

## 4. Rollback triggers
- Checkout fails for any test order
- Any new 500 errors on shop pages in the first 15 minutes
Who can call it: Bilal or Ayesha
```
