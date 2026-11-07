# Rollback runbook

**Why:** a rollback written during an outage is a guess made under pressure.
Write it before the release, when you have time to check each step.

Fill one in per release. Someone other than the author should be able to
follow it without asking questions.

## Template

```markdown
# Rollback: [release tag] — [system]

Author: [name]    Written: [YYYY-MM-DD]    Tested: [yes, on staging, date / no]

## When to use this
Triggers (copied from the release checklist):
- [trigger 1]
- [trigger 2]
Who decides: [name(s)]. If they can't be reached in [N] minutes: [name].

## What we are going back to
- Previous tag: [tag]
- Backup taken: [location, timestamp]
- Database: [migrations in this release are reversible / restore from backup]

## Steps
1. Announce: "Rolling back [tag] because [reason]" in [channel].
2. Maintenance mode on: [command or action]
3. Redeploy previous tag: [command or action]
4. Database: [reverse migration command] OR [restore command + backup file]
5. Restore config/env changes: [what to revert]
6. Clear caches: [what and how]
7. Maintenance mode off.

## Check it worked
- [ ] [Critical flow 1] works
- [ ] [Critical flow 2] works
- [ ] Error logs quiet for [N] minutes
- [ ] Version shown/reported is [previous tag]

## Data written during the bad release
What may have been created or changed between deploy and rollback, and how we
find and fix it: [orders, form submissions, user edits]

## After
- [ ] Announce rollback complete
- [ ] Open an incident report (incidents/incident-report-template.md)
- [ ] Keep the failed release's logs until [date]
```

## Example (fictional)

> **Fictional.** Invented system and steps.

```markdown
# Rollback: v1.8.0 — Larkspur Bakery shop

Author: Bilal    Written: 2026-03-09    Tested: yes, on staging, 2026-03-09

## When to use this
- Checkout fails for any test order
- New 500 errors on shop pages in the first 15 minutes
Who decides: Bilal. If not reachable in 10 minutes: Ayesha.

## What we are going back to
- Previous tag: v1.7.2
- Backup: /backups/2026-03-10/db.sql.gz and files.tar.gz, 21:55 PKT
- Database: one migration, adds a nullable column; reversible

## Steps
1. Post in #shop-ops: "Rolling back v1.8.0 because [reason]".
2. `php artisan down`
3. `git checkout v1.7.2` and run the deploy script
4. `php artisan migrate:rollback --step=1`
5. No config changes in this release
6. `php artisan optimize:clear`
7. `php artisan up`

## Data written during the bad release
Orders placed 22:00–rollback: export from the orders table by created_at and
check each total by hand.
```
