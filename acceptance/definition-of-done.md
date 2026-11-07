# Definition of done

**Why:** projects stall at sign-off when "done" was never written down as
something you can check. Agree it before the work starts.

## Rules

1. **Done is checkable.** Each item is a yes/no someone can verify, not a
   feeling.
2. **If an old process produces an answer, matching it is the first
   acceptance test.** When you replace a spreadsheet, a manual report or a
   legacy tool, its output is the standard people already trust. Pick
   benchmark cases and require the new system to give the same answer. Agree
   the tolerance up front (exact, or to N decimal places).
3. **Verified in the real environment.** "Works on my machine" or "works on
   staging" is not done for anything customers touch.
4. **Evidence is attached.** A test run link, a screenshot, a log line. Not
   "should work".
5. **The failure path is done too.** Errors, retries and alerts are part of the
   scope (see [failure-path checklist](../integrations/failure-path-checklist.md)).

## Template

```markdown
# Definition of done: [feature / deliverable]

Agreed by: [names]    Date: [YYYY-MM-DD]

## Must match the old answer (if replacing something)
| Benchmark case | Old system output | New system output | Tolerance | Match? |
| --- | --- | --- | --- | --- |
| | | | | |

## Functional
- [ ] [User can do X]
- [ ] [Rule Y is enforced]

## Quality
- [ ] Test gate passed (acceptance/test-gate-policy.md)
- [ ] Works at [screen sizes / browsers / devices]
- [ ] Accessibility: [standard, e.g. WCAG 2.1 AA] checked with [tool]

## Failure path
- [ ] What happens on error is defined and tested
- [ ] Alerts go to [owner]

## Delivery
- [ ] Verified on production by [name], evidence: [link]
- [ ] Docs / release notes updated
- [ ] Rollback exists

Sign-off: [name, date]
```

## Example (fictional)

> **Fictional.** Invented tool and numbers.

Definition of done: **Freight cost calculator** replacing the
"Shipping_Rates_v7.xlsx" workbook. Agreed by Omar (ops) and Hina (dev).

| Benchmark case | Old system output | New system output | Tolerance | Match? |
| --- | --- | --- | --- | --- |
| 12 kg, Zone B, express | 4,380.00 | 4,380.00 | exact | ✅ |
| 0.4 kg, Zone A, standard | 610.00 | 610.00 | exact | ✅ |
| 85 kg, Zone D, standard | 18,925.50 | 18,925.50 | exact | ✅ |

- [x] Ops can enter a shipment and get a cost without opening Excel
- [x] Weight over 100 kg shows "quote manually" instead of a number
- [x] Test gate passed: CI run #48
- [x] Verified on production by Omar with the three cases above, 2026-04-02
