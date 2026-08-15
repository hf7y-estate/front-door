# FOCUS — retired 2026-08-15, migrated to GitHub issues

**The backlog now lives at https://github.com/hf7y/front-door/issues.** This is
the ecosystem-wide policy as of 2026-08-07 (`hf7y/scheduler#66`); the sweep is
`hf7y/realisateur#230` and its root cause is `hf7y/realisateur#187`. This file
is a pointer, not a second source of truth. Do not add work items here — file an
issue.

## Where things went

| Was | Now |
|---|---|
| Shared-host footprint (mandark, hermes, deliberately-not-touched) | moved to [`FOOTPRINT.md`](../FOOTPRINT.md) — a live declaration, not backlog |
| The hermes changes flagged for hermes' owner (`.env` JID fix, `npm install`, service enable) | [#9](https://github.com/hf7y/front-door/issues/9) |
| Cloud doorkeeper routine + its prose-not-mechanism Art. 9 fence | [#10](https://github.com/hf7y/front-door/issues/10) |
| Backlog: mechanize remaining trust boundaries | already [#5](https://github.com/hf7y/front-door/issues/5) — nothing new filed |
| Backlog: `front-door-watch.service` keep-or-retire under the mandark→monkey teardown | already [#8](https://github.com/hf7y/front-door/issues/8) |
| Backlog: "every local transport dies with the machine" | a standing limitation, not a task — kept in [`FOOTPRINT.md`](../FOOTPRINT.md) |
| Backlog: Milestone 001 due 2026-08-02 | **done** — recorded `missed` in `protocol.json`, PR #7 merged `0bfc14d`; the thread is [#4](https://github.com/hf7y/front-door/issues/4) |
| Session records 2026-07-29 / 08-01 / 08-02 (sha tables, doorkeeper sweep, philosophy delta) | **deleted as history** — git already is the changelog |

Two of the six backlog items were already issues, and one was already done. Two
new issues were filed, both for things that existed **only** in this file: the
hermes edits and the cloud doorkeeper's operational record.

## Why the session records are gone, not migrated

Three dated blocks listed shas already in `git log`, restated decisions already
in `protocol.json`, and reported the state of issues #2/#4/#5 as of 2026-08-02.
Two of those three status claims were stale by 2026-08-15 — milestone 001 had
been recorded missed on 2026-08-03 and #8 had opened on 2026-08-06 — which is
exactly the failure this migration ends: an issue moves, a file section just
sits there.

## Producer fixed

Per `#187`, retiring a surface without fixing what writes it just means it comes
back. This repo has no `.claude/commands/`; its producer was `CLAUDE.md`, which
told agents to record footprint here and to use `focus-commit` on it. Both lines
are corrected in the same commit: footprint goes to `FOOTPRINT.md`, everything
else goes to an issue.

Full history — the sha tables, the doorkeeper sweep transcript, and the original
footprint prose — is in git before this commit.
