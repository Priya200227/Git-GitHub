# Merging and Merge Conflicts

After working on a separate branch, I may want to combine those changes
with another branch.

This is done using `git merge`.

------------------------------------------------------------------------

## Basic Merge

Suppose I have:

``` text
main
 |
 A
 |
 B

feature
 |
 C
 |
 D
```

I can merge the feature into `main`:

``` bash
git switch main
git merge feature
```

The merge combines the changes from the specified branch into the
current branch.

------------------------------------------------------------------------

## Typical Workflow

``` bash
git switch -c feature-dashboard

# Make changes

git add .
git commit -m "Add dashboard"

git switch main

git merge feature-dashboard
```

Now the feature changes are incorporated into the `main` branch.

------------------------------------------------------------------------

## Fast-Forward Merge

If the `main` branch has not changed since the feature branch was
created, Git may perform a **fast-forward merge**.

For example:

``` text
main
 |
 A
 |
 B
 |
 C
 |
 D
feature
```

In this situation, there are no new commits on `main` that need to be
combined with the feature branch.

Git can simply move the `main` branch pointer forward to the latest
commit.

------------------------------------------------------------------------

## Merge Conflict

A merge conflict occurs when Git cannot automatically combine changes
from two branches.

For example, two branches may modify the same part of a file
differently.

Git may mark the conflict like this:

``` text
<<<<<<< HEAD
Current branch version
=======
Other branch version
>>>>>>> feature
```

These are called **conflict markers**.

I need to manually decide which content should remain, or combine the
two versions if appropriate.

------------------------------------------------------------------------

## Resolving a Conflict

Typical process:

First, check the repository status:

``` bash
git status
```

Git will identify the files with conflicts.

Then:

1.  Open the conflicted file.
2.  Review both versions of the changes.
3.  Choose or combine the correct content.
4.  Remove the conflict markers.
5.  Save the file.

Then stage the resolved file:

``` bash
git add filename
```

Finally, complete the merge:

``` bash
git commit
```

------------------------------------------------------------------------

## Conflict Resolution Workflow

``` text
Merge
  ↓
Conflict detected
  ↓
Check git status
  ↓
Open conflicted file
  ↓
Choose / combine changes
  ↓
Remove conflict markers
  ↓
git add
  ↓
git commit
```

------------------------------------------------------------------------

## Abort a Merge

If I want to cancel the merge:

``` bash
git merge --abort
```

This attempts to return the repository to the state it was in before the
merge started.

------------------------------------------------------------------------

## Important Lesson

Merge conflicts are not necessarily errors in Git.

They mean Git needs human input because two sets of changes cannot be
combined automatically.

The developer must understand the intended result and resolve the
conflicting content accordingly.

------------------------------------------------------------------------

## Key Commands

``` bash
# Merge another branch into the current branch
git merge <branch>

# Check merge status and identify conflicts
git status

# Stage a resolved file
git add <resolved-file>

# Complete the merge
git commit

# Cancel an in-progress merge
git merge --abort
```

------------------------------------------------------------------------

## Mental Model

A merge combines the histories of two branches.

``` text
        C
       / \
A --- B   F
       \ /
        D---E
```

The exact history depends on how the branches developed and which merge
strategy Git uses.

The important idea is:

> `git merge` combines changes from another branch into the branch I
> currently have checked out.
