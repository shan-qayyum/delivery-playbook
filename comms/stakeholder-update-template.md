# Stakeholder update

**Why:** the person deciding needs to know what changed, what's at risk and
what you need from them. Plain words, decisions first, no jargon.

Send weekly, same day, same format. Keep it under one screen. If nothing
changed, say so in one line; silence reads as "something is wrong".

## Template

```markdown
# [Project] — week of [YYYY-MM-DD]

**Overall:** [On track / At risk / Off track] — [one-sentence reason]

## Decisions needed from you
1. [Question] — options: [A] / [B]. My recommendation: [A], because [reason].
   Needed by [date] so that [consequence].

## Done this week
- [Outcome in user/business terms] ([link to evidence])

## Next week
- [Planned outcome]

## Risks and blockers
| Risk | Effect if it happens | What we're doing | Owner |
| --- | --- | --- | --- |
| | | | |

## Numbers (only if they help decide)
- [Open P0/P1 issues: N]
- [Release this week: version, status]
```

Writing rules:

- Lead with decisions. They are the reason the update exists.
- One recommendation per decision, with the reason.
- Outcomes, not activity: "Customers can pay by bank transfer", not "worked on
  payment integration".
- If something slipped, say so and give the new date.

## Example (fictional)

> **Fictional.** Invented project and people.

```markdown
# Larkspur shop rebuild — week of 2026-03-09

**Overall:** At risk — payment provider approval is late.

## Decisions needed from you
1. Launch on 23 March with card payments only, or wait for bank transfer
   approval (no date yet)? My recommendation: launch with cards, because
   most orders on the old shop were card payments and waiting has no end
   date. Needed by Friday so we can book the launch email.

## Done this week
- Sale items can be bought again (ISS-003, verified on production).
- Every release now has a backup and a written rollback.

## Next week
- Mobile speed fixes (ISS-004).
- Launch rehearsal on staging.

## Risks and blockers
| Risk | Effect if it happens | What we're doing | Owner |
| --- | --- | --- | --- |
| Bank transfer approval late | Launch without it | Chasing provider twice a week | Ayesha |

## Numbers
- Open P0/P1 issues: 1 (was 3)
- Release v1.8.0: shipped 10 March, no rollback needed
```
