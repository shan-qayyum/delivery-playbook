# Test gate policy

**Why:** someone has to decide what a release must pass before it ships. If
nobody owns that decision, customers end up testing for you.

A test gate is a list of checks that must all pass before a release goes out.
If any check fails, the release stops. No "ship it and fix later" without a
named person accepting the risk in writing.

## Principles

- **Automated where it repeats.** If a check runs every release, automate it.
  Clicking through by hand is how steps get skipped.
- **One gate per release channel.** If you ship to several targets (web +
  mobile, or several package formats), each target passes its own gate. Four
  packages are four ways to break.
- **Red means stop.** A flaky test is fixed or removed with a reason, never
  ignored to get a release out.
- **The gate has an owner** who decides what is in it and approves exceptions.

## Template

```markdown
# Test gate: [product]

Gate owner: [name]    Last reviewed: [YYYY-MM-DD]

## Applies to
Release channels: [e.g. web production, Android, Debian package]

## Must pass (blocking)
| Check | How | Where it runs | Applies to channel |
| --- | --- | --- | --- |
| Unit tests | [command] | CI | all |
| End-to-end: [critical flow] | [Playwright / Cypress suite] | CI | [channel] |
| Build / package installs cleanly | [command] | CI | [channel] |
| Migrations run on a copy of production data | [command] | staging | web |
| Accessibility checks | [tool, standard] | CI | web |

## Should pass (warning, owner decides)
| Check | How |
| --- | --- |
| Performance budget | [tool, threshold] |

## Exceptions
A blocking check can be skipped only if [gate owner] writes down: what is
skipped, why, the risk, and when it will be fixed. Recorded in [place].

## Evidence
Every release links the passing run in its release checklist.
```

## Example (fictional)

> **Fictional.** Invented product and suites.

Test gate: **Example Notes desktop app**. Gate owner: Zara.

| Check | How | Where it runs | Applies to channel |
| --- | --- | --- | --- |
| Unit tests | `npm test` | CI | all |
| E2E: sign in, sync, offline edit | Playwright, 9 suites | CI | all |
| Package installs and launches | install script per format | CI matrix | .deb, Flatpak, AppImage |
| Upgrade from previous version keeps data | scripted install-then-upgrade | CI matrix | all |

Exception log: 2026-05-14, AppImage launch test skipped (CI runner image
broken). Risk accepted by Zara; AppImage release held back one day until the
check passed.
