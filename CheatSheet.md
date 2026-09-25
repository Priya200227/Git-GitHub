# Git & GitHub Cheat Sheet

## Setup

```bash
git --version

git config --global user.name "Your Name"
git config --global user.email "your@email.com"

git config --list
```

---

## Repository

```bash
git init
git status
```

---

## Stage & Commit

```bash
git add filename
git add .

git commit -m "Add analysis"
```

---

## History

```bash
git log
git log --oneline
git diff
```

---

## Branches

```bash
git branch
git branch feature-name

git switch feature-name
git switch main

git switch -c feature-name

git branch -d feature-name
```

---

## Merge

```bash
git switch main
git merge feature-name
```

---

## Remote

```bash
git remote -v

git remote add origin <repository-url>

git push origin main

git pull origin main

git fetch
```

---

## Clone

```bash
git clone <repository-url>
```

---

## Undo

```bash
# Unstage
git restore --staged filename

# Discard local changes
git restore filename

# Undo local commit while keeping changes
git reset --soft HEAD~1

# Safely undo an existing commit
git revert <commit-id>
```

Use destructive commands such as:

```bash
git reset --hard
```

with extreme care.

---

## Stash

```bash
git stash
git stash list
git stash pop
```

---

## Rebase

```bash
git switch feature-branch
git rebase main
```

---

## Typical Project Workflow

```bash
git clone <repository-url>

cd project

git switch -c feature/my-feature

# Make changes

git status

git add .

git commit -m "Add my feature"

git push -u origin feature/my-feature
```

Then create a Pull Request on GitHub.
