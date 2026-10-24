# Merging and Merge Conflicts

> Learn how to integrate work from one branch into another, understand fast-forward merges, and resolve conflicts when Git cannot combine changes automatically.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain which branch receives changes during a merge.
- Merge a feature branch into a target branch.
- Recognize a fast-forward merge.
- Identify conflict markers and resolve a simple merge conflict.
- Complete or abort a merge safely.

## Contents

- [What Does `git merge` Do?](#what-does-git-merge-do)
- [Basic Merge](#basic-merge)
- [Typical Feature-Branch Workflow](#typical-feature-branch-workflow)
- [Fast-Forward Merge](#fast-forward-merge)
- [What Is a Merge Conflict?](#what-is-a-merge-conflict)
- [Resolve a Merge Conflict](#resolve-a-merge-conflict)
- [Conflict Resolution Workflow](#conflict-resolution-workflow)
- [Abort an In-Progress Merge](#abort-an-in-progress-merge)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## What Does `git merge` Do?

`git merge` integrates changes from another branch into the **currently checked-out branch**. Before merging, confirm which branch you are on:

```bash
git status
git branch --show-current
```

The branch you are currently on is the receiving branch. The branch named in the merge command is the branch whose changes you want to integrate.

## Basic Merge

Suppose `main` and `feature-dashboard` share an earlier commit, and the feature branch has additional work:

```text
      C---D  feature-dashboard
     /
A---B  main
```

To merge the feature branch into `main`:

```bash
git switch main
git merge feature-dashboard
```

This integrates the feature branch into `main`. Depending on the commit graph and merge strategy, Git may fast-forward `main` or create a merge commit. If changes cannot be combined automatically, Git may report conflicts.

**Remember:** `git merge feature-dashboard` affects the branch you currently have checked out. It does not automatically switch to `main`.

## Typical Feature-Branch Workflow

A common workflow looks like this:

```bash
# Start from the target branch
git switch main

# Create and switch to a feature branch
git switch -c feature-dashboard

# Make changes, then inspect them
git status
git diff

# Stage and commit the feature
git add .
git diff --staged
git commit -m "Add dashboard"

# Return to the target branch
git switch main

# Integrate the feature
git merge feature-dashboard
```

Use `git add .` only when you have checked that the changes under the current directory are the ones you intend to stage. For more focused commits, stage specific files instead.

After a successful merge, the changes from the feature branch are integrated into `main`. You can usually delete the local feature branch when it is no longer needed:

```bash
git branch -d feature-dashboard
```

## Fast-Forward Merge

A **fast-forward merge** is possible when the current branch is an ancestor of the branch being merged and no divergent history needs to be joined.

For example, if `main` has not advanced since the feature branch was created:

```text
A---B---C---D  feature-dashboard
     ^
    main
```

When you run:

```bash
git switch main
git merge feature-dashboard
```

Git can simply move the `main` branch reference forward to commit `D`. No new merge commit is needed for a fast-forward merge.

If both branches have new commits that are not part of the other's history, Git generally needs to combine the divergent histories instead of simply moving the branch reference.

## What Is a Merge Conflict?

A merge conflict occurs when Git cannot automatically reconcile changes while merging. A common example is when both branches modify overlapping lines of the same file in incompatible ways.

Git marks the conflicted section in the file. A simplified example is:

```text
<<<<<<< HEAD
Content from the current branch
=======
Content from the branch being merged
>>>>>>> feature-dashboard
```

These lines are **conflict markers**:

- `<<<<<<< HEAD` starts the current branch's version of the conflicted section.
- `=======` separates the two versions.
- `>>>>>>> feature-dashboard` marks the end of the incoming branch's version; the label depends on the merge.

The marker labels can vary by situation. The text shown is an illustration, not necessarily the exact content of every conflict.

A conflict is not necessarily a Git error. It means Git needs you to decide what the correct result should be.

## Resolve a Merge Conflict

First, inspect the repository state:

```bash
git status
```

Git identifies files that need conflict resolution. Then:

1. Open each conflicted file.
2. Read both versions and understand the intended result.
3. Choose one version or combine them into the correct content.
4. Remove all conflict markers from the resolved section.
5. Save the file.
6. Review the result and stage the resolved file.

For example:

```bash
git diff
git add analysis.py
git status
```

After staging the resolved files, complete the merge:

```bash
git commit
```

For a normal merge conflict, Git prepares a merge commit message. Review it and save/close the configured editor to finish the commit. If your Git setup completes the merge automatically after all conflicts are resolved and staged, follow the status message Git provides.

Do not stage a conflicted file until you have reviewed its final contents. Staging tells Git that you consider the conflict resolved; it does not validate whether your chosen code or text is correct.

## Conflict Resolution Workflow

```text
Start merge
    |
    v
Conflict detected
    |
    v
Run git status
    |
    v
Open conflicted files
    |
    v
Choose or combine the intended changes
    |
    v
Remove conflict markers and save
    |
    v
Review the result
    |
    v
git add <resolved-file>
    |
    v
git commit
```

After completing the merge, check the repository state and history:

```bash
git status
git log --oneline --graph --decorate -10
```

## Abort an In-Progress Merge

If you want to cancel an in-progress merge, you can try:

```bash
git merge --abort
```

This attempts to return the repository to the state it was in before the merge began. It is most reliable when you had no uncommitted changes before starting the merge.

If you began the merge with uncommitted work, Git may not always be able to reconstruct those original changes perfectly. For that reason, check `git status` before starting a merge and commit or safely preserve important work first.

## Common Mistakes

- **Merging from the wrong branch:** The currently checked-out branch receives the merge. Confirm it before running `git merge`.
- **Assuming every merge creates a merge commit:** A fast-forward merge may only move the branch reference.
- **Deleting conflict markers without understanding the code:** Resolve the intended content, not just the visible markers.
- **Staging before reviewing:** `git add` marks the conflict as resolved from Git's perspective, but it does not check correctness.
- **Assuming `git merge --abort` is always lossless:** Protect existing uncommitted work before starting.
- **Using `git add .` without checking:** You may stage unrelated changes along with the feature.

## Quick Reference

| Goal | Command |
|---|---|
| Check current branch and repository state | `git status` |
| Show current branch name | `git branch --show-current` |
| Merge a branch into the current branch | `git merge <branch-name>` |
| Inspect differences | `git diff` |
| Stage a resolved file | `git add <resolved-file>` |
| Complete a merge | `git commit` |
| Abort an in-progress merge | `git merge --abort` |
| View graph of recent history | `git log --oneline --graph --decorate -10` |
| Delete a merged local branch | `git branch -d <branch-name>` |

## Practice Exercise

Use a **disposable test repository**.

1. Create a repository and make an initial commit containing `notes.txt`.
2. Create a branch called `feature/notes` and change one line in `notes.txt`. Commit the change.
3. Switch back to `main` and change the same line differently. Commit that change.
4. Run `git merge feature/notes`. Git should report a conflict if the edits overlap incompatibly.
5. Run `git status` and open `notes.txt`.
6. Decide on the final line, remove the conflict markers, and save the file.
7. Stage the resolved file with `git add notes.txt`, then complete the merge with `git commit`.
8. Inspect the graph using `git log --oneline --graph --decorate --all`.

Answer these questions:

- Which branch received the merge?
- Why couldn't Git automatically combine the two edits?
- What does staging a conflicted file tell Git?
- How does a fast-forward merge differ from a merge that creates a merge commit?
- What should you do before starting a merge if you have important uncommitted changes?

## Next Steps

Continue with:

- [Branching](../06_branching/README.md)
- [GitHub Basics](../08_github_basics/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)
- [Git Rebase](../14_git_rebase/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git merge`](https://git-scm.com/docs/git-merge)
- [Git reference: `git status`](https://git-scm.com/docs/git-status)
- [Git reference: `git diff`](https://git-scm.com/docs/git-diff)
- [Git reference: `git add`](https://git-scm.com/docs/git-add)
- [Git reference: `git commit`](https://git-scm.com/docs/git-commit)
- [Git reference: `git log`](https://git-scm.com/docs/git-log)
