# Git Workflow

> A practical guide to moving changes from your working directory into Git history—and, when needed, to GitHub.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain the working directory, staging area, and local repository.
- Use `git status`, `git diff`, `git add`, `git commit`, and `git log` in a normal workflow.
- Choose exactly which changes belong in a commit.
- Explain why a local commit does not automatically appear on GitHub.
- Push committed changes to a remote repository when the remote is configured.

## Contents

- [The Three Main Areas](#the-three-main-areas)
- [The Everyday Workflow](#the-everyday-workflow)
- [1. Working Directory](#1-working-directory)
- [2. Staging Area](#2-staging-area)
- [3. Local Repository](#3-local-repository)
- [Why the Staging Area Matters](#why-the-staging-area-matters)
- [Inspect Changes Before Committing](#inspect-changes-before-committing)
- [Local Workflow vs. GitHub Workflow](#local-workflow-vs-github-workflow)
- [Common Mistakes](#common-mistakes)
- [Practice Exercise](#practice-exercise)
- [Quick Reference](#quick-reference)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## The Three Main Areas

Git's everyday workflow moves changes through three areas:

```text
Working Directory
   (edit files)
       |
       | git add
       v
Staging Area
 (select changes)
       |
       | git commit
       v
Local Repository
 (commit history)
```

These areas have different purposes:

| Area | What it means |
|---|---|
| Working directory | The checked-out files you are currently editing. |
| Staging area (index) | The proposed content for the next commit. |
| Local repository | The commits and history stored in your local Git repository. |

The staging area is not a separate online location. It is part of your local repository's workflow.

## The Everyday Workflow

A useful default sequence is:

```bash
# 1. Check the current state
git status

# 2. Review what changed
git diff

# 3. Select changes for the next commit
git add analysis.py

# 4. Check what is staged
git diff --staged

# 5. Record the staged changes
git commit -m "Update sales analysis"

# 6. Review recent commit history
git log --oneline
```

If your local branch is connected to a remote branch and you want to publish the commit, push it:

```bash
git push origin main
```

Replace `main` with your actual branch name if it differs. The command assumes that `origin` is the configured remote name and that you have permission to push.

## 1. Working Directory

The working directory contains the checked-out files you can open and modify.

For example:

```text
project/
├── README.md
├── analysis.py
└── sales.csv
```

If you edit `analysis.py`, the edit exists in your working directory. Git can report the change, but it is not automatically staged or committed.

Check the current state:

```bash
git status
```

Review unstaged changes to tracked files:

```bash
git diff
```

`git diff` compares the working directory with the staging area. It normally shows changes that have not yet been staged.

**Important:** `git status` and `git diff` do not save a commit. They help you inspect the current state.

## 2. Staging Area

The staging area—also called the **index**—lets you choose the content that will be recorded in the next commit.

Stage one file:

```bash
git add analysis.py
```

Stage multiple specific files:

```bash
git add analysis.py README.md
```

Stage all changes under the current directory, including new files and modifications that Git can add:

```bash
git add .
```

Use `git add .` deliberately. It may stage more than you intended, depending on which files have changed under the current directory. Check the result before committing:

```bash
git status
git diff --staged
```

`git diff --staged` (also written `git diff --cached`) shows the changes staged for the next commit.

### Staging is a selection step

Imagine you modified three files:

```text
analysis.py
README.md
dashboard.pbix
```

If only the Python change belongs in the next commit, stage only that file:

```bash
git add analysis.py
git diff --staged
git commit -m "Update analysis logic"
```

The other unstaged changes remain in the working directory. They are not included in that commit.

A commit records the staged content—not automatically every change in every file on your computer.

## 3. Local Repository

After you stage the changes, create a commit:

```bash
git commit -m "Update sales analysis"
```

A commit records the staged snapshot in your **local** Git history. The message should describe the change clearly and briefly.

View recent commits:

```bash
git log --oneline
```

Inspect the latest commit and its changes:

```bash
git show
```

If there is nothing staged, Git may tell you there is nothing to commit. Check `git status` and stage the intended changes first.

## Inspect Changes Before Committing

A reliable workflow checks both unstaged and staged changes.

| Command | What it helps you inspect |
|---|---|
| `git status` | Which files are modified, staged, untracked, or otherwise in a notable state. |
| `git diff` | Changes in tracked files that are not staged. |
| `git diff --staged` | Changes currently staged for the next commit. |
| `git log --oneline` | Recent local commit history. |
| `git show` | Details of a commit, including its recorded changes. |

A file can have both staged and unstaged edits. For example, you might stage a change, edit the file again, and then run `git status`. Git can show the staged version and the newer unstaged edits separately. Review both diffs before committing.

## Why the Staging Area Matters

Staging helps you make **focused, understandable commits**.

For example, suppose you changed:
- `analysis.py` to fix a calculation;
- `README.md` to improve project documentation;
- `dashboard.pbix` to update a dashboard.

These changes may represent different tasks. You could create a commit for the calculation first:

```bash
git add analysis.py
git diff --staged
git commit -m "Fix sales calculation"
```

Then stage and commit the documentation separately:

```bash
git add README.md
git diff --staged
git commit -m "Improve project documentation"
```

This keeps the commit history easier to review and makes each commit's purpose clearer.

## Local Workflow vs. GitHub Workflow

A local commit does **not** automatically appear on GitHub. Pushing is a separate step.

```text
Edit files
    |
    v
git status / git diff
    |
    v
git add
    |
    v
git diff --staged
    |
    v
git commit
    |
    v
Local Git history
    |
    | git push
    v
Remote repository (for example, GitHub)
```

The commands have distinct jobs:

### `git add`

Selects changes for the next commit by updating the staging area.

### `git commit`

Records the staged snapshot in the local repository's history.

### `git push`

Transfers local commits to a remote repository. It does not simply upload every uncommitted file.

Example:

```bash
git push origin main
```

Here, `origin` is a common name for a remote and `main` is the branch being pushed. Your repository may use a different remote name or branch name.

If you have not configured a remote or upstream branch, Git may ask you to configure one. Check your setup with:

```bash
git remote -v
git branch --show-current
```

To bring remote changes into your current branch, `git pull` is commonly used. It fetches remote changes and then integrates them into the current branch, using the configured strategy. Learn about remotes and collaboration in the later sections of this repository before relying on this in a shared project.

## Common Mistakes

- **Running `git commit` before staging:** Only staged changes are included in the commit.
- **Assuming `git add` uploads files:** It updates the local staging area; it does not send changes to GitHub.
- **Assuming `git commit` publishes changes:** The commit is local until pushed to a remote.
- **Using `git add .` without checking:** You might stage unrelated files. Inspect `git status` and `git diff --staged`.
- **Skipping the diff review:** You may commit debugging output, accidental edits, or unrelated changes.
- **Thinking staging permanently freezes a file:** If you edit the file after staging, the newer edits can remain unstaged.

## Practice Exercise

Try this in a test repository or a safe practice folder.

1. Create a file named `workflow_practice.md` and add a short line.
2. Run `git status`.
3. Stage the file with `git add workflow_practice.md`.
4. Run `git status` and `git diff --staged`.
5. Commit it with a clear message.
6. Run `git log --oneline`.
7. Add a second line to the file and run `git diff`.
8. Stage the second change, inspect `git diff --staged`, and make a second commit.

After completing the exercise, explain in your own words:
- What changed when you ran `git add`?
- What changed when you ran `git commit`?
- Why did the second edit need to be staged again?
- Would either commit appear on GitHub without a push?

## Quick Reference

| Goal | Command |
|---|---|
| Check repository state | `git status` |
| View unstaged changes | `git diff` |
| Stage one file | `git add filename` |
| Stage selected files | `git add file1 file2` |
| Stage changes under the current directory | `git add .` |
| Review staged changes | `git diff --staged` |
| Commit staged changes | `git commit -m "Describe the change"` |
| View recent commits | `git log --oneline` |
| Inspect the latest commit | `git show` |
| List configured remotes | `git remote -v` |
| Show current branch | `git branch --show-current` |
| Push a branch to a remote | `git push origin branch-name` |

## Next Steps

Continue learning with:

- [Essential Git Commands](../03_essential_git_commands/README.md)
- [Undoing Changes](../05_undoing_changes/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `git status`](https://git-scm.com/docs/git-status)
- [Git reference: `git add`](https://git-scm.com/docs/git-add)
- [Git reference: `git diff`](https://git-scm.com/docs/git-diff)
- [Git reference: `git commit`](https://git-scm.com/docs/git-commit)
- [Git reference: `git push`](https://git-scm.com/docs/git-push)
