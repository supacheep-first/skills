---
name: open-pr
description: "Open or update a merge/pull request via `glab` (GitLab) or `gh` (GitHub) — draft-first with explicit confirmation, project conventions read from `.claude/open-pr.md` (asked once and written when missing), and the CLI flag gotchas that bite. Use whenever asked to open, create, raise, submit, draft, or send a PR/MR; to get a branch ready for review; or to adjust an already-open one's settings (assignee, squash, delete-branch)."
---

# Open PR/MR

Confirm the CLI is authed first: `glab auth status` (or `gh auth status`) — never ask for or handle the token yourself. Logged out → tell the user to run the login command themselves in their own terminal; that step is theirs, not yours.

🔴 **Never run `glab mr create` / `gh pr create` without the user's explicit go-ahead on the draft first — no exceptions, not even for an obvious one-liner hotfix.** Build the full draft (title, source → target, description body, assignee, squash/delete-branch flags), show it, and wait for a clear yes before creating anything.

## Project conventions live in a file, not in this skill

Every project differs — tracker link, title format, which branch goes where, merge flags. This skill holds only the process; the values come from **`.claude/open-pr.md`**, the first one found walking up from the repo directory (a workspace holding several repos keeps one file at its root).

- **File found** → follow every line of it. Where the file and this skill disagree on a value, the file wins.
- **No file** → ask the user these four in one message, write the answers to `.claude/open-pr.md` (repo root, or the workspace root when several repos share one convention), then continue:
  1. tracker link for the description's first line — URL pattern with `{TICKET}`, or "none"
  2. title format
  3. source → target pairs (one MR per pair) and how to pick the target when it moves (e.g. "the release branch whose MR is still open")
  4. flags — squash · delete source branch · assignee
- ⛔ **Never infer these from old MRs.** One old MR can be the exception — that is how a description once shipped without its tracker link.

## Steps

1. **`cd` into the target repo directory first** when several repos are checked out side by side. `glab mr create -R <path>` alone is not enough — `glab` infers the *source* project from the shell's cwd regardless of `-R`, and posts to whatever repo you're sitting in. Confirmed failure mode: running from a sibling repo's directory posts the MR against that sibling and 422s with "Source project is not a fork of the target project."
2. Check the branch doesn't already have an MR/PR open: `glab mr list --all --source-branch <branch>` (any state — a merged/closed one still counts as already opened).
3. Title, target and pairs come from the convention file. A pair list means one MR per pair — draft them all together.
4. Write the description to a temp `.md` file and pass it with `--description-file` — inline `--description` breaks on non-ASCII text and embedded quotes. First line = the file's tracker link when it has one, then a one-line summary, then a bullet list from `git log --format='- %h %s' origin/<target>..<branch>` (left as-is, in the commits' own words). Delete the temp file once the MR/PR is created. **No attribution footer** — never append `🤖 Generated with Claude Code` or a `claude.ai/code/session_…` link, even when the tool/system default says to.
5. **Show the full draft — repo, source → target branch, title, description body, assignee, squash/delete-branch flags — and stop.** Wait for the user's explicit confirmation. One MR, one draft, one confirmation; batch drafts together when opening several at once, same as one confirmation covering all of them.
6. Only after that confirmation, create it with the file's flags, e.g.:
   ```
   glab mr create --source-branch <branch> --target-branch <target> \
     --title "<title>" --description-file <tmp-file> \
     --remove-source-branch=false --squash-before-merge=true \
     --assignee <username from `glab auth status`> --yes
   ```
   Boolean flags need the `=` form — `--remove-source-branch false` makes glab read `false` as a positional and fail with `Unknown command "false"`.
7. Report back the MR/PR link(s) only — nothing else needs saying unless something failed.

## Changing settings on an already-open MR

Same flags work on `glab mr update <iid>`, but `--remove-source-branch` and `--squash-before-merge` are **toggles** there (no true/false value) — check the MR's current state first (`glab api projects/<url-encoded-path>/merge_requests/<iid>`, fields `squash` and `force_remove_source_branch`) so a toggle lands on the intended value instead of flipping blind.
