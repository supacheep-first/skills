# Queue contract

For a project whose services `heavy-test` can't express. Point the config at your own script:

```ini
[project]
command = ./scripts/my-queue
```

`heavy-test` then hands every invocation to that command, arguments unchanged, run from the
project root. The skill only ever talks to `heavy-test`, so your script must keep its promises:

- **Same commands** — `main up|down|status [service...]`, `setup <worktree>...`,
  `e2e <worktree>... [--before | --before-after] [-- <test args>]`, `exec <dir> -- <command...>`.
  `--before` tests against the main services with nothing swapped; `--before-after` runs both
  sides in one turn of the queue; tests see `HEAVY_TEST_PHASE=before|after`.
- **One at a time, in arrival order** — at most one `e2e`/`exec`/`main up|down` holds the machine
  across every session; the rest wait without a time limit, first come first served, printing
  their position and who is running. A waiter interrupted in line just leaves it — only the holder
  repairs or releases.
- **Evidence outlives worktrees** — `setup` makes each configured shared path in a worktree point
  at the main checkout's, never replacing tracked files.
- **Mains always come back** — every main service a run stopped is running again before the queue
  is released: on success, on failure, on interrupt.
- **Crashes are repaired** — a run killed outright leaves records; the next run that takes the
  queue stops the leftover worktree services and restores the mains before doing its own work.
- **Hands off other processes** — a port held by something the script didn't start is refused
  (`not proven`), never killed.
- **The tested code is the worktree's** — while tests run, every service of a passed worktree is
  that worktree's build, on the port its main service held.
- **Last line is the result** — `RESULT: passed` (exit 0), `RESULT: failed` (exit 1, tests ran
  and went red), `RESULT: not proven — <reason>` (exit 3, a gate couldn't run).
