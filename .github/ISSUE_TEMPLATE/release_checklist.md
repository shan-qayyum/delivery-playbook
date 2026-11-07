---
name: Release checklist
about: Track one release from pre-checks to close. Full guide in release/release-checklist.md.
title: "Release [tag] — [YYYY-MM-DD]"
labels: release
assignees: ''
---

- Release owner:
- Approver:
- Window (with timezone):

## Pre-release

- [ ] Scope listed (issues/PRs), nothing else in the build
- [ ] Version control matches production (no drift)
- [ ] Release tagged:
- [ ] Test gate passed (link to run):
- [ ] Backup taken (location):
- [ ] Rollback runbook written for this release (link):
- [ ] Rollback triggers agreed (below)
- [ ] Migrations reviewed; reversible or restore plan exists
- [ ] People affected told the window

## Rollback triggers

Roll back without debate if:

- [ ] A critical flow fails verification and can't be fixed in [N] minutes
- [ ] New errors above [N] in the first [N] minutes
- [ ] Data written wrongly or lost

Who can call the rollback:

## Deploy

- [ ] Start announced
- [ ] Deployed tag
- [ ] Migrations run
- [ ] Caches cleared

## Verify on production

- [ ] Critical flows tested end to end:
- [ ] Something that should arrive actually arrives (email, webhook, order)
- [ ] Error logs checked for [N] minutes
- [ ] Each issue in scope has "Verified by" filled in

## Close

- [ ] Done (or rolled back) announced
- [ ] Release notes published
- [ ] Backup kept until:
