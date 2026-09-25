# Git Workflow

Git works around three important areas:

```text
Working Directory
       ↓
   git add
       ↓
Staging Area
       ↓
 git commit
       ↓
Local Repository
```

---

## 1. Working Directory

This is where I actually modify files.

Example:

```text
README.md
analysis.py
sales.csv
```

If I edit `analysis.py`, the change exists in my working directory.

Git can detect the change, but it has not been committed yet.

Check with:

```bash
git status
```

---

## 2. Staging Area

The staging area lets me choose which changes should be included in the next commit.

```bash
git add analysis.py
```

Now `analysis.py` is staged.

I can stage multiple files:

```bash
git add analysis.py README.md
```

Or all changed files:

```bash
git add .
```

---

## 3. Local Repository

After staging, I create a commit:

```bash
git commit -m "Add sales analysis"
```

The commit becomes part of the local Git history.

---

## Complete Workflow

```bash
# Check current state
git status

# Stage changes
git add .

# Save snapshot
git commit -m "Update sales analysis"

# View history
git log --oneline
```

---

## Why Staging Exists

Suppose I modified:

```text
analysis.py
README.md
dashboard.pbix
```

But I only want to commit the Python changes.

I can do:

```bash
git add analysis.py
git commit -m "Update analysis logic"
```

The other files remain outside the commit.

This is why the staging area is useful.

---

## Working Directory → Staging → Repository

```text
         edit
          │
          ▼
┌─────────────────────┐
│ Working Directory   │
└─────────┬───────────┘
          │
       git add
          │
          ▼
┌─────────────────────┐
│ Staging Area        │
└─────────┬───────────┘
          │
      git commit
          │
          ▼
┌─────────────────────┐
│ Local Repository     │
└─────────────────────┘
```

---

## GitHub Is a Separate Step

A local commit does NOT automatically appear on GitHub.

I need to push it:

```bash
git push origin main
```

So the complete workflow is:

```text
Edit
 ↓
git status
 ↓
git add
 ↓
git commit
 ↓
git push
 ↓
GitHub
```

---

## Important Distinction

### `git add`

Prepares changes for the next commit.

### `git commit`

Records staged changes in the local repository.

### `git push`

Uploads local commits to a remote repository such as GitHub.

These three commands perform different jobs.

