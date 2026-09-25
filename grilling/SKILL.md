---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

A round goes out on one channel, never two at once.

Use **`AskUserQuestion`** — the click-to-answer popup — when the _whole_ round fits it: at most 4 questions, at most 4 choices each, every choice nameable in a few words plus a one-line description, and no question needing context before it can be judged. Put the recommended choice first and mark it in its label.

If a single question in the round fails any of those, the whole round goes out as **markdown blocks** instead. Never split one round across both channels: the popup ends your turn the moment the user clicks, so any markdown question asked alongside it is left unanswered and has to be repeated.

Each question is **one block**, fenced off from its neighbours by a horizontal rule. Never let two questions run together as adjacent paragraphs: a round of five questions must read as five blocks, not one wall of text.

```
---

❓ **Q1 · <short title>**

<body: a line or two of context — what is undecided and why it matters>

- **(a)** <choice>
- **(b)** <choice>
- **(c)** <choice>

> ➡️ **(a)** — <why this one>

---
```

Inside the block:

- **Title sits on the `Q` line and stays short enough for one line.** Context belongs in the body, never in the title.
- **Choices are always a bullet list**, one per line, each labelled. A question with no discrete choices still keeps body and recommendation as separate blocks.
- **The recommendation is a blockquote and always the last line**, separated by a blank line. Name the choice, then the reason, on one line.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
