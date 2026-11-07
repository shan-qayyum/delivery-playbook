# Failure-path checklist

**Why:** the failure path is part of the scope. Left for "later", it gets
designed in production, under pressure.

Fill this in for every integration (API, webhook, feed, email, payment,
sync) **before the build starts**, next to the happy path. Review it with
whoever will be woken up when it breaks.

## Template

```markdown
# Failure path: [integration name]

From: [system A]    To: [system B]    Owner: [named person]
Written: [YYYY-MM-DD]    Reviewed by: [name]

## Happy path (one line)
[What goes where, when]

## What can fail
| Failure | Likely? | Effect if unnoticed |
| --- | --- | --- |
| Other side is down or times out | | |
| Other side rejects the data (4xx, validation) | | |
| Auth/credentials expire | | |
| We send duplicates | | |
| Data arrives late or out of order | | |
| Nothing arrives at all (silent failure) | | |

## Retries
- Retried: [which failures]
- Not retried: [which failures, e.g. validation errors, and where they go]
- Schedule: [e.g. 1 min, 5 min, 30 min, 2 h, then stop]
- Idempotency: [key used so a retry can't create a duplicate]
- After the last retry: [dead-letter queue / failed list / manual review]

## Alerts
| Condition | Who is told | Channel | Within |
| --- | --- | --- | --- |
| Item failed all retries | | | |
| Queue backlog over [N] | | | |
| No successful sync in [time] (absence check) | | | |

## Owner and runbook
- Owner: [name]. Backup owner: [name].
- How to replay a failed item: [steps]
- How to pause the integration: [steps]

## Tests before go-live
- [ ] Simulated outage: items are retried and none are lost
- [ ] Simulated rejection: item lands in the failed list, alert fires
- [ ] Retry does not create a duplicate
- [ ] Absence alert fires when nothing arrives
```

The row people skip is **"nothing arrives at all"**. Errors are easy to
alert on. A missing result produces no error, so you need a check that looks
for what should have arrived.

## Example (fictional)

> **Fictional.** Invented systems and thresholds.

Failure path: **Shop orders → print supplier API**. Owner: Danish. Backup:
Mehak.

| Failure | Likely? | Effect if unnoticed |
| --- | --- | --- |
| Supplier API down | Monthly | Orders not printed, customers wait |
| Rejects order (bad address) | Weekly | That order never ships |
| Nothing sent at all (webhook disabled) | Rare | Every order stuck, nobody knows |

- Retries: timeouts and 5xx at 1 min, 10 min, 1 h, 6 h. 4xx not retried;
  goes straight to "Failed orders" list.
- Idempotency: shop order number sent as the supplier's reference.
- Alerts: failed-all-retries → Danish by email within 5 min; no order sent
  for 24 h on a weekday → #ops channel.
- Tests: all four passed on staging 2026-06-11 using the supplier's sandbox.
