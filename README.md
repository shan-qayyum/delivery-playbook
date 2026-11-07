# Delivery Playbook

Copy-paste templates for running releases, triage, approvals and incident
follow-up on small software teams. Each one is short, written in plain
language, and comes with a filled-in example.

I'm Shan Qayyum, a software engineer moving into Technical Project Manager and
delivery roles. My code repos show how I build. This repo shows how I run the
work around the code: what gets fixed first, what a release must pass, how a
change gets approved, and how we go back when something breaks.

## Who it's for

- Tech leads and project coordinators on small teams without a formal
  process yet.
- Engineers who run production support and want a lightweight paper trail.
- Anyone taking over a live system and needing to get it under control.

## Index

| Area | File | Use it when |
| --- | --- | --- |
| Triage | [triage/issue-register.md](triage/issue-register.md) | You inherit a live system and need one ranked list of problems |
| Triage | [triage/issue-register.csv](triage/issue-register.csv) | You want the register as a spreadsheet |
| Release | [release/release-checklist.md](release/release-checklist.md) | Before, during and after every deploy |
| Release | [release/rollback-runbook.md](release/rollback-runbook.md) | Written before the release, used when it goes wrong |
| Release | [release/release-notes-template.md](release/release-notes-template.md) | Telling people what changed |
| Approvals | [approvals/approval-workflow.md](approvals/approval-workflow.md) | Documents or records need sign-off before use |
| Acceptance | [acceptance/definition-of-done.md](acceptance/definition-of-done.md) | Agreeing what "done" has to prove |
| Acceptance | [acceptance/test-gate-policy.md](acceptance/test-gate-policy.md) | Deciding what must pass before anything ships |
| Integrations | [integrations/failure-path-checklist.md](integrations/failure-path-checklist.md) | Before building anything that talks to another system |
| Incidents | [incidents/incident-report-template.md](incidents/incident-report-template.md) | After something went wrong, blameless write-up |
| Comms | [comms/stakeholder-update-template.md](comms/stakeholder-update-template.md) | Weekly status for people who don't read code |
| GitHub | [.github/ISSUE_TEMPLATE/](.github/ISSUE_TEMPLATE/) | Bug report form and release checklist issue |

## How I use these

These templates come from patterns I've used on real work. Clients are not
named.

- **Triage first, code second.** When I took over production support for a
  live e-commerce store, the first thing I wrote was a prioritized register of
  14 issues (P0–P3), each with severity, status and fix plan. That list set
  the order of the work. → [issue register](triage/issue-register.md)
- **Every release can be undone.** On the same store I set up version control
  matching production, a tagged release for every deploy, a backup before each
  change and a documented rollback. I plan every release around one question:
  if this goes wrong, how do we go back? →
  [release checklist](release/release-checklist.md),
  [rollback runbook](release/rollback-runbook.md)
- **Approval is a stage, not an email.** At an industrial engineering firm I
  replaced hand-edited Word templates with an ERPNext approval workflow (Draft
  → In Review → Approved For Use), with role-based permissions, edit locks
  during review, and an immutable snapshot so approved records freeze at
  generation time. → [approval workflow](approvals/approval-workflow.md)
- **Tests decide, not memory.** A customized desktop mail client shipped on 4
  Linux/macOS channels, and each release was gated by 27 automated end-to-end
  test suites. → [test gate policy](acceptance/test-gate-policy.md)
- **The old answer is the first acceptance test.** I rebuilt a legacy Excel
  engineering workbook as a React + TypeScript calculator; sign-off was
  matching the spreadsheet exactly (2.6207 bar on the benchmark case). →
  [definition of done](acceptance/definition-of-done.md)
- **The failure path is part of the scope.** An order hand-off to a lab's
  fulfillment API got retry queues; a property feed got monitoring and alerts,
  restoring all 74 active listings. →
  [failure-path checklist](integrations/failure-path-checklist.md)
- **Look for what didn't arrive.** On the e-commerce store, about 12 quote
  requests were rejected by Google's email checks in 10 days with no error on
  the site. Now one of my first questions on any live system is: what should
  be arriving that isn't? → [incident report](incidents/incident-report-template.md)
- **Design out steps that rely on memory.** Where a step depends on someone
  remembering (a daily timesheet, setting up a channel for a new task), I check
  whether the system can do it instead before adding a reminder.

## Working principles

1. Evidence first. "Fixed" means someone checked, and the check is written
   down.
2. Every release answers "how do we go back?" before it ships.
3. If a step relies on memory, try to design it out.
4. The failure path is scoped and written before the build.
5. If an old process already produces an answer, matching it is the first
   acceptance test.
6. Status updates are for the person deciding: plain words, decisions called
   out.

## Using the templates

Copy a file into your repo or wiki and delete what you don't need. The
`.github/ISSUE_TEMPLATE/` folder works as-is in any GitHub repo.

Every example in this repo is **fictional** and marked as such. Names, dates
and numbers in the examples are made up to show the template filled in.

## License

Docs are licensed under [CC BY 4.0](LICENSE). I picked it because these are
templates meant to be copied and adapted, including at work, and CC BY allows
that with only attribution. A software license fits code; this repo is prose.

## Contact

- Portfolio and case studies: <https://shanqayyum.com>
- LinkedIn: <https://www.linkedin.com/in/shan-qayyum-4594a22aa/>
- GitHub: [shan-qayyum](https://github.com/shan-qayyum)
