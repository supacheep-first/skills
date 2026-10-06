---
name: fix-bug-parallel
description: fix-bug for one of several bugs running side by side — one session per bug, each in its own git worktree, heavy tests queued one at a time by the bundled heavy-test script.
disable-model-invocation: true
---

# fix-bug-parallel

One bug per Claude session; several sessions run side by side. Each session runs `fix-bug` — read
`../fix-bug/SKILL.md` (beside this skill's directory) and follow its steps. This file holds only
what changes when other bugs run beside yours.

## Layout

- **Main checkouts** stay on the project's main branch and run the **main services** — the app as
  everyone else sees it. Bug work happens in worktrees.
- **Worktree** — one per repo the bug touches, at `<project root>/.worktrees/<repo>-<ticket>`, the
  same branch name in every repo, branched from the PR/MR's target branch.
- **Queue** — `heavy-test`, the script in this skill's directory (`heavy-test help` for usage; give
  agents its absolute path). Every heavy test runs through it, one at a time across all sessions.
  It owns the main services: starts them, records their process groups, swaps a worktree's build
  onto the ports a run needs, and brings the mains back before releasing the queue.

**Fast** — run any time, in your worktree: typecheck, lint, format, a unit test that touches
nothing shared. A test that starts its own server on a free port (port 0) is fast too.
**Heavy** — anything that needs a fixed port, the running app, or a shared database: e2e,
integration suites, a full build. Heavy runs only through the queue.

## Project file

Every project fact lives in **`.claude/heavy-test.ini`**, the first found walking up from the
current directory (a workspace of several repos keeps one at its root), plus
`.claude/heavy-test.local.ini` beside it for machine-only values such as `JAVA_HOME` — keep that
one out of git.

No file → build it with the user before step 1:

1. Find each service the heavy tests need running — dev-server scripts, framework defaults, the
   app's own config, a compose file — with its port, start command, and a URL that answers 200
   once it's ready. Find the gitignored files a fresh checkout needs to run
   (`git status --ignored`).
2. Draft the file from `heavy-test.example.ini` in this skill's directory and show it.
3. On the user's yes, write it and run `heavy-test main up`. Done when `heavy-test main status`
   shows every service up.

A project the script can't express brings its own queue under the same contract —
[`CONTRACT.md`](CONTRACT.md).

## Changes per step

- **Before 1 — set up the worktree** for the repo the repro test lives in:
  `git -C <repo> worktree add <project root>/.worktrees/<repo>-<ticket> -b <branch> origin/<target>`
  (an absolute path: `git -C` resolves a relative one inside the repo), then
  `heavy-test setup <worktree>`. Done when the setup prints `ready`. Root or Plan shows another
  repo is touched → set up its worktree the same way, same branch name.
- **1, 5 — heavy tests go through the queue:** `heavy-test e2e <worktree>... -- <test args>`,
  passing every worktree this bug changed; repos not passed stay on their main services. Run it in
  the background — the queue may wait.
- **4 — the brief** carries the absolute path of each worktree and of `heavy-test`; the agent works
  inside those paths.
- **5 — cross-bug data.** A test that goes red in the loop but green run alone points first at data
  another bug's run left in the shared database: report `not proven` naming the test, rather than
  changing code.
- **6 — clean up** this bug's worktrees and branches in safe mode (`git worktree remove`,
  `git branch -d`); report anything git refuses.
