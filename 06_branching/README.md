# Git Branching

> Use branches to develop features, fix bugs, and experiment without making every change directly on the main line of development.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain what a Git branch represents.
- List, create, switch to, and delete local branches.
- Create a branch and make commits on it.
- Explain how branches can share earlier commits and then develop independently.
- Follow a safe feature-branch workflow and understand what switching branches does to your working directory.

## Contents

- [Why Use Branches?](#why-use-branches)
- [What Is a Branch?](#what-is-a-branch)
- [View Branches](#view-branches)
- [Create a Branch](#create-a-branch)
- [Switch Branches](#switch-branches)
- [Create and Switch in One Command](#create-and-switch-in-one-command)
- [A Practical Feature-Branch Workflow](#a-practical-feature-branch-workflow)
- [Branch Naming](#branch-naming)
- [Delete a Branch](#delete-a-branch)
- [Uncommitted Changes and Branch Switching](#uncommitted-changes-and-branch-switching)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## Why Use Branches?

A branch lets you develop a line of work separately from another branch. Branches are commonly used for:

- New features.
- Bug fixes.
- Experiments and prototypes.
- Isolated improvements to existing code or documentation.

Suppose the `main` branch contains a stable project, and you want to develop a new dashboard feature. You can create a feature branch, make commits there, and later merge the work into `main`.

```text
A---B---C  main
         \
          D---E  feature/dashboard
```

In this example, both branches share earlier history through commit `C`. The feature branch has moved forward with its own commits `D` and `E`, while `main` still points to `C`.

## What Is a Branch?

A Git branch is a **movable reference to a commit**, not a complete independent copy of the project.

When you create a branch, it initially points to the commit at the current `HEAD`. As you create commits while on that branch, the branch reference advances. Another branch can continue to point to an earlier commit.

```text
A---B---C  main
         \
          D---E  feature-analysis
```

Here, `main` and `feature-analysis` share commits `A`, `B`, and `C`, then the feature branch continues with `D` and `E`.

A branch is therefore a lightweight way to keep track of a line of development. The actual commits form the history.

## View Branches

List local branches:

```bash
git branch
```

The current branch is marked with an asterisk:

```text
* main
  feature/dashboard
```

To show the current branch name alone:

```bash
git branch --show-current
```

To list local and remote-tracking branches:

```bash
git branch -a
```

Remote-tracking branches are local references to branches observed on a remote; they are not the same as switching directly to a remote server.

## Create a Branch

Create a branch without switching to it:

```bash
git branch feature-dashboard
```

This creates the branch at the current commit. Your current branch and working directory do not switch automatically.

To start a branch from a particular existing branch, switch to that branch first, then create the new branch.

## Switch Branches

Switch to an existing local branch:

```bash
git switch feature-dashboard
```

Git updates the working directory and index to match the selected branch, subject to the state of your current changes.

If you have uncommitted changes, Git may carry them across if doing so is safe. If switching would overwrite conflicting changes, Git generally refuses and asks you to resolve the situation first. Do not assume that switching branches automatically saves or commits your work.

## Create and Switch in One Command

This is a common way to start new work:

```bash
git switch -c feature/dashboard
```

It creates the branch and switches to it in one step. The new branch starts at the current commit.

Use a descriptive name that communicates the task.

## A Practical Feature-Branch Workflow

The following example assumes your local `main` branch is the starting point and your working tree is in a suitable state.

```bash
# Check the current state
git status

# Switch to the starting branch
git switch main

# Create and switch to a feature branch
git switch -c feature/customer-analysis

# Make changes to your files, then inspect them
git status
git diff

# Stage and commit the feature work
git add analysis.py
git diff --staged
git commit -m "Add customer analysis"

# Return to main when ready
git switch main
```

At this point, the feature commit remains on `feature/customer-analysis`; switching to `main` does not automatically merge it into `main`.

When the work is ready, you can integrate it using a merge workflow:

```bash
git merge feature/customer-analysis
```

Run this command while on the branch that should receive the feature—in this example, switch to `main` first. Learn more in [Merging and Conflicts](../07_merging_and_conflicts/README.md).

If the branch needs to be shared with GitHub, publishing it is a separate step. For example, you may push the branch to a configured remote:

```bash
git push -u origin feature/customer-analysis
```

This assumes `origin` is the remote name and you have permission to push. The `-u` option sets the upstream tracking relationship for future pushes and pulls.

## Branch Naming

Choose names that make the purpose of the work clear. Common examples include:

```text
feature/dashboard
feature/customer-analysis
fix/missing-values
experiment/new-model
docs/update-readme
```

Naming conventions vary between teams. Consistent, descriptive names help people understand the intent of a branch when reviewing work.

## Delete a Branch

After a feature branch has been merged, you can usually delete the local branch:

```bash
git branch -d feature-dashboard
```

The `-d` option asks Git to delete the branch only when Git determines that its changes have been merged into the relevant upstream or `HEAD` history.

If Git warns that the branch contains unmerged work, do not immediately force deletion. Confirm that the work is no longer needed.

The force-delete form is:

```bash
git branch -D feature-dashboard
```

`-D` can delete a branch even if Git considers it unmerged. Use it only when you are certain the branch's work can be discarded or is safely preserved elsewhere.

Deleting a local branch does not automatically delete a branch on a remote repository.

## Uncommitted Changes and Branch Switching

Before switching branches, inspect your current state:

```bash
git status
git diff
git diff --staged
```

Keep these principles in mind:

- Committed changes are recorded in the current branch's history.
- Uncommitted changes are not automatically committed just because you switch branches.
- Git may allow compatible uncommitted changes to remain in your working directory across a switch.
- Git may block a switch if it would overwrite changes.
- If you need to set work aside temporarily, `git stash` may help; learn about it in [Git Stash](../13_git_stash/README.md).

Do not use a branch switch as a substitute for understanding the state of your working directory.

## Quick Reference

| Command | Purpose |
|---|---|
| `git branch` | List local branches. |
| `git branch --show-current` | Show the current branch name. |
| `git branch -a` | List local and remote-tracking branches. |
| `git branch <branch-name>` | Create a branch without switching to it. |
| `git switch <branch-name>` | Switch to an existing local branch. |
| `git switch -c <branch-name>` | Create and switch to a new branch. |
| `git branch -d <branch-name>` | Delete a branch Git considers merged. |
| `git branch -D <branch-name>` | Force-delete a local branch; potentially loses an easy reference to unmerged work. |
| `git status` | Inspect the current working state before switching. |
| `git merge <branch-name>` | Merge the named branch into the current branch. |
| `git push -u origin <branch-name>` | Push a branch and set its upstream, assuming the remote is configured. |

## Practice Exercise

Use a test repository or a safe practice folder.

1. Run `git status` and identify the current branch.
2. Create and switch to a branch named `feature/branch-practice` using `git switch -c feature/branch-practice`.
3. Create a file named `branch-practice.md`, add a short note, stage it, and commit it.
4. Run `git log --oneline --decorate --all` to inspect where the branch references point.
5. Switch back to `main` (or the default branch used by your test repository). Confirm that the feature commit is not part of this branch's current history unless it has been merged.
6. Switch back to `feature/branch-practice` and confirm the file is present.
7. If you want to practise merging, switch to the receiving branch and merge the feature branch. Then delete the local feature branch with `git branch -d feature/branch-practice`.

Answer these questions in your own words:

- What commit did the new branch initially point to?
- Why did the feature commit not automatically appear on `main`?
- What is the difference between `git branch feature-name` and `git switch -c feature-name`?
- Why should you inspect uncommitted changes before switching branches?

## Next Steps

Continue with:

- [Merging and Conflicts](../07_merging_and_conflicts/README.md)
- [Git Stash](../13_git_stash/README.md)
- [Git Rebase](../14_git_rebase/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git branch`](https://git-scm.com/docs/git-branch)
- [Git reference: `git switch`](https://git-scm.com/docs/git-switch)
- [Git reference: `git merge`](https://git-scm.com/docs/git-merge)
- [Git reference: `git stash`](https://git-scm.com/docs/git-stash)
- [Git reference: `git push`](https://git-scm.com/docs/git-push)
