# Git Stash

`git stash` temporarily stores uncommitted changes so I can work on something else without creating an incomplete commit.

---

## Why Use Stash?

Suppose I am working on:

```text
feature-dashboard
```

I have made changes but they are not ready to commit.

Suddenly I need to switch to another branch.

Instead of committing unfinished work, I can temporarily stash it.

```text
Working Directory
       ↓
   git stash
       ↓
Stashed Changes
       ↓
Clean Working Directory
```

I can then switch branches and work on something else.

---

## Stash Changes

```bash
git stash
```

This temporarily stores my uncommitted changes and makes the working directory clean.

I can then switch branches:

```bash
git switch main
```

---

## View Stashes

```bash
git stash list
```

Example:

```text
stash@{0}
stash@{1}
```

The most recent stash is usually shown as `stash@{0}`.

---

## Restore Stashed Changes

```bash
git stash pop
```

This restores the most recent stash and removes it from the stash list if the operation succeeds.

---

## Apply Without Removing

```bash
git stash apply
```

This restores the most recent stash but keeps it in the stash list.

This is useful when I want to apply the same stashed changes while preserving the stash entry.

---

## Stash a Specific Message

```bash
git stash push -m "Dashboard work in progress"
```

Adding a message makes it easier to identify the purpose of a stash later.

Example:

```bash
git stash list
```

```text
stash@{0}: On feature-dashboard: Dashboard work in progress
```

---

## Delete a Stash

To remove the most recent stash:

```bash
git stash drop
```

I can also specify a particular stash:

```bash
git stash drop stash@{1}
```

---

## Example Workflow

```bash
# Working on feature
# Changes are not ready to commit

git stash

# Switch to another branch
git switch main

# Work on urgent task

# Return to the feature branch
git switch feature-dashboard

# Restore previous work
git stash pop
```

---

## `git stash pop` vs `git stash apply`

| Command | Restores Changes | Removes Stash Entry |
|---|---|---|
| `git stash pop` | Yes | Yes, if successfully applied |
| `git stash apply` | Yes | No |

---

## Important Concept

Stashing is useful for **temporary unfinished work**.

It should not replace meaningful commits when the work is ready to be recorded.

A stash is better thought of as temporary storage for work in progress.

---

## Key Commands

```bash
# Temporarily stash changes
git stash

# View all stashes
git stash list

# Restore and remove the latest stash
git stash pop

# Restore without removing the stash
git stash apply

# Create a named stash
git stash push -m "Dashboard work in progress"

# Remove the latest stash
git stash drop

# Remove a specific stash
git stash drop stash@{1}
```

---

## Mental Model

```text
Working Directory
       |
       | git stash
       ↓
 Stash Storage
       |
       | git stash pop
       ↓
Working Directory
```

The important idea is:

> `git stash` temporarily stores uncommitted work so I can change context without creating an incomplete commit.
