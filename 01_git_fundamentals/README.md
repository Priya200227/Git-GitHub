# Git Fundamentals

> Understand how Git records changes, organizes project history, and supports reliable development workflows.

[← Back to the repository overview](../README.md)

## Learning objectives

By the end of this section, you should be able to:

- Explain version control and why Git is useful.
- Distinguish Git from GitHub.
- Describe a repository, working tree, staging area, commit, branch, and remote.
- Explain how changes move from your working tree into a commit and, when needed, to a remote repository.
- Initialize or clone a repository, inspect changes, create commits, and review history.
- Apply basic repository hygiene and avoid common Git mistakes.

## Contents

1. [Version control](#1-version-control)
2. [Git and GitHub](#2-git-and-github)
3. [The Git data model and workflow](#3-the-git-data-model-and-workflow)
4. [Core concepts](#4-core-concepts)
5. [A practical first workflow](#5-a-practical-first-workflow)
6. [Essential inspection commands](#6-essential-inspection-commands)
7. [Repository hygiene with `.gitignore`](#7-repository-hygiene-with-gitignore)
8. [Common misconceptions](#8-common-misconceptions)
9. [Safe working habits](#9-safe-working-habits)
10. [Knowledge check](#10-knowledge-check)
11. [Next steps](#11-next-steps)
12. [References](#references)

---

## 1. Version control

**Version control** is a system for recording changes to files over time. It helps you inspect how a project evolved, compare versions, collaborate, and recover earlier states.

Without version control, project folders can accumulate ambiguous copies:

```text
analysis_final
analysis_final_v2
analysis_final_latest
analysis_final_latest_fixed
analysis_final_really_final
```

Version control replaces this ad hoc approach with a history of changes.

### Why version control matters

- **History:** inspect how a project evolved.
- **Traceability:** connect changes to commits and messages.
- **Experimentation:** work on a feature separately from the main line of development.
- **Collaboration:** integrate contributions from multiple people.
- **Recovery:** restore or reapply earlier work when necessary.

Version control is not a complete backup strategy. A local repository can be lost if the device fails, and a remote repository can be deleted or become inaccessible. Keep independent backups for important work.

## 2. Git and GitHub

**Git** is a free, open-source, distributed version control system. It records project history and supports commits, branching, merging, comparison, and collaboration.

**GitHub** is a platform that hosts Git repositories and provides collaboration features such as pull requests, issues, code review, and access management.

| Git | GitHub |
|---|---|
| Version-control software | A hosting and collaboration platform |
| Can be used locally without an internet connection for many operations | Provides hosted repositories and web-based collaboration tools |
| Records commits and manages branches | Hosts Git repositories and adds features around them |
| Does not require GitHub | Uses Git as a core part of its repository workflow |

Git can be used without GitHub. GitHub is one of several services that can host a remote Git repository.

---

## 3. The Git data model and workflow

The most useful beginner mental model distinguishes the **working tree**, **staging area**, **local repository**, and **remote repository**.

```text
┌──────────────────────────────┐
│          Working tree        │
│      Files you edit          │
└──────────────┬───────────────┘
               │ git add
               ▼
┌──────────────────────────────┐
│         Staging area         │
│ Changes selected to commit   │
└──────────────┬───────────────┘
               │ git commit
               ▼
┌──────────────────────────────┐
│       Local repository       │
│   Commits and project history│
└──────────────┬───────────────┘
               │ git push
               ▼
┌──────────────────────────────┐
│      Remote repository       │
│    e.g. a GitHub repository  │
└──────────────────────────────┘
```

This is a conceptual workflow, not a one-way process that every Git operation must follow. You can make many local commits before pushing, and you can fetch remote updates before deciding how to integrate them.

### Working tree

The **working tree** (also called the working directory) contains the checked-out project files you view and edit. Changes here are not automatically part of the next commit.

```bash
git status
git diff
```

- `git status` summarizes the state of the working tree and staging area.
- `git diff` shows unstaged changes to tracked files.

### Staging area (index)

The **staging area**, also called the *index*, is where you select changes for the next commit. You can stage one file while leaving changes in another file unstaged.

```bash
git add README.md
git diff --staged
```

`git diff --staged` shows the changes currently staged for the next commit.

### Local repository

The **local repository** stores Git objects and references, including commits and the data required to represent project history. In a standard non-bare repository, Git's metadata is stored in the `.git` directory.

```bash
git log --oneline
```

Do not manually delete or edit `.git` unless you understand the consequences; it contains essential repository metadata.

### Remote repository

A **remote** is a named reference to another repository, often hosted on GitHub. Remotes allow repositories to exchange commits and related references.

```bash
git remote -v
git fetch origin
```

`origin` is the conventional name for the remote created by `git clone`. It is a name, not a special Git keyword. A repository can have multiple remotes or no remotes.

### The three commands to remember

| Command | What it does |
|---|---|
| `git add` | Stages selected changes locally |
| `git commit` | Records a commit from the staged snapshot in the local repository |
| `git push` | Transfers commits and references to a remote, subject to permissions and remote rules |

A commit is not a push. Committing records work locally; pushing shares it with a remote.

---

## 4. Core concepts

### Repository

A **repository** (or *repo*) is a Git-managed project and its history. A typical working repository contains project files and a `.git` directory.

```text
my-project/
├── data/
├── src/
├── README.md
└── .git/
```

Initialize a repository in the current directory:

```bash
git init
```

`git init` creates the repository metadata. It does not automatically stage or commit existing files.

### Commit

A **commit** records a snapshot of the project as represented by the staging area at that moment. It includes metadata such as the author, timestamp, commit message, object ID, and parent commit or commits.

```bash
git add README.md
git commit -m "Document project setup"
```

Prefer small, coherent commits with messages that explain the change.

Examples:

```text
Add validation to data ingestion
Fix null handling in customer records
Document local development setup
```

### Branch

A **branch** is a movable reference to a commit. Branches let you develop features, fixes, or experiments separately from another line of development.

```text
main:                    A---B---C
                              \
feature/data-cleaning:          D---E
```

This graph is illustrative; actual commit graphs depend on the repository history.

Create and switch to a new branch:

```bash
git switch -c feature/data-cleaning
```

Switch back to `main`:

```bash
git switch main
```

### Merge

A **merge** integrates changes from one branch into another. Depending on the commit graph and options, Git may perform a fast-forward update or create a merge commit.

```bash
git switch main
git merge feature/data-cleaning
```

A merge can produce conflicts when Git cannot automatically reconcile changes. Conflict resolution is covered in [Merging and Conflicts](../07_merging_and_conflicts/).

### Clone

`git clone` creates a local repository from an existing repository URL. It normally creates a working directory, downloads repository history, and configures the source remote as `origin`.

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Replace the example URL with the actual repository URL. Unlike downloading a ZIP archive, cloning gives you Git repository data that supports normal Git workflows.

### Fetch, pull, and push

- **`git fetch`** downloads remote updates and references without automatically integrating them into the current branch.
- **`git pull`** fetches updates and then integrates them into the current branch, usually by merging or rebasing depending on configuration and options.
- **`git push`** sends local commits and references to a remote repository, subject to permissions and remote rules.

```bash
git fetch origin
git pull origin main
git push origin main
```

These commands are not interchangeable. Fetch is useful when you want to inspect remote updates before integrating them.

---

## 5. A practical first workflow

The following exercise creates a small local repository. Run these commands in a terminal where Git is installed.

### Step 1: Verify Git is installed

```bash
git --version
```

### Step 2: Configure commit identity

Set the name and email Git should associate with new commits. Use the identity appropriate for your work.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

These settings supply commit metadata; they do not authenticate you to GitHub.

### Step 3: Create a practice repository

```bash
mkdir git-practice
cd git-practice
git init
```

Create a `README.md` file containing a short project description.

### Step 4: Inspect the state

```bash
git status
```

Git reports the current branch and the state of files, including untracked, modified, and staged changes.

### Step 5: Stage and inspect a change

```bash
git add README.md
git status
git diff --staged
```

Review the staged diff before committing. Staging is a deliberate selection step: only staged changes are included in the next commit.

### Step 6: Create a commit

```bash
git commit -m "Add initial project README"
```

If Git reports that there is nothing to commit, run `git status` and confirm that the intended changes were staged.

### Step 7: Review the history

```bash
git log --oneline
git show --stat
```

`git log` displays commit history. `git show --stat` displays information about a commit and a summary of the files it changed.

### Step 8: Connect to a remote (optional)

Create an appropriate repository on GitHub, then use the remote URL provided by GitHub:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git remote -v
```

If your local branch is named `main` and the remote is ready for its first push:

```bash
git push -u origin main
```

The `-u` option sets the upstream tracking relationship for the branch. If the remote already has commits, you may need to reconcile the histories before pushing. Do not blindly overwrite remote history.

---

## 6. Essential inspection commands

| Command | Purpose |
|---|---|
| `git status` | Show the state of the working tree and staging area |
| `git diff` | Show unstaged changes to tracked files |
| `git diff --staged` | Show staged changes |
| `git log --oneline` | Display a compact commit history |
| `git show` | Inspect a commit |
| `git branch` | List local branches |
| `git switch BRANCH` | Switch to an existing branch |
| `git remote -v` | List configured remotes |
| `git fetch origin` | Fetch updates from `origin` |
| `git help COMMAND` | Open help for a Git command |

Replace placeholders such as `BRANCH` and `COMMAND` with actual names.

---

## 7. Repository hygiene with `.gitignore`

A `.gitignore` file specifies patterns for **untracked** files and paths that Git should ignore. It is commonly used for caches, generated artifacts, local environment files, and editor or operating-system files.

Example for a Python project:

```gitignore
# Python cache
__pycache__/
*.py[cod]

# Virtual environment
.venv/

# Environment variables and local secrets
.env

# Editor and operating-system files
.vscode/
.DS_Store
Thumbs.db
```

Adapt these patterns to the project. Do not ignore files the project needs to run.

Important distinctions:

- `.gitignore` does not automatically stop tracking files already tracked by Git.
- `.gitkeep` is a naming convention sometimes used to keep an otherwise empty directory represented in Git; it is not a special Git command or feature.
- Never commit passwords, API keys, access tokens, or private data. If a secret is committed, revoke or rotate it; deleting it in a later commit does not make the exposed secret safe.

---

## 8. Common misconceptions

1. **Git and GitHub are the same.** Git is version-control software; GitHub is a hosting and collaboration platform.
2. **Saving a file creates a commit.** Saving edits the working tree. Staging and committing are separate operations.
3. **`git add` uploads files to GitHub.** It stages changes locally.
4. **A commit is automatically on GitHub.** A local commit is not shared until it is pushed or transferred by another method.
5. **A branch is a separate full copy of the project.** A branch is a movable reference to a commit; switching branches updates the checked-out working tree.
6. **`.gitignore` untracks any matching file.** It does not automatically untrack files already in the index.
7. **`git pull` only downloads data.** It also integrates fetched changes into the current branch.
8. **Git is a complete backup system.** Git records history, but independent backups are still important.

---

## 9. Safe working habits

- Run `git status` before staging, committing, switching branches, or troubleshooting.
- Review `git diff` and `git diff --staged` so you know what you are committing.
- Prefer small, coherent commits with clear messages.
- Keep secrets and private datasets out of the repository.
- Treat destructive commands such as `git reset --hard` and `git clean` with care.
- Before rewriting shared history or deleting branches, confirm which changes are local and which have been shared.
- Use remote repositories and independent backups for important work.
- Test command sequences in a disposable practice repository before applying unfamiliar commands to important projects.

---

## 10. Knowledge check

Try answering these without looking back:

1. What problem does version control solve?
2. How is Git different from GitHub?
3. What is the difference between the working tree and the staging area?
4. What exactly does a commit record?
5. Why is a commit different from a push?
6. What does `git fetch` do that `git pull` does not do by itself?
7. What is a branch, conceptually?
8. What does `.gitignore` do, and what does it not do?
9. Which commands show unstaged and staged changes?
10. Why should Git not be treated as a complete backup strategy?

### Hands-on exercise

In a disposable practice repository:

1. Create a `README.md` and make an initial commit.
2. Edit two files.
3. Stage only one file and inspect the staged diff.
4. Commit the staged change.
5. Create a feature branch and make another commit.
6. Switch back to `main` and inspect the history.
7. Explain which changes are committed, staged, unstaged, local, and remote.

You should be able to explain the repository state after each command—not merely reproduce the commands.

---

## 11. Next steps

Continue through the learning repository in this order:

1. [Git Workflow](../02_git_workflow/) — understand the daily change-and-commit cycle.
2. [Essential Git Commands](../03_essential_git_commands/) — build a practical command reference.
3. [`.gitignore`](../04_gitignore/) — learn ignore rules and repository hygiene.
4. [Undoing Changes](../05_undoing_changes/) — understand restore, reset, and revert.
5. [Branching](../06_branching/) — create and manage branches.
6. [Merging and Conflicts](../07_merging_and_conflicts/) — integrate changes safely.
7. [GitHub Basics](../08_github_basics/) and [Remote Repositories](../09_remote_repositories/) — work with hosted repositories.
8. [Collaboration](../10_collaboration/), [Pull Requests](../11_pull_requests/), and [Forking](../12_forking/) — practice collaborative workflows.
9. [Git Stash](../13_git_stash/) and [Git Rebase](../14_git_rebase/) — explore additional workflows.

[← Back to the repository overview](../README.md)

## References

- [Git official documentation](https://git-scm.com/doc)
- [Pro Git — About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)
- [Pro Git — Getting a Git Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
- [GitHub Docs — About Git](https://docs.github.com/en/get-started/using-git/about-git)
- [Not Your College — Git & GitHub](https://youtu.be/BnEFaIfcwOU)
- [Ansh Lamba — Git & GitHub](https://youtu.be/Kr8l7rQGwNs)

---

*These notes are a personal learning resource. Concepts and commands should be practiced and checked against the official documentation when needed.*
