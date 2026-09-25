---
name: to-tickets
description: Break a plan, spec, or the current conversation into a set of tracer-bullet tickets, each declaring its blocking edges, published to the configured tracker (edges as text in one file per ticket locally, or native blocking links on a real tracker).
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into a set of **tickets**: tracer-bullet vertical slices, each declaring the tickets that **block** it.

The issue tracker and triage label vocabulary should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec path, an issue number or URL) as an argument, fetch it and read its full body and comments.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Ticket titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change easy, then make the easy change."

### 3. Draft vertical slices

Break the work into **tracer bullet** tickets.

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests): vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Any prefactoring should be done first

</vertical-slice-rules>

Give each ticket its **blocking edges**: the other tickets that must complete before it can start. A ticket with no blockers can start immediately.

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change (rename a column, retype a shared symbol) whose **blast radius** fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a tracer bullet; sequence it as **expand–contract**. First expand: add the new form beside the old so nothing breaks. Then migrate the call sites over in batches sized by blast radius (per package, per directory), each batch its own ticket blocked by the expand, keeping CI green batch to batch because the old form still exists. Finally contract: delete the old form once no caller remains, in a ticket blocked by every migrate batch. When even the batches can't stay green alone, keep the sequence but let them share an integration branch that all block a final integrate-and-verify ticket; green is promised only there.

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each ticket, show:

- **Title**: short descriptive name
- **Blocked by**: which other tickets (if any) must complete first
- **What it delivers**: the end-to-end behaviour this ticket makes work

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the blocking edges correct: does each ticket only depend on tickets that genuinely gate it?
- Should any tickets be merged or split further?

Iterate until the user approves the breakdown.

### 5. Publish the tickets to the configured tracker

Publish the approved tickets. **How** depends on the tracker `/setup-matt-pocock-skills` configured; the tickets are the same either way, only the shape of the blocking edges changes:

- **Local files** → write one file per ticket under `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` in dependency order (blockers first). Each file's "Blocked by" lists the numbers/titles it depends on. Use the per-ticket file template below: one ticket per file, never a single combined file.
- **A real issue tracker (GitHub, Linear, …)** → publish one issue per ticket in dependency order (blockers first) so each ticket's blocking edges can reference real identifiers. Use the platform's native blocking / sub-issue relationship where it has one; otherwise set each ticket's "Blocked by" to the blocking issues. Apply the `ready-for-agent` triage label unless instructed otherwise; the tickets are agent-grabbable by construction.

Work the **frontier**: any ticket whose blockers are all done, minus anything already claimed (see **Assignee**). For a purely linear chain that means top to bottom.

**On the local-files tracker, also write `.scratch/<feature-slug>/issues/HOW-TO-WORK.md`** next to the tickets: how to enter the loop (the command to type), the frontier rule, the claim rule, the three gates, one ticket per session, and an ASCII board of the tickets with their blocking edges. It is the first file any session opens; the tickets are the detail behind it.

Give it a **`## Answers already given`** section, empty to start. Every answer the user hands a session mid-loop goes there — which environment to test against, whether an e2e spec may create real data, a direction already ruled out. A loop that asks the same question every ticket is no better than doing the work by hand, and no session can remember across the others.

### 6. Close the loop

The tickets are a **build loop**, the mirror of a wayfinder map: one ticket per session, claim before work, close only through the gates, repeat until the frontier is empty.

When the last ticket closes, that session writes a **`## Build status`** section at the end of `HOW-TO-WORK.md` and does nothing else: what now works end to end, which tickets are closed, what is left, and who owns it. Without it the next reader finds a wall of closed tickets and no way to tell whether the feature is shippable.

Anything the loop produced but did not finish (a full DoD report, another team's work, an open question the spec already carries) goes in that section's table, never back into a ticket.

Do NOT close or modify any parent issue.

<local-ticket-template>

# <NN>: <Ticket title>

**What to build:** the end-to-end behaviour this ticket makes work, from the user's perspective, not a layer-by-layer implementation list.

**Blocked by:** the numbers/titles of the tickets that gate this one, or "None (can start immediately)".

**Status:** ready-for-agent

**Assignee:** _(empty = unclaimed. A session claims the ticket by writing its name **and today's date** here before any work; a claim dated earlier than today is dead and the ticket is takeable again — so an interrupted session can never park a ticket forever.)_

- [ ] Acceptance criterion 1
- [ ] Acceptance criterion 2

## Done when all three gates pass

- [ ] **Gate 1 — runnable check green** (this repo's own bar: typecheck / lint / build / unit tests)
- [ ] **Gate 2 — `/code-review` leaves no blocker**
- [ ] **Gate 3 — an e2e spec covers this ticket's own behaviour and `pnpm test:e2e <spec>` (local) passes** (a plain spec file, no report, no screenshots; extend an existing flow/helper before writing a new file; no `cdp.ts`, values only via `tests/e2e/env.ts`; `local` has no fixtures of its own — copy `.env.dev` into `.env.local`, gitignored, change only the base/API URL to localhost. `pnpm test:e2e:dev` is the user's own command after a PR/deploy, not a gate)
- [ ] Feature map refreshed, and anything that cost unreasonable time recorded as a gotcha

## Rounds

_(one line per failed review round — `round 1: <finding>` — so the next round reads it instead of remembering it. The same finding twice means the ticket is wrong: stop and ask.)_

</local-ticket-template>

<issue-template>

## Parent

A reference to the parent issue on the tracker (if the source was an existing issue, otherwise omit this section).

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective, not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Blocked by

- A reference to each blocking ticket, or "None (can start immediately)".

</issue-template>

In either form, avoid specific file paths or code snippets: they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.
