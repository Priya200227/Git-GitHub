# Remote Repositories

> Learn how local Git repositories connect to remote repositories, and how to fetch, pull, push, and clone changes safely.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain the difference between a local repository and a remote repository.
- Inspect, add, and understand configured remotes.
- Push local commits and set an upstream branch.
- Distinguish `git fetch` from `git pull`.
- Clone an existing repository and follow a safe remote workflow.

## Contents

- [Local vs. Remote](#local-vs-remote)
- [Check Remotes](#check-remotes)
- [Add a Remote](#add-a-remote)
- [Push](#push)
- [Set an Upstream Branch](#set-an-upstream-branch)
- [Fetch](#fetch)
- [Pull](#pull)
- [Clone](#clone)
- [Push vs. Fetch vs. Pull vs. Clone](#push-vs-fetch-vs-pull-vs-clone)
- [Common Workflow](#common-workflow)
- [Common Mistakes and Safe Habits](#common-mistakes-and-safe-habits)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## Local vs. Remote

A **remote repository** is another repository copy, often hosted on a platform such as GitHub. Local and remote repositories can exchange commits.

```text
Local Repository
      |
      | git push
      v
Remote Repository
      |
      | git fetch / git pull
      v
Local Repository and Branch
```

The local repository contains local Git history and work. A remote provides a separate location for sharing repository history with other contributors or machines.

A remote is a named reference to another repository's URL. The remote itself is not a live window into the server: Git stores local remote-tracking references to record the remote branch tips it has observed.

## Check Remotes

List configured remote names and their URLs:

```bash
git remote -v
```

Example output:

```text
origin  https://github.com/OWNER/PROJECT.git (fetch)
origin  https://github.com/OWNER/PROJECT.git (push)
```

`origin` is a common remote name. Replace the example owner and project with the actual repository details. A remote can have different fetch and push URLs.

If this command prints no remotes, the local repository may not yet be connected to a remote.

## Add a Remote

To connect an existing local repository to a remote repository:

```bash
git remote add origin <repository-url>
```

Replace `<repository-url>` with the URL copied from your hosting provider. Do not type the angle brackets literally.

Check that it was added:

```bash
git remote -v
```

If `origin` already exists, Git will report that the remote name is already in use. Inspect the current configuration before changing it. Do not overwrite a remote URL unless you know which repository it should point to.

## Push

Push local commits from the `main` branch to the remote named `origin`:

```bash
git push origin main
```

This sends commits from the local `main` branch to the remote branch `main`, subject to the repository's configuration and your permissions.

A push publishes commits, not every uncommitted edit or untracked file in the working directory. Stage and commit the changes you intend to share first.

## Set an Upstream Branch

When pushing a branch for the first time, you can set its upstream tracking relationship:

```bash
git push -u origin main
```

The `-u` option (short for `--set-upstream`) configures the local branch to track the corresponding remote branch when the push succeeds.

After the upstream is configured, commands can often be shortened to:

```bash
git push
git pull
```

This depends on your repository's configuration and current branch. The examples assume the local branch is named `main` and the remote is named `origin`.

## Fetch

Download remote updates without automatically integrating them into your current branch:

```bash
git fetch
```

If the repository has multiple remotes and you want to fetch from a specific one:

```bash
git fetch origin
```

Fetch updates local remote-tracking references, such as `origin/main`, and downloads the necessary Git objects. It does not automatically merge those updates into your current local branch.

You can inspect the current state and compare the fetched branch with your local branch:

```bash
git status
git log --oneline --graph --decorate --all
git diff main..origin/main
```

The last command compares the two branch tips for differences; use the relevant local and remote-tracking branch names for your repository.

## Pull

Fetch remote updates and integrate them into the current branch:

```bash
git pull
```

To specify a remote and branch explicitly:

```bash
git pull origin main
```

This integrates `origin/main` into the branch currently checked out. It does **not** automatically switch you to your local `main` branch.

Conceptually, `git pull` performs two stages:

```text
Fetch remote updates
        |
        v
Integrate into the current branch
```

The integration strategy depends on configuration and options. It may use a fast-forward, a merge, or a rebase. If the histories conflict, you may need to resolve conflicts before the operation can finish.

Before pulling, inspect your current branch and working-tree state:

```bash
git status
git branch --show-current
```

## Clone

To create a local copy of an existing remote repository:

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/OWNER/PROJECT.git
cd PROJECT
```

Cloning downloads the repository's history and working files, creates a local repository, and configures a remote—usually named `origin`. The destination directory name is normally based on the repository name unless you specify another one.

Use `git clone` when you want to start from an existing remote repository. Use `git init` when starting a new repository in a local directory.

## Push vs. Fetch vs. Pull vs. Clone

| Command | Purpose |
|---|---|
| `git push` | Sends local commits to a remote repository. |
| `git fetch` | Downloads remote updates without integrating them into the current branch. |
| `git pull` | Fetches remote updates and integrates them into the current branch. |
| `git clone` | Creates a local copy of an existing repository. |

The key distinction is that **fetch downloads updates, while pull also integrates them**.

## Common Workflow

A typical workflow for an already configured repository is:

```bash
# Check the current state and branch
git status
git branch --show-current

# Fetch remote updates
git fetch origin

# Inspect branches and recent history
git log --oneline --graph --decorate --all

# Integrate updates into the current branch when appropriate
git pull

# Make changes, then inspect them
git status
git diff

# Stage and review intended changes
git add analysis.py
git diff --staged

# Commit the changes
git commit -m "Update analysis"

# Publish the commit
git push
```

You do not need to run both `git fetch` and `git pull` every time: `git pull` itself fetches before integrating. Running `fetch` separately is useful when you want to inspect remote updates before deciding whether to integrate them.

## Common Mistakes and Safe Habits

- **Assuming `git fetch` changes your current branch:** It updates remote-tracking information but does not automatically integrate the changes.
- **Assuming `git pull` switches branches:** It integrates remote changes into the branch you currently have checked out.
- **Pushing uncommitted work:** Commit the changes first; a normal push transfers commits.
- **Using the wrong remote or branch name:** Check with `git remote -v` and `git branch --show-current`.
- **Pulling without checking local changes:** Review `git status` first, especially in a shared project.
- **Assuming `origin` is mandatory:** It is a conventional name, not a required keyword.
- **Overwriting a remote URL without checking:** Confirm which repository the remote should reference before changing configuration.

## Quick Reference

| Goal | Command |
|---|---|
| List remotes and URLs | `git remote -v` |
| Add a remote | `git remote add origin <repository-url>` |
| Push `main` to `origin` | `git push origin main` |
| Push and set upstream | `git push -u origin main` |
| Fetch from `origin` | `git fetch origin` |
| Pull `main` from `origin` into the current branch | `git pull origin main` |
| Clone a remote repository | `git clone <repository-url>` |
| Check repository state | `git status` |
| Show current branch | `git branch --show-current` |
| View all branches and recent history | `git log --oneline --graph --decorate --all` |

## Practice Exercise

Use a test repository or a project you are comfortable experimenting with.

1. Clone a repository you have permission to access, or use an existing local repository with a configured remote.
2. Run `git remote -v` and identify the remote name and URL.
3. Run `git fetch origin`.
4. Inspect the branch graph with `git log --oneline --graph --decorate --all`.
5. Explain what changed locally after the fetch and whether your current branch was automatically integrated.
6. Make a small, harmless edit, stage it, and commit it.
7. Push the commit if you have permission and the repository is appropriate for the exercise.
8. Check the remote repository to confirm the commit was published.

Answer these questions:

- What is a remote-tracking branch such as `origin/main`?
- How does `git fetch` differ from `git pull`?
- What does `git push -u origin main` configure?
- Why should you check the current branch before running `git pull origin main`?

## Next Steps

Continue with:

- [GitHub Basics](../08_github_basics/README.md)
- [Collaboration](../10_collaboration/README.md)
- [Pull Requests](../11_pull_requests/README.md)
- [Forking](../12_forking/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git remote`](https://git-scm.com/docs/git-remote)
- [Git reference: `git push`](https://git-scm.com/docs/git-push)
- [Git reference: `git fetch`](https://git-scm.com/docs/git-fetch)
- [Git reference: `git pull`](https://git-scm.com/docs/git-pull)
- [Git reference: `git clone`](https://git-scm.com/docs/git-clone)
- [Git reference: `git status`](https://git-scm.com/docs/git-status)
