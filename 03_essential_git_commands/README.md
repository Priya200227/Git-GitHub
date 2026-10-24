# Essential Git Commands

> A practical command reference for initializing repositories, inspecting changes, staging work, creating commits, and working with branches and remotes.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Initialize a local Git repository and check its status.
- Stage selected changes and review them before committing.
- Inspect differences and view commit history.
- Explain the difference between local commands and remote operations.
- Recognize the commands used to clone repositories and work with branches.

## Contents

- [Start a Repository](#start-a-repository)
- [Inspect Repository State](#inspect-repository-state)
- [Stage Changes](#stage-changes)
- [Commit Changes](#commit-changes)
- [Inspect Changes and History](#inspect-changes-and-history)
- [Work with Remotes](#work-with-remotes)
- [Work with Branches](#work-with-branches)
- [Typical Workflow](#typical-workflow)
- [Common Mistakes and Safe Habits](#common-mistakes-and-safe-habits)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## Start a Repository

### `git init` — initialize a repository

```bash
git init
```

Creates a new, empty Git repository in the current directory by creating the repository metadata (normally in a `.git` directory). Existing files are not automatically committed; they remain files in the working directory until you stage and commit them.

Check the result:

```bash
git status
```

If you are starting a project from scratch, run `git init` from the project directory. Avoid accidentally initializing a repository in the wrong folder.

### `git clone` — copy an existing repository

```bash
git clone https://github.com/OWNER/REPOSITORY.git
```

Clones a remote repository into a new local directory, including its Git history and remote-tracking setup. Replace the example URL with the actual repository URL.

Use `git clone` when a repository already exists remotely. Use `git init` when you want to start a new repository in your current directory.

## Inspect Repository State

### `git status` — check the current state

```bash
git status
```

Shows useful information such as:

- The current branch.
- Changes staged for the next commit.
- Changes to tracked files that are not staged.
- Untracked files that Git is not yet tracking.
- Whether the branch is ahead of or behind its upstream branch, when that information is available.

Use it frequently, especially before staging, before committing, and after commands that change the repository state.

## Stage Changes

The staging area (also called the index) lets you select the content that will be included in the next commit.

### `git add filename.py` — stage one file

```bash
git add filename.py
```

Stages the current contents of that file for the next commit.

### `git add file1.py file2.py` — stage specific files

```bash
git add file1.py file2.py
```

Useful when you want to commit only selected files.

### `git add .` — stage changes under the current directory

```bash
git add .
```

Stages changes Git can add under the current directory, including new files and modifications. Be careful: it can include unrelated work. Review what was staged:

```bash
git status
git diff --staged
```

A `.gitignore` file tells Git which matching untracked files to ignore. It does not automatically remove files that are already tracked, and it is not a substitute for reviewing the staging area.

## Commit Changes

### `git commit -m "message"` — record staged changes

```bash
git commit -m "Add customer analysis"
```

Records the staged snapshot as a commit in the local repository's history. A commit should represent a logical unit of work and have a concise, descriptive message.

Before committing, inspect the staged changes:

```bash
git diff --staged
```

If Git reports that there is nothing to commit, check `git status` and confirm that you staged the intended changes.

## Inspect Changes and History

### `git diff` — inspect unstaged changes

```bash
git diff
```

Shows differences between the working directory and the staging area for tracked files. It normally shows edits that have not been staged.

### `git diff --staged` — inspect staged changes

```bash
git diff --staged
```

Shows differences between the staging area and the current commit. Review this before creating a commit.

### `git log` — view commit history

```bash
git log
```

Shows commit history with details such as commit identifiers, authors, dates, and messages.

For a compact view:

```bash
git log --oneline
```

To inspect a particular commit and its recorded changes:

```bash
git show <commit>
```

Replace `<commit>` with a commit hash or another valid commit reference. The angle-bracketed text is a placeholder, not a literal value to type.

## Work with Remotes

Local repository commands and remote operations have different purposes.

### `git remote -v` — list configured remotes

```bash
git remote -v
```

Displays configured remote names and their fetch/push URLs. `origin` is a common default name, but a repository may use another name.

### `git push` — publish local commits

```bash
git push origin main
```

Pushes commits from the local `main` branch to the `main` branch on the remote named `origin`, subject to the repository's configuration and permissions. Replace the remote and branch names if yours differ.

A push does not upload every uncommitted file in your working directory. Changes must be committed first.

### `git pull` — fetch and integrate remote changes

```bash
git pull
```

Fetches remote updates and integrates them into the current branch according to the configured upstream and pull strategy. Integration may use a fast-forward, merge, or rebase, depending on configuration and options.

Before pulling in a shared project, check your current state:

```bash
git status
```

If you need more control, learn the separate roles of `git fetch`, merge, and rebase in the later sections of this repository.

## Work with Branches

### `git branch` — list or manage branches

```bash
git branch
```

Lists local branches. The current branch is marked with an asterisk.

### `git switch` — switch branches

```bash
git switch main
```

Switches to an existing local branch named `main`.

To create and switch to a new branch:

```bash
git switch -c feature/customer-analysis
```

Use a descriptive branch name that reflects the task.

### `git merge` — integrate another branch

```bash
git switch main
git merge feature/customer-analysis
```

This example switches to `main` and merges the named branch into it. The target branch is the branch you are currently on; here, `main` receives the integration. A merge can complete as a fast-forward, create a merge commit, or report conflicts that need to be resolved.

Learn more in [Branching](../06_branching/README.md) and [Merging and Conflicts](../07_merging_and_conflicts/README.md).

## Typical Workflow

For a project where Git is already initialized:

```bash
# Check the repository state
git status

# Review unstaged edits
git diff

# Stage only the intended file
git add analysis.py

# Review the staged content
git diff --staged

# Record the staged changes
git commit -m "Update customer analysis"

# Review recent history
git log --oneline
```

If the repository has a configured remote and you intend to publish the commit, push the appropriate branch:

```bash
git push origin main
```

Use the branch you are actually working on. If the repository is new, or its upstream has not been configured, the setup may require additional steps.

## Common Mistakes and Safe Habits

- **Assuming `git init` commits existing files:** It creates repository metadata; you still need to stage and commit files.
- **Running commands in the wrong directory:** Check where you are before initializing or changing a project.
- **Using `git add .` without reviewing:** You may stage unrelated files or generated outputs.
- **Confusing `git diff` with `git diff --staged`:** They show different sets of changes.
- **Assuming `git commit` publishes to GitHub:** A commit is local until pushed.
- **Assuming `.gitignore` untracks existing files:** Ignoring rules affect untracked files; already tracked files remain tracked unless explicitly removed from the index.
- **Pushing the wrong branch:** Check the current branch with `git branch --show-current`.
- **Ignoring repository state before pulling:** Inspect `git status` and understand how remote changes will be integrated.

A safe habit is to run `git status` before and after important operations and review the staged diff before committing.

## Quick Reference

| Command | Purpose |
|---|---|
| `git init` | Initialize a repository in the current directory. |
| `git clone <url>` | Clone an existing repository locally. |
| `git status` | Inspect branch and file state. |
| `git add <file>` | Stage a file's current contents. |
| `git add file1 file2` | Stage selected files. |
| `git add .` | Stage addable changes under the current directory. |
| `git diff` | View unstaged changes to tracked files. |
| `git diff --staged` | View changes staged for the next commit. |
| `git commit -m "message"` | Record staged changes in local history. |
| `git log` | View commit history. |
| `git log --oneline` | View a compact commit history. |
| `git show <commit>` | Inspect a commit and its changes. |
| `git remote -v` | List configured remotes. |
| `git push origin main` | Push local commits from `main` to remote `origin`. |
| `git pull` | Fetch and integrate remote changes into the current branch. |
| `git branch` | List local branches. |
| `git switch <branch>` | Switch to an existing branch. |
| `git switch -c <branch>` | Create and switch to a new branch. |
| `git merge <branch>` | Merge the named branch into the current branch. |

## Practice Exercise

Use a test repository or a safe practice folder.

1. Run `git init` in a new project directory, or use an existing test repository.
2. Create a file named `practice.py` with a short comment.
3. Run `git status` and identify the untracked file.
4. Stage it with `git add practice.py`.
5. Review the staged change with `git diff --staged`.
6. Commit it with a descriptive message.
7. Edit the file again. Compare the output of `git diff` before and after staging the second change.
8. Run `git log --oneline` to see the commit history.

Check your understanding:

- What does `git init` create?
- What is the difference between `git diff` and `git diff --staged`?
- Why does `git add` not publish a change to GitHub?
- What is the difference between `git clone` and `git init`?
- Which branch receives the changes when you run `git merge`?

## Next Steps

Continue with:

- [Git Ignore](../04_gitignore/README.md)
- [Undoing Changes](../05_undoing_changes/README.md)
- [Branching](../06_branching/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git init`](https://git-scm.com/docs/git-init)
- [Git reference: `git status`](https://git-scm.com/docs/git-status)
- [Git reference: `git add`](https://git-scm.com/docs/git-add)
- [Git reference: `git commit`](https://git-scm.com/docs/git-commit)
- [Git reference: `git diff`](https://git-scm.com/docs/git-diff)
- [Git reference: `git log`](https://git-scm.com/docs/git-log)
- [Git reference: `git push`](https://git-scm.com/docs/git-push)
- [Git reference: `git pull`](https://git-scm.com/docs/git-pull)
- [Git reference: `git branch`](https://git-scm.com/docs/git-branch)
- [Git reference: `git switch`](https://git-scm.com/docs/git-switch)
- [Git reference: `git merge`](https://git-scm.com/docs/git-merge)
