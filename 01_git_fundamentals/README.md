# Git Fundamentals

> A practical foundation for understanding Git's data model, everyday
> workflow, and core terminology.

## Learning objectives

By the end of this guide, you should be able to:

-   Explain version control and why Git is useful.
-   Distinguish Git from GitHub.
-   Describe a repository, commit, branch, remote, and working tree.
-   Explain the relationship between the working directory, staging
    area, and repository.
-   Create a repository, record changes, inspect history, and connect a
    remote.
-   Apply basic safety practices when working with Git.

## 1. What is version control?

**Version control** is a system for recording changes to files over
time. It helps you understand what changed, when it changed,
and---depending on the system and workflow---who made the change and
why.

Without version control, projects can accumulate ambiguous copies:

``` text
analysis_final
analysis_final_v2
analysis_final_latest
analysis_final_latest_fixed
analysis_final_really_final
```

Version control replaces this ad hoc approach with a traceable history
of changes. You can compare versions, collaborate with others, and
recover earlier project states when needed.

### Why it matters

-   **History:** review how a project evolved.
-   **Traceability:** associate changes with commits and messages.
-   **Experimentation:** develop a feature on a separate branch.
-   **Collaboration:** combine work from multiple contributors.
-   **Recovery:** restore or reapply earlier work when necessary.

Version control does not replace backups. A local repository can still
be lost if the device fails, and a remote repository can still be
misconfigured or deleted.

## 2. What is Git?

**Git is a free, open-source, distributed version control system.** It
records project history as a series of commits and supports branching,
merging, comparison, and collaboration.

Git is distributed because each clone normally contains the repository's
history, not just a live view of files on a central server. You can
inspect history and create commits locally without an internet
connection.

Git helps you:

-   Track changes to files.
-   Stage selected changes for the next commit.
-   Record commits with messages and metadata.
-   Compare versions and inspect history.
-   Create and switch between branches.
-   Merge work from different branches.
-   Connect to remote repositories and exchange commits.

## 3. Git vs. GitHub

Git and GitHub are related, but they are not the same.

  -----------------------------------------------------------------------
  Git                                 GitHub
  ----------------------------------- -----------------------------------
  Version-control software            A platform for hosting Git
                                      repositories and collaborating

  Runs locally and can run in other   Provides hosted repositories and
  environments                        collaboration features

  Records commits and manages         Adds pull requests, issues,
  branches                            reviews, permissions, and project
                                      tools

  Can be used without GitHub          Uses Git repositories as a core
                                      part of its workflow
  -----------------------------------------------------------------------

A typical workflow looks like this:

``` text
Local project
    |
    | git add / git commit
    v
Local Git repository
    |
    | git push
    v
Remote repository on GitHub
```

Git can be used without GitHub. GitHub is one possible remote hosting
and collaboration platform; other hosting services exist as well.

## 4. The four places to understand in Git

The most important beginner mental model is the relationship between the
working tree, staging area, local repository, and remote repository.

``` text
Working tree
  Files you edit
      |
      | git add
      v
Staging area (index)
  Changes selected for the next commit
      |
      | git commit
      v
Local repository
  Commits and project history
      |
      | git push
      v
Remote repository
  Shared copy hosted elsewhere
```

### Working tree (working directory)

The working tree is the checked-out set of project files you see and
edit. Changes here are not automatically included in the next commit.

Inspect working-tree changes with:

``` bash
git status
git diff
```

### Staging area (index)

The staging area is where you choose which changes should be included in
the next commit. You can stage some files while leaving other edits
unstaged.

``` bash
git add README.md
git add src/
git diff --staged
```

`git diff --staged` shows the changes staged for the next commit.

### Local repository

The local repository stores Git's objects and references, including
commits and the information needed to reconstruct tracked project
versions. It is maintained in the repository's `.git` directory in a
standard non-bare working repository.

``` bash
git log --oneline
```

Do not manually edit or delete `.git` unless you understand the
consequences. Doing so can damage or remove repository metadata and
history.

### Remote repository

A remote is a named reference to another repository, often hosted on
GitHub. Remotes let you exchange commits and related references with
another repository.

``` bash
git remote -v
git fetch origin
git push origin main
```

`origin` is the conventional default name for the remote created by
`git clone`; it is a name, not a special Git keyword. A repository can
have multiple remotes or none.

## 5. Core Git concepts

### Repository

