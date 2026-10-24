# Undoing Changes in Git

> Learn to undo Git changes safely by identifying whether they are unstaged, staged, committed locally, or already shared with a remote.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Identify where a change currently exists.
- Discard unstaged changes and unstage changes without deleting them.
- Explain how `git reset` changes the current branch and working state.
- Use `git revert` to reverse a commit by creating a new commit.
- Choose a safer approach when commits have already been pushed or shared.

## Contents

- [Start with the Change's Location](#start-with-the-changes-location)
- [1. Discard Unstaged Changes](#1-discard-unstaged-changes)
- [2. Unstage Changes](#2-unstage-changes)
- [3. Undo the Latest Local Commit](#3-undo-the-latest-local-commit)
- [4. Understand `git reset`](#4-understand-git-reset)
- [5. Use `git revert`](#5-use-git-revert)
- [Reset vs. Revert](#reset-vs-revert)
- [What If the Commit Was Pushed?](#what-if-the-commit-was-pushed)
- [Safe Habits Before Undoing Changes](#safe-habits-before-undoing-changes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## Start with the Change's Location

Git commands differ depending on whether a change is in the working directory, the staging area, a local commit, or a commit that has already been shared.

```text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Commit
       |
       | git push
       v
Shared Remote History
```

Before running an undo command, ask:

1. Is the change unstaged, staged, committed, or pushed?
2. Do I need to keep the work, or discard it?
3. Have other people already based their work on this commit?

When in doubt, inspect the state and preserve a backup before using a destructive command.

## 1. Discard Unstaged Changes

If you changed a tracked file but have not staged the latest edits, you can restore the file:

```bash
git restore analysis.py
```

This restores the working-tree version of `analysis.py` to the version in the index (the staging area). If the file has no staged changes, this usually restores it to the current commit's version.

**Warning:** This discards the unstaged edits to that file. Git may not be able to recover those edits afterward.

Check the state first:

```bash
git status
git diff
```

Use `git restore <file>` only when you are sure you do not need the unstaged changes.

## 2. Unstage Changes

If you staged a file with:

```bash
git add analysis.py
```

but do not want the staged changes in the next commit, unstage it with:

```bash
git restore --staged analysis.py
```

This removes the file's staged changes from the index; it does **not** delete the changes from the working directory.

```text
Before:
Working Directory: modified
Staging Area:      modified

After git restore --staged:
Working Directory: modified
Staging Area:      matches HEAD (for this file)
```

Inspect the result:

```bash
git status
git diff
```

This command is useful when you staged the wrong file or want to revise which changes belong in the next commit.

## 3. Undo the Latest Local Commit

If the most recent commit is local and you want to move the current branch back one commit while keeping the changes staged, use:

```bash
git reset --soft HEAD~1
```

`HEAD~1` means the first parent of the current commit—normally the previous commit on a simple linear history.

Example:

```text
Before:
Commit A ---- Commit B (HEAD)

After git reset --soft HEAD~1:
Commit A (HEAD)

The changes recorded in Commit B remain staged.
```

This is useful when you committed too early and want to amend or recreate the commit. It moves the branch pointer; it does not create a new commit that reverses the old one.

Be especially careful if the commit has already been pushed or shared. Rewriting shared history can disrupt collaborators.

## 4. Understand `git reset`

`git reset` moves the current branch to a target commit and, depending on the option, updates the staging area and working directory.

### `--soft`: keep changes staged

```bash
git reset --soft HEAD~1
```

Moves the current branch to the target commit while leaving the index and working tree unchanged. Changes from the commit you moved back from remain staged.

### Mixed (default): keep changes, unstage them

```bash
git reset HEAD~1
```

This is equivalent to:

```bash
git reset --mixed HEAD~1
```

Moves the current branch and resets the index to the target commit, while leaving the working tree unchanged. Changes from the removed commit therefore remain in the working directory as unstaged changes, assuming no other complications.

### `--hard`: reset index and working directory

```bash
git reset --hard HEAD~1
```

Moves the current branch and resets the index and working directory to match the target commit. Tracked changes that differ from the target may be discarded, including local edits. This can destroy work that has not been saved elsewhere.

**Do not use `--hard` as a routine cleanup command.** First inspect `git status`, review your work, and make a backup if there is any chance you will need it.

`git reset` has other uses and options, but these are the common forms for understanding how commits, staging, and working-tree changes differ.

## 5. Use `git revert`

`git revert` reverses the changes introduced by a commit by creating a **new commit**. It does not simply move the branch pointer backward.

```bash
git revert <commit-id>
```

Replace `<commit-id>` with the actual commit hash or another valid commit reference; do not type the angle brackets literally.

Conceptually:

```text
Commit A -> Commit B -> Commit C -> Revert C
                                  (new commit)
```

The new revert commit applies the inverse of the selected commit's changes. If Git cannot apply that inverse cleanly, it may report conflicts that you must resolve before completing the revert.

This is generally a better choice for undoing a commit that has already been pushed to a shared branch because it preserves the existing commit history and records the reversal as a new commit.

For merge commits, reverting requires additional care because you must choose a mainline parent. See the official `git revert` documentation before attempting that operation.

## Reset vs. Revert

| Command | What it does | Typical use |
|---|---|---|
| `git reset --soft` | Moves the branch; keeps changes staged. | Rework a recent local commit. |
| `git reset` / `--mixed` | Moves the branch; unstages changes but keeps the working-tree content. | Redo a local commit while retaining the edits. |
| `git reset --hard` | Moves the branch and resets the index and working tree. | Discard local work only when you are certain it is safe. |
| `git revert` | Creates a new commit that reverses an earlier commit. | Undo a change while preserving shared history. |

## What If the Commit Was Pushed?

If the commit is already on a shared remote branch, prefer `git revert` in most collaborative situations:

```bash
git revert <commit-id>
git push
```

Check the result and follow your team's review process. The exact push command depends on the current branch and its configured upstream.

Avoid resetting a shared branch and force-pushing rewritten history unless you understand the consequences and have coordinated with everyone affected. Force-pushing can overwrite remote history and disrupt collaborators.

If a secret was committed, reverting or resetting the commit is not enough to make the secret safe. Revoke or rotate the credential immediately and follow the appropriate incident-response steps.

## Safe Habits Before Undoing Changes

- Run `git status` to understand the current state.
- Use `git diff` and `git diff --staged` to inspect work before discarding or resetting it.
- If the changes might be useful later, make a backup or create a temporary branch before proceeding.
- Use `git restore` only when you intend to discard the relevant working-tree edits.
- Use `git reset` carefully because it moves the current branch and can remove changes from the index or working tree.
- Prefer `git revert` for undoing commits on shared branches.
- Do not assume that pushed secrets become safe after a commit is reversed.

Useful inspection commands:

```bash
git status
git diff
git diff --staged
git log --oneline
```

## Quick Reference

| Situation | Command | Effect |
|---|---|---|
| Discard unstaged edits to a tracked file | `git restore filename` | Restores the working-tree file from the index. |
| Unstage a file but keep its edits | `git restore --staged filename` | Updates the index; keeps working-tree changes. |
| Move back one commit and keep changes staged | `git reset --soft HEAD~1` | Moves the branch; leaves the index and working tree unchanged. |
| Move back one commit and keep changes unstaged | `git reset HEAD~1` | Moves the branch and resets the index; keeps working-tree edits. |
| Move back one commit and discard local tracked changes | `git reset --hard HEAD~1` | Resets branch, index, and working tree; potentially destructive. |
| Reverse an existing commit with a new commit | `git revert <commit-id>` | Records an inverse change in history. |
| Inspect state before undoing | `git status` | Shows staged, unstaged, and untracked changes. |
| Inspect staged changes | `git diff --staged` | Shows content staged for the next commit. |

## Practice Exercise

Use a **disposable test repository**. Do not practise destructive commands on your main project.

1. Create a file called `practice.txt`, stage it, and commit it.
2. Edit the file without staging it. Run `git status` and `git diff`.
3. Run `git restore practice.txt` and observe that the unstaged edit is discarded.
4. Edit the file again and stage it. Run `git restore --staged practice.txt`, then inspect `git status`. Confirm the edit remains in the working directory.
5. Commit the edit. Run `git log --oneline` to record the current history.
6. In this disposable repository, run `git reset --soft HEAD~1`. Observe that the latest commit disappears from the current branch history while its changes remain staged.
7. Commit the staged change again, then use `git revert HEAD` to create a new commit reversing it.

Before each step, ask yourself: **What will happen to the branch, index, working directory, and commit history?**

Do not run `git reset --hard` for this exercise unless the repository contains only disposable data and you understand exactly what it will discard.

## Next Steps

Continue with:

- [Git Workflow](../02_git_workflow/README.md)
- [Essential Git Commands](../03_essential_git_commands/README.md)
- [Branching](../06_branching/README.md)
- [Merging and Conflicts](../07_merging_and_conflicts/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git restore`](https://git-scm.com/docs/git-restore)
- [Git reference: `git reset`](https://git-scm.com/docs/git-reset)
- [Git reference: `git revert`](https://git-scm.com/docs/git-revert)
- [Git reference: `git status`](https://git-scm.com/docs/git-status)
- [Git reference: `git diff`](https://git-scm.com/docs/git-diff)
- [Git reference: `git log`](https://git-scm.com/docs/git-log)
