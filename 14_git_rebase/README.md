# Git Rebase

Rebase is a Git operation used to move or replay commits onto a different base commit.

It can help create a more linear project history.

---

## Example

Suppose the history looks like:

```text
A---B---C  main
     \
      D---E  feature
```

While the feature was being developed, `main` moved forward.

Rebase can replay the feature commits on top of the latest `main`:

```text
A---B---C---D'---E'  feature
```

The feature commits are recreated on the new base.

The recreated commits have new commit identities because Git is creating new commits rather than moving the original commits.

---

## Basic Rebase

```bash
git switch feature
git rebase main
```

Git attempts to replay the feature branch commits on top of `main`.

---

## Rebase vs Merge

### Merge

A merge combines the histories of two branches.

```text
A---B---C------F
     \        /
      D---E---
```

The merge commit `F` connects the two lines of development.

### Rebase

A rebase replays commits on top of another base:

```text
A---B---C---D'---E'
```

This can produce a more linear history.

---

## Rebase Conflicts

A rebase can also produce conflicts when Git cannot automatically replay a commit.

If a conflict occurs:

```bash
git status
```

Resolve the conflicted file manually.

Then stage the resolved file:

```bash
git add <file>
```

Continue the rebase:

```bash
git rebase --continue
```

Git may stop at additional conflicts, in which case I repeat the same process.

---

## Abort Rebase

If I want to stop the rebase and return to the state before the rebase started:

```bash
git rebase --abort
```

---

## Important Warning

Rebase rewrites commit history.

Because of this, I should be careful when rebasing commits that have already been shared with other people.

A useful rule is:

> Avoid rewriting shared history unless I understand the consequences.

For example, rebasing a branch that only exists locally is generally easier to manage than rewriting commits that other contributors are already using.

---

## When Rebase Can Be Useful

Rebase can be useful when:

- Updating a feature branch with the latest `main` branch
- Keeping a cleaner linear history
- Preparing local commits before sharing them
- Incorporating the latest base changes without creating a merge commit

---

## Key Commands

```bash
# Rebase the current feature branch onto main
git rebase main

# Continue after resolving a conflict
git rebase --continue

# Cancel an in-progress rebase
git rebase --abort
```

---

## Merge vs Rebase

| Operation | What it does |
|---|---|
| `git merge` | Combines two lines of development |
| `git rebase` | Replays commits onto a new base |

---

## Mental Model

### Merge

```text
Combine histories
```

### Rebase

```text
Replay my commits on a new base
```

The goal is not to memorize which one is "better".

The important thing is understanding how each operation changes Git history.
