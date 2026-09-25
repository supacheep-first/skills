---
name: open-pr
description: "Open or update a merge/pull request via `glab` (GitLab) or `gh` (GitHub) — draft-first with explicit confirmation, an optional branch-pairing convention, and the CLI flag gotchas that bite. Use whenever asked to open, create, raise, submit, draft, or send a PR/MR; to get a branch ready for review; or to adjust an already-open one's settings (assignee, squash, delete-branch)."
---

# Open PR/MR

Confirm the CLI is authed first: `glab auth status` (or `gh auth status`) — never ask for or handle the token yourself. Logged out → tell the user to run the login command themselves in their own terminal; that step is theirs, not yours.

🔴 **Never run `glab mr create` / `gh pr create` without the user's explicit go-ahead on the draft first — no exceptions, not even for an obvious one-liner hotfix.** Build the full draft (title, source → target, description body, assignee, squash/delete-branch flags), show it, and wait for a clear yes before creating anything.

## Steps

1. **`cd` into the target repo directory first** when several repos are checked out side by side. `glab mr create -R <path>` alone is not enough — `glab` infers the *source* project from the shell's cwd regardless of `-R`, and posts to whatever repo you're sitting in. Confirmed failure mode: running from a sibling repo's directory posts the MR against that sibling and 422s with "Source project is not a fork of the target project."
2. Check the branch doesn't already have an MR/PR open: `glab mr list --all --source-branch <branch>` (any state — a merged/closed one still counts as already opened).
3. Work out the target branch from this project's branch-pairing convention, if it has one (see below for a worked example). Title format and language follow the project's own convention — check for an existing pattern (recent MR titles, a CONTRIBUTING doc) rather than guessing; **never reuse the commit subject** as the MR/PR title, they serve different readers.
4. Write the description to a temp `.md` file and pass it with `--description-file` — inline `--description` breaks on non-ASCII text and embedded quotes. Typical shape: a ticket-tracker link on the first line, a one-line summary, then a bullet list from `git log --oneline origin/<target>..origin/<branch>` (left as-is, in the commits' own words). Delete the temp file once the MR/PR is created.
5. **Show the full draft — repo, source → target branch, title, description body, assignee, squash/delete-branch flags — and stop.** Wait for the user's explicit confirmation. One MR, one draft, one confirmation; batch drafts together when opening several at once, same as one confirmation covering all of them.
6. Only after that confirmation, create it, with whatever this project's own defaults are, e.g.:
   ```
   glab mr create --source-branch <branch> --target-branch <target> \
     --title "<title>" --description-file <tmp-file> \
     --remove-source-branch=false --squash-before-merge=true \
     --assignee <username from `glab auth status`> --yes
   ```
   Boolean flags need the `=` form — `--remove-source-branch false` makes glab read `false` as a positional and fail with `Unknown command "false"`.
7. Report back the MR/PR link(s) only — nothing else needs saying unless something failed.

## Branch-pairing convention (worked example)

Some orgs open **two** MRs per ticket instead of one: a `-dev`/`_for_dev` branch → a dev/integration branch, and a plain/`-uat` branch → a UAT branch **directly** (not through a periodic release-batch branch — those are batch merges, not per-ticket work). Seeing only the dev-track MR open for a ticket is a signal the UAT-track branch/MR is still missing — confirm with `git rev-list --count origin/<uat-branch>..origin/<plain-branch>` before opening it. Reuse the dev MR's title for the UAT counterpart, with a suffix like ` [UAT]` appended.

If the project you're in doesn't split this way, skip this section entirely — one branch, one MR.

## Changing settings on an already-open MR

Same flags work on `glab mr update <iid>`, but `--remove-source-branch` and `--squash-before-merge` are **toggles** there (no true/false value) — check the MR's current state first (`glab api projects/<url-encoded-path>/merge_requests/<iid>`, fields `squash` and `force_remove_source_branch`) so a toggle lands on the intended value instead of flipping blind.
