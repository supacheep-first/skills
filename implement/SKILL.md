---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
---

Implement the work described by the user in the spec or tickets.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, use /code-review to review the work.

Commit your work to the current branch.

## Working a ticket loop

When the work comes from a set of tickets (`/to-tickets`), they are a **build loop**, not a checklist. Open `HOW-TO-WORK.md` next to them first: it holds the board, the rules, and the answers earlier sessions already got from the user.

Everything below applies **inside this loop only**. It says nothing about how to work outside it.

1. **Take one ticket from the frontier** — open, every blocker closed, unclaimed. If the user named one, use that.
2. 🔴 **Decide who builds it — before you open a single source file.** A ticket from `/to-tickets` is defined work by construction (What to build · acceptance criteria · traps · a spec it points at), so the default is to **hand it to the specialist dev agents, one per repo, launched in parallel** — not to build it yourself. Check the project's own agent registry for who those are.
   Build it yourself only when the spec is still forming as you go; **if you choose that, write the reason into the ticket's `Rounds`** so the next session can see it was a decision, not a drift.
   **This step is worthless after the fact.** Read source first and delegation always *feels* uneconomical — the context is already in your head. That feeling is an artifact of the order you opened files in, not evidence about the work.
   You still own everything else: the three gates, the review, the feature map, closing the ticket, and shutting the agents down when it closes.
3. **Claim it before any work**: write your name *and today's date* into **Assignee**. A claim dated before today is dead — take the ticket, don't wait for anyone to release it.
4. Build the whole vertical slice: it has to be demoable on its own.
5. **Pass all three gates, in order. A failed gate means staying on this ticket, never moving to the next one.**
   - **Gate 1** — this repo's runnable check is green.
   - **Gate 2** — `/code-review` leaves no blocker. Pass the spec's path yourself if the review can't find it. **Append what it found to the ticket, one line per round** (`round 1: <finding>`) — the next round reads that line instead of relying on memory.
   - **Gate 3** — an e2e spec covers *this ticket's own* behaviour and passes **against `local`** (the project's own local e2e command). The `dev`/staging variant of that command is the user's own command to re-run after a PR/deploy — not a gate you wait on. `local` usually has no fixtures of its own: copy the `dev` env file into a `local` one (gitignored) and point only the base/API URLs at localhost — the local backend typically still hits the same shared dev database (VPN/tunnel, whatever this project uses), so the fixture data is valid there too. A plain spec file: no report, no screenshots, no PDF. Extend an existing flow or helper before writing a new file; what you leave behind is the regression asset the end-of-feature DoD run reuses instead of writing from scratch. Portable by construction: never import a hardcoded browser-attach helper (one that needs a specific machine's Chrome left open — that's for the DoD/CI-only run), read every value through the project's own e2e env module (no new ad-hoc `process.env` names), and data the spec cannot create itself goes through that project's env-var-per-target convention.
     A ticket with no user-facing behaviour (a prefactor, one batch of a wide refactor) writes no new spec: the gate is that the **existing suite stays green**.
6. Refresh the feature map and record anything that cost unreasonable time as a gotcha, then close the ticket.
7. **One ticket per session.** Stop there and report; the next session takes the next frontier ticket.

When the frontier is empty, don't take more work: write the **`## Build status`** section at the end of `HOW-TO-WORK.md` and stop.

### Bring the environment up yourself

Gate 3 drives a real browser, so the app has to be running. Do this before the gate, and **read the failure before touching any code**:

1. Ask the running app first (health endpoint / the port the app serves).
2. No answer → start the backend **in the background**, then poll that endpoint until it answers or ~90s pass.
3. Still nothing → read the startup log and split the cause:
   - **can't reach the database** (VPN, network, credentials) → **stop and tell the user.** Never edit code over this.
   - anything else → report what the log said, then stop.

**Gate 3 red while Gate 1 is green is an environment suspicion first, a code suspicion second.** Chasing it as a code bug is the worst failure mode of an unattended run.

### Stop and ask — these are not yours to decide

- **Database shape**: a new file under `db/migration/`, a new `@Column` / `@SequenceGenerator`, anything that changes a legacy table. Report it, don't write it.
- **Another repo's code.** The path you're about to edit lives outside the repo this ticket belongs to.
- **Another team's documents.** You may fix the spec of the feature you are building (its own `api-spec.md` / `business-logic.md`) when a gate proves it wrong — that is how a discovery here reaches the people upstream. You may not touch documents another team owns.
- **`git push`.** Committing is fine; publishing is the user's call. (This is a rule of this loop, not a rule about pushing in general.)
- **The same review blocker twice.** Two rounds on one finding means the ticket is wrong, not the code. Stop.
- **An e2e spec that has to create or modify real data.** Ask once, before writing it; **write the answer into `HOW-TO-WORK.md`** so later tickets read it instead of asking again.

Whenever you stop for any of these: **clear your name from Assignee** and write one line in the ticket saying what blocked it, so it returns to the frontier carrying its reason.
