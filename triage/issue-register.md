# Issue register

**Why:** one ranked list decides the order of work. Without it, the loudest
issue gets fixed first, not the most important one.

Use it when you take over a live system, after an audit, or whenever open
problems no longer fit in someone's head. A spreadsheet version is in
[issue-register.csv](issue-register.csv).

## Severity levels

| Level | Meaning | Examples | Response |
| --- | --- | --- | --- |
| **P0** | Customers, data or revenue are harmed right now | Data publicly exposed; checkout or lead capture broken; site down | Drop other work. Fix or contain today. |
| **P1** | Serious harm is likely soon, or a key flow is degraded | Errors on a main page; no backups; deploys can't be rolled back | Fix this week. |
| **P2** | Real problem with a workaround or limited reach | Broken page for one browser; slow admin screen; missing alert | Schedule into the next cycle. |
| **P3** | Polish, debt or nice-to-have | Copy fixes; minor layout issues; refactors | Batch when convenient. |

Rule of thumb: rank by **impact on customers and the business**, not by how
hard the fix is. Effort goes in the fix plan.

## Columns

| Column | What goes in it |
| --- | --- |
| ID | Short and stable (`ISS-001`). Never reuse an ID. |
| Severity | P0–P3, from the table above. |
| Title | One line a non-engineer understands. |
| Impact | Who is affected and how, in plain words. |
| Status | `Open` → `In progress` → `Fixed` → `Verified`. Also `Won't fix` with a reason. |
| Owner | One named person. Not a team. |
| Fix plan | The intended change, plus any rollback note. |
| Verified by | Who checked the fix in production, how, and when. Empty until then. |

`Fixed` and `Verified` are different statuses on purpose. An issue is closed
only when someone other than the person who changed the code has checked the
result, and the check is written in **Verified by**.

## Template

```markdown
# Issue register: [system name]

Last reviewed: [YYYY-MM-DD] by [name]

| ID | Severity | Title | Impact | Status | Owner | Fix plan | Verified by |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ISS-001 | P0 | | | Open | | | |
| ISS-002 | P1 | | | Open | | | |
```

Review cadence: daily while any P0 is open, weekly otherwise. Re-rank when new
facts arrive.

## Example (fictional)

> **Fictional.** The system, names and dates below are invented to show the
> template filled in.

Issue register: Larkspur Bakery online shop. Last reviewed: 2026-03-04 by
Ayesha.

| ID | Severity | Title | Impact | Status | Owner | Fix plan | Verified by |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ISS-001 | P0 | Contact form emails never arrive | Every enquiry since the host change is lost | Verified | Bilal | Send through an authenticated mail account; set SPF/DKIM/DMARC | Ayesha, 3 test enquiries received in sales inbox, 2026-03-02 |
| ISS-002 | P1 | No backup before deploys | A bad deploy can't be undone | Fixed | Bilal | Automated backup step in release checklist | |
| ISS-003 | P1 | Shop page returns error 500 on sale items | Customers can't buy discounted items | In progress | Sara | Fix null price handling; add test | |
| ISS-004 | P2 | Order confirmation page slow on mobile | Some customers refresh and double-order | Open | Sara | Compress images; check duplicate-order guard | |
| ISS-005 | P3 | Footer year is out of date | Looks unmaintained | Open | Bilal | Render year dynamically | |
