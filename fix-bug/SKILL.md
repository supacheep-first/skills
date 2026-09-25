---
name: fix-bug
description: Fix bug loop — confirm the goal with the user, reproduce with a failing test, find root cause, plan the fix, delegate to a specialist agent, review, loop to green, then hand off for DoD evidence or a live test. Use when the user reports a concrete bug (repro steps, a ticket, a hotfix branch) and wants it fixed and shipped, or when a bug turns up mid-session during other work — not for open-ended diagnosis (diagnosing-bugs) or a trivial one-line fix.
---

# fix-bug

For a reported bug with a fix to ship — not the ba→sa→wayfinder pipeline, which is for scoping new
feature work. Two entry points:

- **Fresh** — a bug report, ticket, or hotfix branch, nothing dug into yet. Start at 0.
- **Mid-flight** — a bug turns up while doing other work and root cause is already clear from that
  work. Still open with 0, then skip straight to 3; write the repro test alongside or after the fix
  instead of before it.

Seven steps, in order. Each ends red or green, plan or no plan — checkable, not vibes.

## 0. Confirm goal

Invoke `grilling` before touching anything: state the bug as one falsifiable sentence — symptom,
expected vs. actual, and how far the fix should reach (this exact report, or the whole class of
it) — and get the user's confirmation or correction. The repro test in step 1 is this goal made
executable; don't write it until the goal is settled.

## 1. Repro — go red

Write a test that reproduces the reported symptom before touching the fix. Run it: it must fail
for the reported reason, not just fail. No repro test is possible (a pure UI/visual bug) → say so
and move to Root.

## 2. Root

Read the real code and real data — grep, DB, logs — over the first plausible story. Two checks
that pay for themselves:

- **Sibling check** — if this codebase runs structurally similar modules or repos side by side
  (e.g. an admin variant and a people-facing variant of the same feature), check whether the
  sibling already solved the same problem before concluding a fix needs a schema change or new
  plumbing, and reuse its approach.
- **Live check** — an assumption about "how the data always looks" is a guess until a live query
  confirms it across more than one row.

Root is done when you can point to the exact mechanism producing the failure, with evidence, not
inference. Genuinely unclear after a real look → `diagnosing-bugs` is the deeper loop for that;
come back here with a mechanism in hand.

## 3. Grill — plan the fix

Invoke `grilling`, scoped to the decisions this fix actually needs: which approach, how much to
reuse vs. rewrite, and — if a second, unrelated bug turned up in Root — whether it bundles into
this fix or splits into its own ticket (splits into `handoff` to write up).

## 4. Delegate

A root cause + a plan is a fully-defined ticket — hand it to the specialist agent for that layer
(`backend-dev`, `frontend-dev`, ...), the workspace's default for any fully-defined ticket.

Implement it yourself when the fix is small enough to finish in a handful of tool calls (≈5) —
writing the fix and its test costs less than briefing an agent. Delegate everything past that size.

## 5. Review loop

Run `/code-review`. Send findings back to whoever implemented the fix; they fix, then rerun the
repro test and the wider suite. Repeat until the review is clean and every test is green — done is
"review ran clean," not "review ran."

## 6. Close

Before handing off: any doc or feature-map fact this fix made wrong gets fixed now, on the spot —
the workspace already tells you how; this is the checkpoint that stops it sliding to "later."

Then ask which exit: hand off to whatever this project uses for formal test-result evidence (a
DoD/QA agent, if one exists), or invoke `run` to boot the app for a live check. Either ends the
loop.
