# Essential Git Commands

## Initialize a Repository

```bash
git init
```

Creates a new Git repository in the current directory.

---

## Check Repository Status

```bash
git status
```

Shows:

* Modified files
* Untracked files
* Staged files
* Current branch
* Other useful repository information

This is one of the commands I should use frequently.

---

## Stage a File

```bash
git add filename.py
```

---

## Stage Multiple Files

```bash
git add file1.py file2.py
```

---

## Stage Everything

```bash
git add .
```

Use this carefully when the repository contains files that should not be part of the commit.

A good `.gitignore` helps prevent unwanted files from being staged.

---

## Commit

```bash
git commit -m "Add customer analysis"
```

A commit should represent a logical unit of work.

---

## View Commit History

```bash
git log
```

Compact version:

```bash
git log --oneline
```

---

## See Unstaged Changes

```bash
git diff
```

This shows changes between the working directory and the staged version.

---

## Typical Workflow

```bash
git status
git add .
git commit -m "Update customer analysis"
git log --oneline
```

If the repository has a remote:

```bash
git push origin main
```

---

## Quick Reference

| Command      | Purpose                               |
| ------------ | ------------------------------------- |
| `git init`   | Initialize repository                 |
| `git status` | Check current state                   |
| `git add`    | Stage changes                         |
| `git commit` | Create a commit                       |
| `git log`    | View history                          |
| `git diff`   | Inspect changes                       |
| `git push`   | Upload commits                        |
| `git pull`   | Download and integrate remote changes |
| `git clone`  | Copy a remote repository locally      |
| `git branch` | Work with branches                    |
| `git switch` | Switch branches                       |
| `git merge`  | Combine branches                      |

