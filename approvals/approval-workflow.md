# Approval workflow

**Why:** when approval happens over email or chat, nobody can say later which
version was approved, by whom, or whether it changed afterwards. Make approval
a stage in the system, not a message about it.

This is a generic pattern. It works in an ERP (ERPNext/Frappe workflows), a
document system, or a custom app with a `status` field and role checks.

## Stages

```text
Draft ──submit──▶ In Review ──approve──▶ Approved
  ▲                  │
  └────reject────────┘  (with a written reason)
```

| Stage | Meaning | Who can edit content |
| --- | --- | --- |
| Draft | Being written. Not for use. | Author |
| In Review | Frozen for the reviewer. | Nobody (edit lock) |
| Approved | Signed off. Safe to use. | Nobody. Changes need a new revision. |

Optional extra stages, only if you need them: `Superseded` (a newer revision
was approved), `Withdrawn` (no longer valid). Keep the main path to three
stages; every extra stage is another place work can wait.

## Roles and permissions

| Action | Author | Reviewer | Approver | Admin |
| --- | --- | --- | --- | --- |
| Create draft | ✅ | | | ✅ |
| Edit draft | ✅ (own) | | | ✅ |
| Submit for review | ✅ (own) | | | |
| Edit while In Review | | | | |
| Reject back to Draft | | ✅ | ✅ | |
| Approve | | | ✅ | |
| Approve own document | | | ❌ | ❌ |
| Start new revision of Approved | ✅ | | | ✅ |
| Delete Approved | | | | ❌ |

Rules that matter more than the table:

1. **Separation:** the person who wrote it can't approve it.
2. **Edit lock:** once submitted, nobody edits it under the reviewer. To
   change it, reject it back to Draft. Even admins don't edit In Review.
3. **Reasons are required** on reject. A rejection without a reason is just a
   delay.
4. **Every transition is logged:** who, when, from-stage, to-stage, comment.

## Immutable snapshot on approval

When a record is approved, save a frozen copy of everything that was approved:
the content, the generated document (e.g. the PDF), and the values pulled in
from other records at that moment.

Why: approved documents often pull data from shared lists (a hazard library,
a price list, a template). If someone edits that shared list next month, the
approved document must not change silently. The snapshot is what was signed.

Implementation notes:

- Store the snapshot as a separate, read-only record linked to the source,
  with the approver, timestamp and a content hash.
- Generate the output document once, at approval, and store it. Don't
  regenerate it on view.
- Changes after approval create **revision 2** as a new Draft. Revision 1
  stays Approved until revision 2 is approved, then becomes Superseded.

## Template: workflow spec

```markdown
# Approval workflow: [document/record type]

Purpose: [what is being approved and why approval matters]

## Stages
| Stage | Meaning | Who can edit |
| --- | --- | --- |
| Draft | | |
| In Review | | Nobody |
| Approved | | Nobody |

## Transitions
| From | To | Action | Allowed roles | Required input |
| --- | --- | --- | --- | --- |
| Draft | In Review | Submit | Author | |
| In Review | Draft | Reject | Reviewer, Approver | Reason |
| In Review | Approved | Approve | Approver (not author) | |
| Approved | Draft (new revision) | Revise | Author | Reason for change |

## Snapshot on approval
Fields frozen: [list]
Linked data copied in: [list]
Generated output stored: [PDF / file]

## Notifications
| Event | Who is told | How |
| --- | --- | --- |
| Submitted | Reviewer | |
| Rejected | Author | |
| Approved | Author + [users of the document] | |

## Acceptance tests
- [ ] Author cannot approve own document
- [ ] Nobody can edit In Review (test with admin too)
- [ ] Editing linked data after approval does not change the approved output
- [ ] Every transition appears in the log with user and time
```

## Example (fictional)

> **Fictional.** Invented organisation and document type.

Approval workflow: **Site visit risk assessment** at "Harbor Electrical".

- Stages: Draft → In Review → Approved.
- Roles: Field Engineer (author), Safety Lead (reviewer), Operations Manager
  (approver).
- Transitions: Field Engineer submits; Safety Lead or Operations Manager can
  reject with a reason; only the Operations Manager approves, and never their
  own assessment.
- Snapshot: on approval, the hazard list, controls, site address and the
  generated PDF are copied into a read-only snapshot. Editing the shared
  hazard library later doesn't change the approved PDF.
- Notifications: the Safety Lead gets a task on submit; the crew lead gets the
  approved PDF.
- Acceptance tests: all four checks above passed on staging before go-live,
  run by the Safety Lead with a test account per role.
