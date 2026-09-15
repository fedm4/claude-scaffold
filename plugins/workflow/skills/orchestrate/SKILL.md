---
name: orchestrate
description: Use when picking up work from the project backlog to run it end to end — fetching the next item, splitting a plan into small items, briefing and launching one subagent per task, verifying its check, and closing the item with a PR. This is the detailed "how"; the short rules already live in this plugin's rules.md and apply on top of this.
---

# How to run a task from the backlog

The short rules (git, language, verification) already apply here without
repeating. This skill adds the roles and the step-by-step procedure.

## Roles
You (Opus) are the product manager / functional analyst: understand the
problem, split it, write the brief, verify, close the item. You never write
product code. Sonnet is a semi-senior dev: closed tasks only. Haiku is a
junior dev: mechanical tasks only (boilerplate, renames, types,
translations).

A delegated task is always closed: you can name the files it touches, its
signature, and the command that validates it. If you can't write that down,
it's not ready — go back to the plan.

One subagent at a time, sequential, never parallel. The machine verifies:
look at PASS/FAIL, not the diff.

Never delegate: architecture and API design, root-cause debugging, or any
review where judgment is the product.

## 0. Read the project's CLAUDE.md
It names the backlog tool, the docs tool, and the language. If
`backlogs/<tool>.md` exists next to this skill, read it and follow it for
every backlog operation (fetching, creating, closing items). If it doesn't
exist, use that tool's MCP the obvious way.

## 1. Take the next item
Fetch the next item in order (the first not done), together with its
comments — a comment can change scope or carry a decision that isn't in the
item's body.

## 2. Plan or closed task?
If the item describes something big (a feature, a phase, "investigate X and
fix it") rather than a bounded change:
- Split it into small items, each with: what, files it touches, who does it
  (sonnet/haiku/you), the check that validates it, the branch, and what it
  depends on.
- Design decisions go in the parent/epic item, not scattered across the
  split items.
- Create the items in the backlog.
- **Stop here.** A plan is never executed straight from the chat; each small
  item gets picked up later through this same procedure.

If the item is already a closed task (bounded files, executable check),
continue to step 3.

## 3. Branch
Create or switch to the task's branch. Never work on `main`.

## 4. Shared brief in the scratchpad
Before launching a subagent, write a brief with:
- The relevant repo pattern (how similar things are already done nearby).
- The full split of the task (or of every task in the plan, if several
  subagents will read the same brief): who touches which files, so no one
  overwrites another's work.
- Explicit rules for the subagent:
  - No git (no `add`, `commit`, `push`, `checkout`).
  - Run its own check at the end and return the literal output plus
    PASS/FAIL.
  - If the check fails, never adjust the test to make it pass — the test is
    the spec. If the subagent thinks the test itself is wrong, it stops and
    says so instead of touching it.

## 5. Launch one subagent
Sonnet by default; haiku only if the task is purely mechanical. Pass it the
brief from the scratchpad.

## 6. Verify
Run the task's check yourself (the command, not a manual diff read).

- **FAIL 1st time**: relaunch the same subagent with the error as extra
  context.
- **FAIL 2nd time**: you do the task yourself. No 3rd delegated attempt.

## 7. Commit
One commit per small task, never accumulated with others. Stage files by
name (never the whole tree) and check `git status --short` first — there may
be unrelated work in progress that isn't yours to commit.

## 8. Close the item
Write the conclusion inside the item first: what was done, the check
result, the PR link once it exists. Only then mark it done.

## 9. Tidy history before push
Commits tell the story of the change. Squash fixups with
`git commit --fixup <sha>` followed by
`GIT_SEQUENCE_EDITOR=: git rebase -i --autosquash origin/main`; when the
history is too tangled for that, `git reset --soft origin/main` and
recommit in order. Either way, verify the tree didn't change:
`git diff --quiet <old-tip> HEAD`. Push with `--force-with-lease` if the
branch was already pushed and got rewritten. Open the PR against `main` (no
session links in commits, PR body, or comments).

## 10. Final report
One line per task: `[task] · [model] · PASS|FAIL`.

## Review round
Review comments become items named `Review PR #N · <what>` (so they read as
follow-ups, not new PRs). Fix them on the same branch, tidy history again
(step 9), push with `--force-with-lease`, and reply to each comment thread
saying what changed.
