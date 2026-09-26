# Git Branching

A branch allows me to work on a separate line of development without
directly changing the main branch.

Branches are commonly used for:

-   New features
-   Bug fixes
-   Experiments
-   Different versions of development

------------------------------------------------------------------------

## Why Use Branches?

Suppose my main project is stable:

``` text
main
  |
  A
  |
  B
```

I want to develop a new dashboard feature.

Instead of changing `main` directly:

``` text
main
  |
  A
  |
  B
   \
    C
    |
    D
feature-dashboard
```

I can develop the feature separately and merge it later.

------------------------------------------------------------------------

## View Branches

``` bash
git branch
```

The current branch is marked with `*`.

Example:

``` text
* main
  feature-dashboard
```

------------------------------------------------------------------------

## Create a Branch

``` bash
git branch feature-dashboard
```

This creates the branch but does not switch to it.

------------------------------------------------------------------------

## Switch Branches

``` bash
git switch feature-dashboard
```

This switches my working directory to the selected branch.

------------------------------------------------------------------------

## Create and Switch to a Branch

``` bash
git switch -c feature-dashboard
```

This creates a new branch and switches to it.

This is commonly used when starting new work.

------------------------------------------------------------------------

## Delete a Branch

After merging a feature branch:

``` bash
git branch -d feature-dashboard
```

This deletes the local branch after Git determines that it has been
merged.

------------------------------------------------------------------------

## Example Workflow

``` bash
git switch -c feature-analysis

# Make changes

git add .
git commit -m "Add customer analysis"

git switch main
```

At this point, the feature work exists on the separate branch.

The feature branch can later be merged into `main`.

------------------------------------------------------------------------

## Branch Naming

I can use descriptive names such as:

``` text
feature/dashboard
feature/customer-analysis
fix/missing-values
experiment/new-model
```

Good branch names make collaboration easier because they communicate the
purpose of the branch.

------------------------------------------------------------------------

## Important Concept

A branch is not a completely separate copy of the project.

It is a movable reference to commits in Git history.

The important idea is:

``` text
main
 |
 A
 |
 B
 |
 C

feature
 |
 B
 |
 D
 |
 E
```

Both branches can share earlier commits while developing independently.

------------------------------------------------------------------------

## Branches and Commits

When I create a new branch, the new branch initially points to the same
commit as the branch I created it from.

After I make new commits, the branches can move independently.

Example:

``` text
        C  ← main
       /
A --- B
       \
        D --- E  ← feature
```

The `main` and `feature` branches share commits `A` and `B`, but then
continue independently.

------------------------------------------------------------------------

## Switching Branches

When I switch branches, Git updates the working directory to match the
selected branch.

For example:

``` bash
git switch main
```

or:

``` bash
git switch feature-analysis
```

Before switching branches, I should understand what will happen to my
current uncommitted changes.

------------------------------------------------------------------------

## Key Commands

``` bash
# List branches
git branch

# Create a branch
git branch <branch-name>

# Switch branches
git switch <branch-name>

# Create and switch to a new branch
git switch -c <branch-name>

# Delete a merged branch
git branch -d <branch-name>
```

------------------------------------------------------------------------

## Quick Reference

  Command                         Purpose
  ------------------------------- -------------------------------
  `git branch`                    List local branches
  `git branch <branch-name>`      Create a branch
  `git switch <branch-name>`      Switch to a branch
  `git switch -c <branch-name>`   Create and switch to a branch
  `git branch -d <branch-name>`   Delete a merged branch

------------------------------------------------------------------------

## Mental Model

Think of a branch as a movable pointer to a commit:

``` text
A → B → C
        ↑
       main
```

Create a feature branch:

``` text
A → B → C
        ↑
       main
        ↑
      feature
```

Make new commits on the feature branch:

``` text
A → B → C → D → E
        ↑         ↑
       main     feature
```

The branches can now develop independently until I decide to merge them.