A **repository** (or *repo*) is a Git-managed project and its history. A
normal working repository contains project files plus a `.git`
directory.

``` text
my-project/
├── data/
├── src/
├── README.md
└── .git/
```

Create a new local repository inside the current directory with:

``` bash
git init
```

`git init` initializes repository metadata; it does not automatically
stage or commit the existing files.

### Commit

A **commit** records a snapshot of the project as represented by the
staging area at that moment, along with metadata such as the author,
timestamp, message, and a unique object ID. Commits are linked to their
parent commit or commits, forming project history.

``` bash
git add README.md
git commit -m "Document project setup"
```

A good commit should ideally represent one coherent, understandable
change. Write a message that describes the change rather than saying
only that something was updated.

Examples:

``` text
Add data validation to ingestion script
Fix null handling in customer records
Document local development setup
```

A commit is not the same as a push. A commit records work in your local
repository; a push sends commits to a remote.

### Branch

A **branch** is a movable reference to a commit, commonly used to
develop work separately from another line of development. Branches make
it easier to isolate features, fixes, and experiments.

``` text
main:                 A---B---C
                           \
feature/data-cleaning:      D---E
```

This is a conceptual illustration, not a guarantee of the exact commit
graph in a particular repository.

Create and switch to a branch:

``` bash
git switch -c feature/data-cleaning
```

Switch back to the main branch:

``` bash
git switch main
```

### Merge

A **merge** integrates changes from one branch into another. Depending
on the commit graph and options, Git may create a merge commit or
perform a fast-forward update.

``` bash
git switch main
git merge feature/data-cleaning
```

Merging can produce conflicts when Git cannot automatically reconcile
competing changes. Conflict resolution is covered in the later branching
and merging section of this learning repository.

### Remote

A **remote** is a named connection to another Git repository. The name
`origin` is commonly used for the remote from which a repository was
cloned.

``` bash
git remote -v
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Replace the example URL with the URL of your own repository. Do not add
a remote if the correct one is already configured.

### Clone

`git clone` creates a local repository from an existing repository URL.
It typically creates a working directory, downloads the available
history, and configures the source remote as `origin`.

``` bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Cloning differs from downloading a ZIP archive: a clone includes Git
repository data and can participate in normal Git workflows.

### Fetch, pull, and push

-   **`git fetch`** downloads updates and references from a remote
    without automatically integrating them into your current branch.
-   **`git pull`** fetches updates and then integrates them into the
    current branch, usually by merging or rebasing according to
    configuration and options.
-   **`git push`** sends local commits and references to a remote,
    subject to permissions and remote rules.

``` bash
git fetch origin
git pull origin main
git push origin main
```

These commands are not interchangeable. In particular, `fetch` is useful
when you want to inspect remote updates before integrating them.

## 6. A practical first workflow

The following example assumes you have Git installed and are working in
a project directory.

### Step 1: Check your installation

``` bash
git --version
```

Configure the name and email that Git should associate with new commits.
Use the identity appropriate for your work.

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

These settings populate commit metadata; they do not log you into
GitHub.

### Step 2: Initialize a repository

``` bash
mkdir git-practice
cd git-practice
git init
```

Create a file such as `README.md` and add a short description.

### Step 3: Inspect the repository state

``` bash
git status
```

Git status reports the current branch and the state of files, including
untracked, modified, staged, and other relevant changes.

### Step 4: Stage the intended changes

``` bash
git add README.md
git status
```

Stage only what you intend to include. For a reviewable workflow,
inspect the staged diff before committing.

``` bash
git diff --staged
```

### Step 5: Create a commit

``` bash
git commit -m "Add initial project README"
```

If Git reports that there is nothing to commit, inspect `git status` and
confirm that the intended changes were staged.

### Step 6: Review history

``` bash
git log --oneline
git show --stat
```

`git log` displays commit history. `git show` displays information about
a commit and, by default, its patch.

### Step 7: Connect to GitHub (when appropriate)

Create an empty repository on GitHub, then connect your local repository
using the URL GitHub provides:

``` bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git remote -v
```

If your local branch is named `main` and the remote repository is ready
for its first push:

``` bash
git push -u origin main
```

The `-u` option sets the upstream tracking relationship for that branch,
making later pushes and pulls more convenient. If the remote already
contains commits, such as an initial README, the histories may need to
be reconciled before pushing.

