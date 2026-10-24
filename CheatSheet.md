# Git & GitHub Cheat Sheet

> A practical quick reference for common Git commands and the GitHub collaboration workflow. Use the command details and safety notes below when working in a repository.

## Contents

- [Setup and Configuration](#setup-and-configuration)
- [Repository Basics](#repository-basics)
- [Stage and Commit](#stage-and-commit)
- [Inspect Changes and History](#inspect-changes-and-history)
- [Branches](#branches)
- [Merge](#merge)
- [Remote Repositories](#remote-repositories)
- [Clone](#clone)
- [Undo Changes](#undo-changes)
- [Stash](#stash)
- [Rebase](#rebase)
- [Git Ignore](#git-ignore)
- [Forks and Pull Requests](#forks-and-pull-requests)
- [Typical Feature-Branch Workflow](#typical-feature-branch-workflow)
- [Safety Reminders](#safety-reminders)

---

## Setup and Configuration

```bash
# Check the installed Git version
git --version

# Set your commit identity
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# View configuration values
git config --list
```

`user.name` and `user.email` are written into commits; they are not the same as authenticating to GitHub. Use the email address you want associated with your commits.

## Repository Basics

```bash
# Initialize a repository in the current directory
git init

# Show the current branch and working-tree state
git status
```

`git init` creates a new local Git repository. Use `git status` frequently to check which branch you are on and whether files are modified, staged, or untracked.

## Stage and Commit

```bash
# Stage one specific file
git add filename

# Stage all changes under the current directory
git add .

# Review what is staged
git diff --staged

# Commit staged changes
git commit -m "Add analysis"
```

Prefer staging specific files when practical. Before committing, inspect the staged diff to avoid including unrelated or sensitive files.

## Inspect Changes and History

```bash
# Show commit history
git log

# Compact commit history
git log --oneline

# Show unstaged changes
git diff

# Show staged changes
git diff --staged
```

Use `git diff` to inspect unstaged edits and `git diff --staged` to inspect the snapshot that the next commit will record.

## Branches

```bash
# List local branches
git branch

# Create a branch without switching to it
git branch feature-name

# Switch to an existing branch
git switch feature-name
git switch main

# Create and switch to a new branch
git switch -c feature-name

# Delete a local branch that has been merged
git branch -d feature-name
```

A branch is a lightweight reference to a commit. Replace example branch names with the actual names used by your project. `git branch -d` may refuse to delete a branch Git considers unmerged; do not force-delete it unless you understand the consequences.

## Merge

```bash
# Switch to the intended target branch
git switch main

# Integrate the feature branch
git merge feature-name
```

A merge integrates histories. Depending on the branch history and options, Git may create a merge commit or perform a fast-forward. Resolve any conflicts and inspect the result before continuing work.

## Remote Repositories

```bash
# List configured remotes and their URLs
git remote -v

# Add a remote named origin
git remote add origin <repository-url>

# Push local main to origin's main branch
git push origin main

# Fetch and integrate updates from origin/main into the current branch
git pull origin main

# Download remote updates without integrating them into the current branch
git fetch origin
```

`origin` is a conventional remote name, not a special Git keyword. `git pull origin main` fetches from `origin` and integrates the specified remote branch into the current branch; it does not necessarily mean “update local main” unless you are currently on local `main`.

For a new feature branch, publish it and configure upstream tracking with:

```bash
git push -u origin feature-name
```

## Clone

```bash
# Clone a remote repository to a new local directory
git clone <repository-url>

# Move into the cloned directory (replace with the actual directory name)
cd project
```

Cloning creates a local repository from a remote repository. Replace the placeholder URL and directory name with real values.

## Undo Changes

```bash
# Unstage a file, keeping its working-tree changes
git restore --staged filename

# Discard unstaged changes in a tracked file
git restore filename

# Move HEAD back one commit while keeping changes staged
git reset --soft HEAD~1

# Create a new commit that reverses the effects of an existing commit
git revert <commit-id>
```

**Caution:** `git restore filename` discards unstaged edits to that tracked file. Check `git status` and inspect your changes before running it.

`git reset --soft HEAD~1` moves the current branch tip back one commit while keeping the changes from that commit staged. `git revert` records a new commit that reverses another commit's changes, which is generally suitable for undoing changes already shared with others.

The following command is destructive to tracked working-tree and index changes:

```bash
git reset --hard
```

Use it only when you understand exactly what will be discarded. It is not a routine cleanup command.

## Stash

```bash
# Stash changes to tracked files
git stash

# Include untracked files too
git stash push -u -m "Work in progress"

# List saved stashes
git stash list

# Apply the latest stash and remove its entry if successful
git stash pop

# Apply the latest stash but keep its entry
git stash apply
```

By default, `git stash` does not include untracked files. Use `-u` when you need to include them. Check the result with `git status`, especially if applying a stash produces conflicts.

## Rebase

```bash
# Switch to the feature branch
git switch feature-branch

# Replay its commits onto local main
git rebase main

# Continue after resolving a conflict
git rebase --continue

# Cancel an in-progress rebase
git rebase --abort
```

Rebase recreates commits on a new base, so the replayed commits receive new identities. Avoid rebasing shared history unless you understand the impact and have coordinated with the team. If you need the latest remote base, fetch first and use the intended reference, such as `origin/main`.

## Git Ignore

Create or update a `.gitignore` file to tell Git which untracked files and paths to ignore.

Example `.gitignore` entries:

```gitignore
# Environment variables and local secrets
.env
.env.*
!.env.example

# Python cache
__pycache__/
*.py[cod]

# Virtual environments
.venv/
venv/

# Editor and operating-system files
.vscode/
.DS_Store
Thumbs.db
```

Adjust these patterns to the needs of your project. **`.gitignore` does not stop tracking a file that is already tracked.** Never commit passwords, API keys, access tokens, or private credentials.

## Forks and Pull Requests

Typical fork-based contribution flow:

```text
Original repository
        |
       Fork
        |
        v
    Your GitHub fork
        |
       Clone
        |
        v
    Local repository
        |
   Feature branch
        |
     Commit + push
        |
        v
  Pull request to original
```

Common remote conventions:

```text
origin   → your fork
upstream → original repository
```

Add the original repository as `upstream` and inspect remotes:

```bash
git remote add upstream <original-repository-url>
git remote -v
git fetch upstream
```

A pull request is a GitHub collaboration feature, not a Git command. It proposes changes from a source branch to a target branch for review before integration.

## Typical Feature-Branch Workflow

This example assumes the repository uses `main` as its base branch and the remote is named `origin`.

```bash
# Update the local base branch
git switch main
git pull

# Create a feature branch
git switch -c feature/my-feature

# Make changes, then inspect the working tree
git status
git diff

# Stage intended files and review the staged snapshot
git add path/to/file
git diff --staged

# Commit the change
git commit -m "Add my feature"

# Push the feature branch to the remote
git push -u origin feature/my-feature
```

Then open a pull request on GitHub, confirm the source and target branches, describe the change, and follow the project's review and testing requirements.

## Safety Reminders

- Run `git status` before and after operations you are unsure about.
- Review `git diff` and `git diff --staged` before committing.
- Stage only the files you intend to commit.
- Do not commit secrets or credentials.
- Understand the effect of destructive commands before running them.
- Be cautious when rewriting commits that have already been shared.
- Follow your repository's branch, review, and merge policies.

## Official References

- [Git documentation](https://git-scm.com/docs)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [GitHub Docs](https://docs.github.com/)
