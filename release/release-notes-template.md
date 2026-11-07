# Release notes

**Why:** people who use the system need to know what changed for them, not
which files changed. Write for the reader, not the commit log.

## Template

```markdown
# [Product] [version] — [YYYY-MM-DD]

## What's new
- [Change in user terms: what you can now do]

## Fixed
- [Problem as the user saw it] ([issue ID])

## Changed
- [Behaviour that works differently now, and what to do about it]

## Known issues
- [Open problem, workaround, expected fix]

## Do you need to do anything?
[Nothing / Clear your browser cache / Re-enter X / Tell support if Y]

Questions: [contact or channel]
Rollback plan exists: [yes, link for internal readers]
```

## Example (fictional)

> **Fictional.** Invented product and changes.

```markdown
# Larkspur Bakery shop 1.8.0 — 2026-03-10

## What's new
- Product photos load faster on phones.

## Fixed
- Sale items could not be added to the basket; they can now (ISS-003).

## Changed
- Nothing that changes how you use the shop.

## Known issues
- The order confirmation page can still be slow on older phones. Please
  don't refresh it; your order went through. Fix planned for 1.9.0.

## Do you need to do anything?
Nothing.

Questions: #shop-ops
```
