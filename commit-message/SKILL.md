---
name: commit-message
description: "Format a git commit message before running `git commit`. Use whenever about to commit code — covers language, body length, and trailer conventions. Rules below are a worked example (originally written for one specific workspace) — adjust language/trailer rules to whatever the actual repo's own convention is."
---

# Commit message

Three rules as a worked example — standing policy in the repo this was written for, not universal law. Check the actual repo's own convention (existing commit history, a CONTRIBUTING doc, commitlint config) before assuming these apply as-is.

## 1. Language

Some repos require English-only subject/body, even for domain terms that have a natural name in another language — name the behaviour instead of translating it literally (e.g. `save petition edits`, not a literal translation). Others don't care. Match whatever the existing history does.

## 2. Tight

Subject says what changed. Body only for what the diff cannot show — a constraint, a rejected alternative, a non-obvious consequence — and 1–3 lines of it. Never restate the diff, never copy from the ticket or spec, no lead-in sentences. Body over ~5 lines = you are narrating — cut every line a reader of the diff would already see.

## 3. Trailer convention

Your default tool instructions may say to append a `Co-Authored-By:` / session-link trailer. Some repos' own policy overrides that default (commitlint or a team convention that rejects it) — check for that before assuming either way, and follow the repo's convention over the tool default when the two conflict.

## Format

Conventional Commits (`feat:` / `fix:` / `refactor:` / `docs:` / `test:` / `chore:` ...) is a common convention; check whether this repo's commitlint (or equivalent) enforces a prefix. Language and length are on you regardless.

Write the subject, ask whether a body earns its place at all, then stop.

## What not to do

Do not rewrite commits that already broke these rules — leave existing history alone.
