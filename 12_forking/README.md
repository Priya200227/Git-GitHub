# Forking

> Learn how GitHub forks, local clones, and Git remotes work together in a typical open-source contribution workflow.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this section, you should be able to:

- Explain what a fork is and when it is useful.
- Distinguish a fork from a clone.
- Clone your fork and work on a feature branch.
- Understand the conventional roles of `origin` and `upstream`.
- Fetch changes from the original repository and choose an appropriate way to integrate them.

## Contents

- [What Is a Fork?](#what-is-a-fork)
- [Fork Workflow](#fork-workflow)
- [Typical Open-Source Contribution Workflow](#typical-open-source-contribution-workflow)
- [Fork vs. Clone](#fork-vs-clone)
- [Clone Your Fork](#clone-your-fork)
- [Push Changes to Your Fork](#push-changes-to-your-fork)
- [Understanding `origin` and `upstream`](#understanding-origin-and-upstream)
- [Keeping Your Fork Updated](#keeping-your-fork-updated)
- [Why Forking Is Useful](#why-forking-is-useful)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

---

## What Is a Fork?

A **fork** is a copy of a repository hosted under a different GitHub account or namespace. Forking is commonly used when you want to contribute to a repository but do not have direct write access to the original repository.

Your fork is a separate hosted repository where you can publish your own branches and commits. It remains connected conceptually to the original project, but changes in either repository do not automatically appear in the other.

## Fork Workflow

```text
Original GitHub Repository
           |
          Fork
           |
           v
    My GitHub Fork
           |
         Clone
           |
           v
    My Local Repository
```

After forking, clone your fork to your computer. You can create branches, make changes, commit them locally, and push those branches back to your fork.

## Typical Open-Source Contribution Workflow

```text
Fork the original repository
             |
             v
Clone your fork
             |
             v
Create a feature branch
             |
             v
Make and test changes
             |
             v
Commit the changes
             |
             v
Push the branch to your fork
             |
             v
Open a pull request
             |
             v
Original repository
```

The pull request proposes integrating your changes into the original repository. The project maintainers review the proposal and decide whether and how to merge it.

## Fork vs. Clone

These are related but different concepts.

### Fork

Creates a hosted copy of a repository under another GitHub account or namespace.

```text
Original GitHub Repository
           |
          Fork
           |
           v
My GitHub Repository
```

### Clone

Creates a local repository from a remote repository, including its Git history and working files.

```text
Remote Repository
        |
       Clone
        |
        v
Local Repository
```

A simple distinction:

- **Fork:** creates a separate hosted repository on GitHub.
- **Clone:** copies a repository to your local computer.

They are often used together when contributing to open-source projects, but neither operation requires the other in every workflow.

## Clone Your Fork

After creating a fork on GitHub, copy its clone URL and run:

```bash
git clone <my-fork-url>
cd <repository-directory>
```

Replace `<my-fork-url>` with the actual URL of your fork and `<repository-directory>` with the directory created by the clone operation.

Create a feature branch before making changes:

```bash
git switch -c feature/my-change
```

This lets you work on a separate branch rather than making the change directly on your local default branch.

## Push Changes to Your Fork

After editing files, inspect your changes and stage only the intended files:

```bash
git status
git diff

git add path/to/file
git diff --staged
git commit -m "Add requested change"
```

`path/to/file` is a placeholder. Replace it with the actual path of a file you changed. If the change spans multiple files, stage the intended paths.

Push the feature branch to your fork:

```bash
git push -u origin feature/my-change
```

When you clone your fork, Git normally names that remote `origin`. The `-u` option sets the upstream tracking relationship for the branch, making subsequent pushes simpler.

After pushing, open a pull request on GitHub from your fork's feature branch to the appropriate branch in the original repository. Verify the source and target repositories and branches before submitting the PR.

## Understanding `origin` and `upstream`

When you clone your fork, Git normally configures `origin` to point to that fork:

```text
origin → My GitHub fork
```

To fetch changes from the original repository, add it as a second remote. The name `upstream` is a common convention:

```bash
git remote add upstream <original-repository-url>
git remote -v
```

Conceptually:

```text
upstream → Original repository
origin   → My GitHub fork
local    → Repository on your computer
```

`origin` and `upstream` are just remote names. Git does not require these specific names, and `local` is not a remote in this diagram—it represents your local repository.

## Keeping Your Fork Updated

The original repository may receive new commits after you create your fork. To update your local knowledge of the original repository, fetch from `upstream`:

```bash
git fetch upstream
```

Fetching downloads new objects and updates remote-tracking references, such as `upstream/main`, without automatically integrating those changes into your current local branch.

Inspect the available branches and history before deciding how to update your local branch:

```bash
git branch -a
git log --oneline --graph --decorate --all
```

If the original repository's default branch is `main`, and your project workflow calls for updating your local `main` from it, one possible approach is:

```bash
git switch main
git merge upstream/main
```

This example assumes that `upstream/main` exists and that merging it into your local `main` is appropriate for your workflow. The source branch may have a different name, and a team may prefer another integration method. If your local branch has diverged, review the history and project guidance before integrating changes.

If you also want your GitHub fork's default branch to reflect the updated local branch, push the updated branch to `origin`:

```bash
git push origin main
```

Only do this when `main` is the intended branch and you are authorized to update it. Follow the project's contribution guidance, especially if you have local commits or changes that must be preserved.

## Why Forking Is Useful

Forking is especially useful for:

- Contributing to open-source projects.
- Proposing changes to public GitHub repositories.
- Working on a project where you do not have direct write access.
- Experimenting independently without changing the original repository.

A fork gives you a hosted copy where you can publish your work while still being able to fetch updates from the original project.

## Common Mistakes

| Mistake | Why it matters | Better practice |
|---|---|---|
| Confusing a fork with a clone | One creates a hosted copy; the other creates a local repository | Learn the role of each operation |
| Pushing to the wrong remote | Changes may be published to an unintended repository | Check `git remote -v` before pushing |
| Opening a PR against the wrong repository or branch | The proposal may target the wrong project or branch | Verify both the base repository and target branch |
| Assuming a fork updates automatically | The original repository may advance independently | Fetch from `upstream` and integrate changes deliberately |
| Running `git add .` without inspection | Unrelated or sensitive files may be staged | Inspect status and stage intended paths |
| Assuming `upstream` is a special Git keyword | It is only a conventional remote name | Use `git remote -v` to see actual configuration |
| Merging upstream changes without checking local work | Conflicts or unexpected history changes may result | Inspect branch history and follow project guidance |

## Quick Reference

| Goal | Command |
|---|---|
| Clone your fork | `git clone <my-fork-url>` |
| Create a feature branch | `git switch -c feature/my-change` |
| Check working-tree status | `git status` |
| Inspect unstaged changes | `git diff` |
| Stage a specific file | `git add path/to/file` |
| Inspect staged changes | `git diff --staged` |
| Commit changes | `git commit -m "Add requested change"` |
| Push a feature branch | `git push -u origin feature/my-change` |
| Add the original repository | `git remote add upstream <original-repository-url>` |
| Inspect configured remotes | `git remote -v` |
| Fetch original-repository updates | `git fetch upstream` |
| List local and remote-tracking branches | `git branch -a` |
| Inspect all branch history | `git log --oneline --graph --decorate --all` |

## Practice Exercise

Use a repository where you are allowed to contribute, or create a test repository for practice.

1. Fork a public repository on GitHub, if its license and contribution guidance permit your intended use.
2. Clone your fork to your computer.
3. Create a branch named `docs/forking-practice`.
4. Make a small, appropriate documentation change.
5. Inspect the changes with `git status` and `git diff`.
6. Stage only the intended file(s), inspect them with `git diff --staged`, and commit.
7. Push the branch to your fork.
8. Open a pull request to the original repository if contributions are welcome and the change is appropriate.
9. Add the original repository as `upstream` if you want to practise fetching updates, then run `git fetch upstream`.
10. Inspect `git branch -a` and identify your local branches, your fork's remote-tracking branches, and the original repository's remote-tracking branches.

You do not need to submit a pull request to practise the fork, clone, branch, commit, push, and remote commands. Follow the target project's contribution rules before proposing changes.

## Next Steps

Continue to [Git Stash](../13_git_stash/README.md) to learn how to temporarily set aside local changes while keeping your working tree available for another task.

Related topics:

- [Pull Requests](../11_pull_requests/README.md)
- [Collaboration](../10_collaboration/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [GitHub Docs: Fork a repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
- [GitHub Docs: About forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks)
- [GitHub Docs: Configuring a remote repository for a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork)
- [Git documentation: `git clone`](https://git-scm.com/docs/git-clone)
- [Git documentation: `git remote`](https://git-scm.com/docs/git-remote)
- [Git documentation: `git fetch`](https://git-scm.com/docs/git-fetch)
