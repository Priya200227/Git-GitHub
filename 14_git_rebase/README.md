# Git Rebase

> Learn how Git rebase replays commits onto a new base, how it differs from merging, and how to handle conflicts safely.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this section, you should be able to:

- Explain what rebase does to a branch's commits.
- Distinguish rebase from merge.
- Rebase a feature branch onto another branch.
- Resolve conflicts and continue or abort a rebase.
- Understand why rebasing shared commits requires care.

## Contents

- [What Is Rebase?](#what-is-rebase)
- [Example: Replay Commits onto a New Base](#example-replay-commits-onto-a-new-base)
- [Basic Rebase Workflow](#basic-rebase-workflow)
- [Rebase vs. Merge](#rebase-vs-merge)
- [Handling Rebase Conflicts](#handling-rebase-conflicts)
- [Abort an In-Progress Rebase](#abort-an-in-progress-rebase)
- [Rebase and Shared History](#rebase-and-shared-history)
- [When Rebase Can Be Useful](#when-rebase-can-be-useful)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

---

## What Is Rebase?

`git rebase` is a Git operation that replays commits onto a different base commit. It can help create a more linear project history by moving a branch's work to a newer base without creating a merge commit for that update.

Rebase does not simply move the original commit objects. Git creates new commits for the replayed changes, so the recreated commits have new identities.

## Example: Replay Commits onto a New Base

Suppose the history looks like this:

```text
A---B---C  main
     \
      D---E  feature
```

While the feature was being developed, `main` moved forward to `C`. Rebase can replay the feature commits on top of `C`:

```text
A---B---C---D'---E'  feature
```

`D'` and `E'` represent recreated commits. They may contain the same logical changes as `D` and `E`, but their commit identities differ because their parent history has changed.

The diagram is simplified: the exact commits replayed depend on the branch histories and the rebase operation.

## Basic Rebase Workflow

To replay the commits on `feature` onto `main`:

```bash
git switch feature
git rebase main
```

This tells Git to replay the commits that belong to `feature` but are not part of the selected base history onto `main`.

Before rebasing, make sure you are on the branch you intend to rebase and that your working tree is in a suitable state. Check with:

```bash
git status
git log --oneline --graph --decorate --all
```

If you want the feature branch to include the latest remote `main`, first update your local view of the remote according to your project's workflow. For example:

```bash
git fetch origin
```

Then rebase onto the appropriate remote-tracking branch if that is what your team uses, such as `origin/main`:

```bash
git switch feature
git rebase origin/main
```

Do not assume that `main` is automatically up to date with the remote; `git rebase main` uses your local `main` reference.

## Rebase vs. Merge

### Merge

A merge integrates two lines of development. When a merge commit is needed, the history may look like this:

```text
A---B---C------F  main
     \        /
      D---E---
```

The merge commit `F` connects the histories. Git can also perform a fast-forward merge when the target branch can move forward without creating a merge commit, so not every merge produces a commit like `F`.

### Rebase

A rebase replays commits on a new base:

```text
A---B---C---D'---E'  feature
```

This can create a more linear history, but it rewrites the identities of the replayed commits.

| Operation | What it does | Typical history effect |
|---|---|---|
| `git merge` | Integrates the histories of branches | May create a merge commit; can also fast-forward |
| `git rebase` | Replays commits onto a new base | Creates new commit identities for replayed commits |

Neither operation is universally better. The right choice depends on the project's workflow, desired history, and whether the commits have already been shared.

## Handling Rebase Conflicts

A rebase can stop when Git cannot apply a commit automatically, for example when changes overlap with changes in the new base.

### 1. Inspect the state

```bash
git status
```

Git reports which files need attention. Open each conflicted file and resolve the conflict markers, preserving the intended result.

### 2. Stage the resolved files

After resolving a file:

```bash
git add path/to/file
```

Replace `path/to/file` with the actual path. Stage all files that you have resolved for the current step.

### 3. Continue the rebase

```bash
git rebase --continue
```

Git may stop again if another commit has a conflict. Repeat the inspect, resolve, stage, and continue process until the rebase completes.

Do not create a regular commit just to move past a rebase conflict unless Git or your workflow explicitly calls for it. During a normal rebase conflict, `git rebase --continue` is the command used to continue replaying commits after staging the resolution.

## Abort an In-Progress Rebase

If you decide not to continue, you can usually return the branch to its pre-rebase state with:

```bash
git rebase --abort
```

Use this while a rebase is in progress. Inspect `git status` afterward to confirm the repository state.

## Rebase and Shared History

**Rebase rewrites commit history.** Because replayed commits receive new identities, rebasing commits that other people have already based work on can disrupt collaboration.

A useful rule:

> Rebase local, unpublished work when appropriate; coordinate carefully before rewriting commits that have already been shared.

Rebasing a feature branch that exists only on your computer is generally easier to manage than rewriting a shared branch. If you have already pushed a branch and then rebase it, pushing may be rejected because the remote history no longer matches. Do not force-push blindly; follow team policy and understand who else may depend on that history.

## When Rebase Can Be Useful

Rebase can be useful when:

- Updating a feature branch with changes from a newer base branch.
- Keeping a more linear project history.
- Organizing local commits before sharing them.
- Incorporating base-branch changes without adding a merge commit for that integration.

Rebase does not automatically make code correct or resolve every conflict. Always inspect the result and run the relevant tests or checks.

## Common Mistakes

| Mistake | Why it matters | Better practice |
|---|---|---|
| Rebasing the wrong branch | Commits may be replayed onto an unintended base | Check `git status` and confirm the current branch |
| Assuming local `main` is current | The local branch may not contain the latest remote commits | Fetch and use the intended base reference |
| Treating rebase as a simple move | Replayed commits get new identities | Understand that rebase recreates commits |
| Ignoring conflicts | The rebase cannot safely finish until conflicts are resolved or the operation is aborted | Inspect `git status`, resolve files, stage, and continue |
| Rebasing shared commits without coordination | Other contributors may depend on the original history | Follow team policy and communicate before rewriting shared history |
| Force-pushing after a rebase without checking | Shared remote history may be overwritten or disrupted | Understand the divergence and follow the repository's rules |

## Quick Reference

| Goal | Command |
|---|---|
| Rebase current branch onto `main` | `git rebase main` |
| Rebase `feature` onto local `main` | `git switch feature` then `git rebase main` |
| Fetch remote updates | `git fetch origin` |
| Rebase onto remote-tracking `main` | `git rebase origin/main` |
| Inspect repository state | `git status` |
| Inspect branch history | `git log --oneline --graph --decorate --all` |
| Stage a resolved file | `git add path/to/file` |
| Continue after resolving conflicts | `git rebase --continue` |
| Cancel an in-progress rebase | `git rebase --abort` |

## Practice Exercise

Use a test repository so you can experiment without risking important work.

1. Create a base branch and make a few commits.
2. Create a feature branch and add one or two commits.
3. Switch back to the base branch and add another commit.
4. Switch to the feature branch and run `git rebase main`.
5. Inspect the new history with `git log --oneline --graph --decorate --all`.
6. If you want to practise conflict handling, make overlapping edits to the same lines on the base and feature branches, then try rebasing again in a fresh test scenario.
7. When a conflict occurs, inspect `git status`, resolve the file, stage it, and run `git rebase --continue`.
8. In a separate test, start a rebase and use `git rebase --abort` to practise cancelling it.

Use the actual base branch name if it is not `main`. Do not perform conflict experiments in a repository containing important uncommitted work.

## Next Steps

This completes the numbered folder sequence in this learning repository. Review the main [learning roadmap](../README.md) and the [Git cheat sheet](../CheatSheet.md) to reinforce the commands and concepts across the repository.

Related topics:

- [Branching](../06_branching/README.md)
- [Merging and Conflicts](../07_merging_and_conflicts/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)
- [Git Stash](../13_git_stash/README.md)

## Official References

- [Git documentation: `git rebase`](https://git-scm.com/docs/git-rebase)
- [Git documentation: `git merge`](https://git-scm.com/docs/git-merge)
- [Pro Git book: Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
- [Git documentation: `git fetch`](https://git-scm.com/docs/git-fetch)