## 7. Essential inspection commands

  Command               Purpose
  --------------------- -----------------------------------------------------
  `git status`          Show the state of the working tree and staging area
  `git diff`            Show unstaged changes
  `git diff --staged`   Show staged changes
  `git log --oneline`   Show a compact commit history
  `git show`            Inspect a commit
  `git branch`          List local branches
  `git switch BRANCH`   Switch branches
  `git remote -v`       List configured remotes
  `git fetch`           Download remote updates without integrating them
  `git help COMMAND`    Open help for a Git command

Replace placeholders such as `BRANCH` with actual branch names. For
detailed documentation, use `git help <command>` or the official Git
reference linked below.

## 8. `.gitignore` and untracked files

A `.gitignore` file tells Git which **untracked** files or paths to
ignore. It is commonly used for temporary files, local environment
configuration, generated artifacts, and secrets that should not be
committed.

Example `.gitignore`:

``` gitignore
# Python
__pycache__/
*.py[cod]
.venv/

# Environment variables and local secrets
.env

# Editor / operating-system files
.vscode/
.DS_Store
Thumbs.db
```

Adjust these patterns to the project. Do not ignore files that the
project needs to run, and do not assume `.gitignore` removes files
already tracked by Git. If a secret has been committed, removing it in a
later commit is not sufficient; rotate or revoke the secret and follow
the appropriate history-remediation process.

## 9. Common beginner misconceptions

1.  **"Git and GitHub are the same."** Git is version-control software;
    GitHub hosts repositories and adds collaboration features.
2.  **"Saving a file creates a commit."** Saving edits the working tree.
    You normally stage changes and commit them separately.
3.  **"`git add` uploads files to GitHub."** It stages changes locally;
    it does not upload them.
4.  **"A commit is automatically on GitHub."** A commit remains local
    until it is pushed or otherwise transferred.
5.  **"A branch is a separate full copy of the project."** A branch is a
    reference to a commit; checking out a branch changes the working
    tree to the selected version.
6.  **"`.gitignore` untracks anything listed in it."** It does not
    automatically stop tracking files already in the index.
7.  **"`git pull` only downloads data."** It also integrates fetched
    changes into the current branch.
8.  **"Git is a backup system."** Git provides history and recovery
    mechanisms, but it does not replace independent backups.

## 10. Safe working habits

-   Run `git status` before staging, committing, switching branches, or
    troubleshooting.
-   Review `git diff` and `git diff --staged` so you know what you are
    committing.
-   Prefer small, coherent commits with clear messages.
-   Never commit passwords, API keys, access tokens, private datasets,
    or other sensitive material.
-   Be careful with destructive commands such as `git reset --hard` and
    `git clean`.
-   Before rewriting history or deleting branches, confirm which changes
    are local and which have been shared.
-   Use a remote repository and an independent backup strategy for
    important work.

## 11. Knowledge check

Try answering these without looking back:

1.  What problem does version control solve?
2.  How is Git different from GitHub?
3.  What is the difference between the working tree and the staging
    area?
4.  What exactly does a commit record?
5.  Why is a commit different from a push?
6.  What does `git fetch` do that `git pull` does not do by itself?
7.  What is a branch, conceptually?
8.  What does `.gitignore` do---and what does it not do?
9.  Which commands would you use to inspect unstaged and staged changes?
10. Why should Git not be treated as a complete backup strategy?

## 12. Recommended learning sequence

Continue through the repository in this order:

1.  **Git fundamentals** --- concepts and mental models.
2.  **Git workflow** --- the working tree, staging, commits, and daily
    command patterns.
3.  **Essential Git commands** --- practical command reference.
4.  **`.gitignore`** --- ignore rules and repository hygiene.
5.  **Undoing changes** --- safe recovery and the difference between
    restore, reset, and revert.
6.  **Branching** --- creating, switching, and managing branches.
7.  **Merging and conflicts** --- integrating work and resolving
    conflicts.
8.  **GitHub basics and remote repositories** --- publishing and
    synchronizing work.
9.  **Collaboration and pull requests** --- review and team workflows.

## References

-   [Git official documentation](https://git-scm.com/doc)
-   [Pro Git book --- Getting
    Started](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)
-   [Pro Git book --- Git
    Basics](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
-   [GitHub Docs --- About
    Git](https://docs.github.com/en/get-started/using-git/about-git)

------------------------------------------------------------------------

*Purpose: foundational study notes for a practical Git learning
repository.*
