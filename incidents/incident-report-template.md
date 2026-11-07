# Incident report

**Why:** the goal is to change the system so it can't happen the same way
again. Blame changes nothing except what people are willing to report.

Blameless means: describe what people did and why it made sense at the time.
Look for the missing check, unclear step or silent failure that let it
through.

Write it within a few days, while details are fresh. Share it with everyone
affected.

## Template

```markdown
# Incident: [short title]

Date: [YYYY-MM-DD]    Severity: [P0–P3]    Author: [name]
Status: [Draft / Reviewed]    Reviewed by: [names]

## Summary
[2–3 sentences: what happened, who was affected, how long, current state]

## Impact
- Who/what was affected: [customers, orders, leads, internal users]
- How many / how much: [only what you can count; say "unknown" if unknown]
- Duration: [from first bad event to fix]

## Timeline (with timezone)
| Time | Event |
| --- | --- |
| | First bad event (often found later, from logs) |
| | Detected: [how, by whom] |
| | Response started |
| | Contained |
| | Fixed |
| | Verified fixed: [how] |

## Detection gap
Time between first bad event and detection: [duration]
Why it took that long: [no alert / alert went to nobody / no error was shown]

## What should have arrived that didn't?
[The missing result: an email, an order, a sync, a report, a log line. Many
incidents show up as an absence, not an error. Name it, and name the check
that would have noticed it.]

## Root cause
[The underlying cause, not the trigger. Ask "why" until you reach something
you can change.]

## What went well
-

## Actions
| Action | Type (prevent / detect / respond) | Owner | Due | Issue |
| --- | --- | --- | --- | --- |
| | | | | |

## Evidence
[Links to logs, screenshots, tickets. Remove personal data and secrets.]
```

## Example (fictional)

> **Fictional.** Invented incident, names and times.

```markdown
# Incident: Newsletter sign-ups not saved for 6 days

Date: 2026-07-20    Severity: P1    Author: Kamran
Status: Reviewed    Reviewed by: Nadia, Usman

## Summary
After the 14 July release, the newsletter form showed "Thanks!" but did not
save sign-ups. Fixed 20 July. Sign-ups from those 6 days were lost.

## Impact
- Visitors who signed up 14–20 July: not added to the list
- How many: unknown; the form did not log failures
- Duration: 6 days

## Timeline (PKT)
| Time | Event |
| --- | --- |
| 14 Jul 21:10 | Release v2.3 deployed; form field renamed |
| 20 Jul 11:00 | Nadia notices the weekly sign-up count is zero |
| 20 Jul 11:30 | Kamran reproduces: API rejects the renamed field |
| 20 Jul 12:15 | Fix deployed, test sign-up saved and verified |

## Detection gap
6 days. The form showed success regardless of the API response. No alert.

## What should have arrived that didn't?
New rows in the subscriber list. Nothing checked that sign-ups kept arriving.

## Root cause
The form ignored the API's error response, and the release checklist had no
step to test the newsletter form end to end.

## Actions
| Action | Type | Owner | Due | Issue |
| --- | --- | --- | --- | --- |
| Show an error if the save fails | prevent | Kamran | 22 Jul | #301 |
| Add newsletter sign-up to the E2E gate | prevent | Usman | 25 Jul | #302 |
| Alert if zero sign-ups in 48 h | detect | Nadia | 25 Jul | #303 |
```
