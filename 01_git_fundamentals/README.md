# Git Fundamentals

## What is Version Control?

Version control is a system for tracking changes made to files over time.

Without version control, a project can quickly become messy:

```text
project_final
project_final_v2
project_final_latest
project_final_latest2
project_final_REAL_FINAL
```

Version control solves this by maintaining a history of changes.

---

## What is Git?

Git is a **distributed version control system**.

It runs on my computer and tracks changes in a project.

Git allows me to:

* Track file changes
* Create commits
* Compare versions
* Create branches
* Merge changes
* Recover previous versions
* Work with remote repositories

---

## Git vs GitHub

These are not the same thing.

### Git

Git is the version-control software.

It works locally on my computer.

```text
My Computer
    │
    └── Git
         ├── Track changes
         ├── Create commits
         ├── Create branches
         └── Maintain history
```

### GitHub

GitHub is a cloud platform for hosting Git repositories and collaborating with other people.

```text
My Computer
     │
     │ git push
     ▼
   GitHub
```

Git can work without GitHub.

For example, I can initialize a Git repository and make commits entirely on my laptop.

---

## Repository

A repository is a project managed by Git.

After running:

```bash
git init
```

Git creates a hidden `.git` directory.

```text
my-project/
│
├── data/
├── src/
├── README.md
└── .git/
```

The `.git` directory contains Git's internal information and history.

I should not manually modify or delete it.

---

## Commit

A commit is a recorded snapshot of the project's staged changes.

Example:

```bash
git add README.md
git commit -m "Add project documentation"
```

A commit contains information such as:

* Changes
* Author
* Timestamp
* Commit message
* Commit identifier

Good commit messages describe what changed.

```text
Add sales analysis
Fix customer segmentation
Update README
Add data cleaning script
```

---

## Branch

A branch allows me to work on a separate line of development.

For example:

```text
main
 │
 ├── feature-dashboard
 │
 └── feature-cleaning
```

I can work on a feature without directly modifying the main branch.

---

## Remote

A remote is a reference to another copy of the repository, usually hosted on a service such as GitHub.

The conventional name for the primary remote is:

```text
origin
```

Example:

```bash
git remote -v
```

---

## Clone

`git clone` creates a local copy of a remote repository.

```bash
git clone https://github.com/user/project.git
```

This gives me the project files and Git history locally.

---

## My Mental Model

I think about Git as:

```text
Files
  ↓
Track changes
  ↓
Create snapshots
  ↓
Create branches
  ↓
Combine work
  ↓
Share through GitHub
```

